---
layout: page
title: Devops
permalink: /devops/
is_menu_entry: true
position: left
order: 9000
---

## Table of Contents

- [Introduction](#introduction)
- [Environments](#environments)
- [Deployment architecture](#deployment-architecture)
    - [Deployment repository](#deployment-repository)
    - [Base paths](#base-paths)
- [Avenirs Workflows](#avenirs-workflows)
- [Self-hosted runners](#self-hosted-runners)
    - [Key principles](#key-principles)
    - [Installation](#installation)
    - [Remove / Unregister](#remove--unregister)
- [GitHub Actions deployments](#github-actions-deployments)
    - [Reusable workflow](#reusable-workflow)
    - [Dev deployments](#dev-deployments)
    - [Qualif deployments](#qualif-deployments)
    - [Recette deployments](#recette-deployments)
- [Tagging strategy](#tagging-strategy)
- [Secrets and access](#secrets-and-access)

## Introduction[⇧](#table-of-contents)

This page centralizes everything related to **DevOps**: environments, runners, and how we deploy through **GitHub Actions**.
The objective is to keep **environment parity**, minimize manual steps, and make deployments **reproducible**.

---

## Environments[⇧](#table-of-contents)

We operate **three environments** with the **same architecture**:

- **srv-dev** (development)
- **srv-qualif** (qualification)
- **srv-recette** (recette)

Common traits:

- SSH access is only allowed **over VPN** (OpenVPN). The `.ovpn` client configuration is stored in **Vaultwarden**.
- You need an SSH keypair (ed25519) to access the servers (public key stored on server).
- All servers share the same base path under:

```
/opt/avenirs-dev/
```

---

## Deployment architecture[⇧](#table-of-contents)

### Deployment repository

Deployments are orchestrated by the repository:

- [`avenirs-esr/avenirs-deployment`](https://github.com/avenirs-esr/avenirs-deployment){:target="_blank"}

On each server, this repository is present at:

```
/opt/avenirs-dev/avenirs-deployment
```

Key elements:

- `conf/deployment.conf` — central configuration that selects **branches/tags** to deploy per repository.
- `conf/tags.conf` — used by the CI/CD pipeline to determine the target **tags for each repository** and automatically update `deployment.conf` on the target server before triggering the deployment.
- `scripts/` — server-side scripts that actually run the deployment sequence.

The main entry points are:

- `scripts/deploy_srv-dev.sh`
- `scripts/deploy_srv-qualif.sh`
- `scripts/deploy_srv-recette.sh`

> The full step-by-step deployment sequence is described in the `avenirs-deployment` README.

### Base paths

- `/opt/avenirs-dev/` — base path for deployment artifacts.
- `/opt/avenirs-dev/avenirs-deployment/` — orchestration scripts + deployment config.
- `/opt/avenirs-dev/srv-dev/` — environment-specific deployment working copies (repos, compose stacks, etc.).

---

## Avenirs Workflows

The repository `avenirs-workflows` centralizes reusable workflows and
composite GitHub Actions.

Purpose:

-   Avoid duplication
-   Standardize CI/CD
-   Encapsulate deployment logic
-   Accelerate new pipeline creation

Repositories call centralized workflows using:

    uses: avenirs-esr/avenirs-workflows/.github/workflows/<workflow>.yaml@main

Repository structure:

    avenirs-workflows/
    ├── .github/
    │   ├── actions/
    │   └── workflows/
    ├── gh-page-template/
    ├── trivy-template/
    ├── CHANGELOG.md
    └── README.md

Benefits:

-   Single source of truth
-   Clean repositories
-   Faster CI/CD evolution
-   Safer workflow refactoring


---

## Self-hosted runners[⇧](#table-of-contents)

We use a **self-hosted GitHub Actions runner** on our servers so jobs run **inside our network** (close to Docker, databases, private services, etc.).

#### Key principles

- The runner process runs as a **local Unix user** (we use `$SRV_RUNNER_USER`), managed as a **systemd service**.
- For deployments to succeed **non-interactively**, the runner user must:
    - Have **no sudo prompts** (NOPASSWD for the necessary commands).
    - Have the **same effective rights as `$SRV_USER`** on the deployment tree (or share a common group).
    - Belong to the **`docker`** group (or run Docker via `sudo` NOPASSWD).
    - Optionally, be part of a shared **`deploy`** group that owns `/opt/avenirs-dev` (group write, `setgid`).

#### Installation

> Variables you will use below:
> - `token` — the registration token from GitHub (Org → Settings → Actions → Runners → “New self-hosted runner”).
> - `name` — runner name (e.g. `srv-dev`).
> - `UNIT` — resolved systemd unit name (see command below).
> - `REG_TOKEN` — removal token (new token generated for removal if needed).

```bash
# As root (or a sudo-capable user)
sudo useradd --system --create-home --home-dir /opt/$SRV_RUNNER_USER --shell /bin/bash $SRV_RUNNER_USER 2>/dev/null || sudo useradd -m -d /opt/$SRV_RUNNER_USER -s /bin/bash $SRV_RUNNER_USER

sudo mkdir -p /opt/$SRV_RUNNER_USER
sudo chown -R $SRV_RUNNER_USER:$SRV_RUNNER_USER /opt/$SRV_RUNNER_USER

# Optional (recommanded): shared group ownership for the base path
sudo groupadd -f deploy
sudo usermod -aG deploy $SRV_RUNNER_USER
sudo usermod -aG deploy $SRV_USER
sudo chgrp -R deploy /opt/avenirs-dev
sudo chmod -R g+rwX /opt/avenirs-dev
sudo find /opt/avenirs-dev -type d -exec chmod g+s {} \;

# Allow $SRV_RUNNER_USER passwordless sudo for everything (use with caution)
sudo bash -c 'cat >/etc/sudoers.d/99-$SRV_RUNNER_USER-all <<EOF
Defaults:$SRV_RUNNER_USER !requiretty
Defaults:$SRV_RUNNER_USER env_keep += "JASYPT_ENCRYPTOR_PASSWORD JAVA_HOME M2_HOME NVM_DIR SDKMAN_DIR NODE_OPTIONS"
$SRV_RUNNER_USER ALL=(ALL) NOPASSWD:ALL
EOF'
sudo chmod 0440 /etc/sudoers.d/99-$SRV_RUNNER_USER-all

# Docker access (either add to group or use sudo for docker)
sudo usermod -aG docker $SRV_RUNNER_USER

# Switch to runner user and configure
sudo -iu $SRV_RUNNER_USER
mkdir -p ~/actions-runner && cd ~/actions-runner

# Before running following command, go on github, create new self-hosted runner (select the correct runner image) and execute download parts commands (except the first mkdir) 
./config.sh --url https://github.com/avenirs-esr   --token "$token"   --name "$name"   --runnergroup Default   --work _work   --labels self-hosted,Linux,X64,"$name"

./run.sh
# Ctrl+C to stop foreground runner

```
**Environment variable (already in $SRV_RUNNER_USER):**

```bash
# Install all dependencies that $SRV_RUNNER_USER will need (Java, Maven, Node).
curl -s "https://get.sdkman.io" | bash
source "$HOME/.sdkman/bin/sdkman-init.sh"
sdk install java 21.0.4-oracle
sdk default java 21.0.4-oracle
sdk install maven 3.9.9
sdk default maven 3.9.9

export NVM_DIR="$HOME/.nvm"
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
  . "$NVM_DIR/nvm.sh"
nvm install --lts
nvm alias default "lts/*"

echo "export JASYPT_ENCRYPTOR_PASSWORD='your-secret-value'" >> ~/.bashrc

```
**Git configuration (already in $SRV_RUNNER_USER):**

```bash
git config --global --add safe.directory /opt/avenirs-dev/avenirs-deployment
git config --global --add safe.directory /opt/avenirs-dev/srv-dev
git config --global --add safe.directory /opt/avenirs-dev/srv-dev/services/apisix/apisix-docker
git config --global --add safe.directory /opt/avenirs-dev/srv-dev/services/apisix/apisix-dashboard
git config --global --add safe.directory /opt/avenirs-dev/srv-dev/services/avenirs-portfolio/avenirs-portfolio-security
git config --global --add safe.directory /opt/avenirs-dev/srv-dev/services/avenirs-portfolio/avenirs-cofolio-client
git config --global --add safe.directory /opt/avenirs-dev/srv-dev/services/avenirs-portfolio/avenirs-cofolio-api
git config --global --add safe.directory /opt/avenirs-dev/srv-dev/services/cas/cas-overlay-template

exit

```
**Install as a service and start:**

```bash
cd /opt/$SRV_RUNNER_USER/actions-runner
sudo ./svc.sh install $SRV_RUNNER_USER
sudo ./svc.sh start

# Locate and enable the unit
systemctl list-unit-files --type=service | grep '^actions\.runner'
# Example export (adjust if needed):
# export UNIT="actions.runner.***.***.service"
sudo systemctl enable "$UNIT"
systemctl status "$UNIT"
```


#### Remove / Unregister

```bash
# Stop & uninstall service
cd /opt/$SRV_RUNNER_USER/actions-runner
sudo ./svc.sh stop
sudo ./svc.sh uninstall
sudo systemctl daemon-reload

# Remove runner registration (as $SRV_RUNNER_USER)
sudo -iu $SRV_RUNNER_USER
cd /opt/$SRV_RUNNER_USER/actions-runner
./config.sh remove --token "$REG_TOKEN"
```

> **Troubleshooting**
> - **Docker socket**: “Permission denied” → add `$SRV_RUNNER_USER` to **`docker`** and restart the service.
> - **Git dubious ownership** → ensure files under `/opt/avenirs-dev` are owned by `$SRV_RUNNER_USER:deploy` (if a deploy group axist), avoid root-owned files.
> - **All tools** not found → ensure environment variables are preserved when using `sudo` (e.g. `JAVA_HOME`, `M2_HOME`, `NVM_DIR`, `SDKMAN_DIR`, etc.).

---

## GitHub Actions deployments[⇧](#table-of-contents)

### Reusable workflow

Deployments are driven by a shared **reusable workflow** (called from repositories via `uses:`).

Typical characteristics:

- Centralizes the “how to deploy”
- Keeps repos light (only event wiring + inputs)
- Runs on the correct self-hosted runner (srv-dev / srv-qualif / srv-recette)
- Delegates the actual deployment to `avenirs-deployment` server scripts

### Dev deployments

Development is deployed automatically.

Trigger condition:
- Every time a Pull Request is merged into a `develop` branch
- Only for repositories that have a direct impact on the application (API, back-office, client, etc.)

Process:
- A GitHub Actions workflow is triggered on merge to `develop`
- The deployment job runs on the **srv-dev** self-hosted runner
- The server-side deployment script from `avenirs-deployment` is executed
- Docker images and containers are refreshed accordingly

This ensures that **srv-dev always reflects the latest validated develop state**.

---

### Qualif deployments

Qualification follows a controlled continuous deployment process driven by User Stories (US).

#### 1. Functional readiness

When all sub-tasks of a User Story are merged into `develop`:

- A workflow automatically detects that the US is complete
- The US is moved to the **“In Review”** column in GitHub Projects

This column represents:
> “Ready for qualification deployment”

---

#### 2. Nightly deployment workflow

Every night, a scheduled workflow runs.

Step 1 — Check pending US
- The workflow checks if there are User Stories in the **“In Review”** column.
- If none → the workflow stops.
- If some exist → the workflow continues.

Step 2 — E2E validation on develop
- End-to-end tests are executed against the current `develop` state.
- If tests fail:
    - A comment is automatically added on each US in “In Review”
    - Deployment is aborted.

Step 3 — Deployment to qualif  
If E2E tests pass:
- The deployment workflow runs on **srv-qualif**
- The deployment is executed via `avenirs-deployment`

If deployment fails:
- A comment is automatically added on the affected US.

Step 4 — Move to Recette  
If deployment succeeds:
- All User Stories that were in “In Review”
- Are automatically moved to the **“Recette”** column

This guarantees:
- No deployment without validated E2E tests
- Atomic qualification batches
- Automatic traceability on failure

---

### Recette deployments

Recette deployments can be:

- Manual
- Or automatically triggered by a release

#### Automatic deployment

Triggered when:
- A new GitHub Release is created on `srv-dev` repository
- The tag contains `-rc` (e.g. `1.2.3-rc`)

Process:

1. The deployment workflow reads `conf/tags.conf` from the `avenirs-deployment` repository.
2. `tags.conf` defines the exact tag to use for each service.
3. The workflow updates the server-side `deployment.conf` accordingly.
4. The deployment command is executed on **srv-recette**.
5. Docker images are updated and containers restarted.

This ensures:
- Full control over service versions
- Cross-service version consistency

---

## Tagging strategy[⇧](#table-of-contents)

We use **Semantic Versioning (SemVer)** as our versioning strategy:
`MAJOR.MINOR.PATCH`

Version increment rules:

- **MAJOR** — significant milestone or public version step  
  (e.g. initial V0 release, future V1, V2, etc.)

- **MINOR** — sprint release version  
  Represents a functional increment delivered at the end of a sprint.

- **PATCH** — bug fixes or small corrective updates  
  Backward-compatible fixes applied on top of a sprint version.

---

### Environment-specific tags

Based on SemVer, we use the following conventions:

- **Qualif**: `X.Y.Z-qa`  
  GitHub Release marked as **pre-release**

- **Recette**: `X.Y.Z-rc`  
  GitHub Release marked as **pre-release**

- **Production**: `X.Y.Z`  
  Final GitHub Release (not pre-release)

Examples:

- `1.4.2-qa`
- `1.4.2-rc`
- `1.4.2`

---

## Secrets and access[⇧](#table-of-contents)

Secrets are stored in:

- GitHub Secrets (workflow execution)
- Vaultwarden (server access, VPN, SSH keys, deployment credentials)

Commonly used secrets/variables:

- `SRV_USER`
- `SRV_RUNNER_USER`
- `SRV_BASE_PATH`
- `SRV_DEPLOY_CMD` / `SRV_UPDATE_DEPLOYMENT_CMD` (if used by the deployment composite/action)
- `JASYPT_ENCRYPTOR_PASSWORD`
- VPN `.ovpn` file and SSH keys

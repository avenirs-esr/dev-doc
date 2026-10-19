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
- [Node.js version upgrades](#nodejs-version-upgrades)
    - [Scope and prerequisites](#scope-and-prerequisites)
    - [Align local project configuration](#align-local-project-configuration)
    - [Align shared workflows](#align-shared-workflows)
    - [Align Docker builders in srv-dev](#align-docker-builders-in-srv-dev)
    - [Validate the migration](#validate-the-migration)
    - [Merge and rollout order](#merge-and-rollout-order)
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

## Node.js version upgrades[⇧](#table-of-contents)

A Node.js upgrade must keep **local development, CI jobs, self-hosted deployment commands, and Docker builders** aligned. Changing `.nvmrc` alone does not update the other execution environments.

The examples below use the **Node.js 22 → 24 migration**. For a future upgrade, replace the target major consistently and recheck tooling compatibility. This procedure covers runtime migration, **not a general dependency upgrade or a deployment-process redesign**.

### Scope and prerequisites

Coordinate the migration across these repositories:

| Repository | Responsibility |
|---|---|
| `avenirs-dsav` | Shared Vue components, library build and declarations, Storybook, documentation, and npm publication. |
| `avenirs-cofolio-client` | Cofolio application, local tooling, quality checks, accessibility, and E2E tests. |
| `avenirs-cofolio-backoffice-client` | Backoffice application, local tooling, quality checks, and accessibility. |
| `avenirs-workflows` | Shared Node setup, reusable workflows, composite actions, and server-side runtime selection. |
| `srv-dev` | Docker builder images, image-version defaults, and application build contexts. |

Before editing:

1. Record the current branch and working-tree changes in each repository. Preserve existing work and keep migration changes identifiable.
2. Read each `package.json` for its actual scripts and inspect all runtime selectors, workflow callers, and Docker build arguments.
3. Agree on the target Node major and the minimal tooling changes needed to support it. Do not run `npm update` or `npm audit fix` as part of the runtime migration.
4. Prepare coordinated changes without merging an application change that would immediately deploy through an older builder.

### Align local project configuration

Apply this checklist independently to **DSAV, Cofolio, and Cofolio Backoffice**:

| File | Node 24 configuration / check |
|---|---|
| `.nvmrc` | Set the selected major to `24`. |
| `.npmrc` | Set `engine-strict=true` so installation enforces declared engine requirements. |
| `package.json` | Declare `"engines": { "node": ">=24.0.0" }`. |
| `package-lock.json` | Synchronize the root engine metadata and any explicitly approved tooling changes; preserve unrelated resolutions. |
| `tsconfig.node.json` | Extend `@tsconfig/node24/tsconfig.json` and ensure that preset is declared and installed. |
| Node type definitions | Check that the selected `@types/node` version is appropriate for the APIs used; review any update explicitly. |

Also inspect nested packages, such as Cofolio's `e2e/`, and any existing alternative runtime selectors. Do not introduce redundant version files.

Select the runtime before validating:

```bash
nvm install
nvm use
node --version
npm --version
```

### Align shared workflows

In `avenirs-workflows`:

1. Update `.github/actions/setup-node/action.yaml` to select Node `24`. Keep the existing npm pin unless a separate compatibility change is required.
2. Align the existing `nvm install` / `nvm use` commands in `.github/actions/deploy-on-server/action.yaml`.
3. Search all other actions and workflows for independent Node pins, NVM commands, and Node-based container images. Include project/wiki automation, quality checks, coverage, accessibility, E2E tests and report merging, docs, builds, and publication.
4. Ensure **each job** that executes Node commands sets up its own runtime. A job's `needs` dependency does not share another job's filesystem or `PATH`.
6. Preserve existing triggers, permissions, conditions, installation flags, and deployment behavior.

DSAV, Cofolio, and Backoffice normally consume shared workflows through `@main`. If testing an unpublished migration branch, temporarily update the relevant caller refs **and inspect nested shared-action refs**: a workflow loaded from a feature branch can still call `setup-node@main` internally.

Confirm the selected Node version in the actual CI logs. A green check or a workflow name does not prove which runtime was used. GitHub's internal JavaScript-action runtime is also distinct from the Node runtime used by the application's build commands.

### Align Docker builders in srv-dev

For the Cofolio builder, update both sources of the image version:

- `services/avenirs-portfolio/Dockerfile-avenirs-cofolio-client`: `ARG NODE_ALPINE_VERSION=24-alpine`.
- `scripts/srv-dev-env.sh`: `NODE_ALPINE_VERSION_DEFAULT="24-alpine"`.

Trace the complete chain: **source default → bootstrap-generated environment → Compose build argument → Dockerfile `FROM`**. Compose can override the Dockerfile default, so changing only the Dockerfile is insufficient. Do not hand-edit generated `.env` files or expose their secrets; use the existing regeneration process during the planned rollout.

Audit the Backoffice build/deployment path as well. Locate its actual Dockerfile, source checkout, and runtime arguments rather than assuming that it shares Cofolio's builder. Leave Node images used by unrelated services unchanged unless they are explicitly in scope.

For a build-only verification:

- Check the actual frontend ref inside the `srv-dev` submodule used as the build context; a standalone checkout or infrastructure branch does not select that ref.
- Build the existing Dockerfile directly with the target Node image argument and a dedicated local verification tag. Use the environment files required by the normal build; do not fabricate configuration.
- Confirm installation, type checking, lint, and asset generation succeed, then validate the final image. Inspect Node/npm in the **builder stage**, not in the final nginx image.
- Record the tested source revision and image architecture. An ARM64 build does not validate AMD64.

### Validate the migration

Run checks with the selected Node in each repository. Use its current manifest as the source of truth for script names.

| Scope | Checks |
|---|---|
| DSAV, Cofolio, Backoffice | Clean installation, `npm run type-check`, `npm run lint`, `npm run test:unit`, `npm run test:coverage`, `npm run build`, and `npm run build:development`. |
| Accessibility | Build with `npm run build-only:development`, then run `npm run test:a11y`; confirm the preview server serves this checkout's fresh build. |
| DSAV | Run the **full** package build, including declaration generation and alias rewriting; also run `npm run storybook:build` and `npm run docs:build`. |
| Cofolio E2E package | Install dependencies in `e2e/`, install the required browsers, and run the applicable E2E scenarios and report-merging path. |
| Shared CI | Validate workflow syntax and inspect completed quality, test, accessibility, and applicable E2E jobs for target-runtime evidence. |
| srv-dev | Exercise the real Docker build and check the resulting image without deploying it. |

### Merge and rollout order

Coordinate merges and releases in this order:

1. **Shared workflows:** merge the validated runtime changes to `avenirs-workflows/main`.
2. **Permanent workflow refs:** restore temporary consumers and nested shared refs to `@main`; rerun checks on the final application and DSAV revisions and verify Node in the logs.
3. **DSAV:** merge to `develop`, then publish through the normal release process. Verify the package and documentation publication; do not trigger a real release merely as a smoke test.
4. **srv-dev:** make the validated Docker/default-version changes available through the existing infrastructure rollout **before deploying either migrated application**. Check the branch/tag actually selected on the server: merging infrastructure to `develop` does not update a server pinned to another ref.
5. **Cofolio and Backoffice:** if a new DSAV release is required, explicitly update each consumer's dependency/lockfile and revalidate. Merge the applications only once their CI and deployment builders use the target runtime, then verify the deployed revisions and basic frontend/API behavior.

Before closing the migration, confirm that no temporary workflow refs remain, runtime settings agree across all five repositories, required checks and image builds have passed, and documentation reflects the selected version. Keep unrelated dependency upgrades and deployment redesigns out of the migration.

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
- Check if the US have a "[TEST]" sub-issue
- The US is moved to the **“In Review”** column in GitHub Projects

This column represents:
> “Ready for qualification deployment”

If the US dont have a "[TEST]" sub-issue, a comment is automatically added to the US to notify the team that it cannot be qualified until test cases are defined, and the US still in "In Progress" column.

> When a sub-issue is added to a US that has been completed, regardless of which column the US is in, it is automatically moved to “Backlog Ready” column

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

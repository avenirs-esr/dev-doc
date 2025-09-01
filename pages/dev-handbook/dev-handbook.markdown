---
layout: page
title: Dev Handbook
permalink: /dev-handbook/
is_menu_entry: true
position: left
order: 4000
---

## Table of Contents

- [Introduction](#introduction)
- [Backend](#backend)
  - [Languages and Frameworks](#languages-and-frameworks)
  - [Tools](#tools)
    - [Sdkman: Java & Maven version management](#sdkman-java--maven-version-management)
    - [Nvm: Node version management](#nvm-node-version-management)
  - [Developer's agreements](#developers-agreements)
    - [Code style](#code-style)
    - [Performance](#performance)
- [Frontend](#frontend)
- [Git](#git)
  - [Git workflow](#git-workflow)
    - [Branch naming convention](#branch-naming-convention)
  - [Versioning (SemVer)](#versioning-semver)
  - [Conventional commits](#conventional-commits)
  - [Commitlint & husky](#commitlint--husky)
- [Clean Code Checklist](#clean-code-checklist-cf-uncle-bob)
- [Development environment](#development-environment)
- [Deployment](#deployment)
  - [Environments](#environments)
  - [Self-hosted runners](#self-hosted-runners)
  - [Continuous Deployment](#continuous-deployment)
- [TODOs](#todos)

## Introduction[⇧](#table-of-contents) 
This section provides an overview of the development practices and tools used in the project: GitHub Flow, versioning, frameworks, CLI tools, and more.<br/>
The project follows a microservice and feature-based architecture to keep the codebase modular and scalable.

## Backend[⇧](#table-of-contents) 

### Languages and Frameworks
- [Java 21](https://endoflife.date/eclipse-temurin){:target="_blank"}
- [Maven 3.9](https://endoflife.date/maven){:target="_blank"}
- [Node.js 22 (latest LTS at 2025-04)](https://endoflife.date/nodejs){:target="_blank"}
- NestJS 10: latest major version (no LTS policy) 
- [Spring boot 3.3](https://endoflife.date/spring-boot){:target="_blank"}

### Tools

#### Sdkman: Java & Maven version management

[Sdkman](https://sdkman.io/){:target="_blank"} can help to manage the version of different tools.

**Installation:**
```
curl -s "https://get.sdkman.io" | bash
```

**Switching to the required version can be automated:**
```
cat $HOME/.sdkman/etc/config
sdkman_auto_env=true
```
**Note:**To run skdman (you need to restart your terminal after the installation).

**Exemple of configuration file: .sdkmanrc**
```
cat .sdkmanrc
# Enable auto-env through the sdkman_auto_env config
# Add key=value pairs of SDKs to use below
java=21.0.4-oracle
maven=3.9.8  # Ajoute apres
```

**To run skdman (you need to restart your terminal after the installation):**
```
sdk env install
```
 
#### Nvm: Node version management**

[Nvm](https://github.com/nvm-sh/nvm) can help to manage node.js version.<br/>
Switching to the target version can be automated by adding this snippet in .bashrc file.

**Exemple of configuration file: .nvmrc**
```
cat .nvmrc 
v22.14.0
```

**Bash sample to automate the version switch:**
``` 
autoload_nvmrc() {
  local nvmrc_path=$(nvm_find_nvmrc)
  
  if [ -n "$nvmrc_path" ]; then
    local nvmrc_node_version=$(cat "$nvmrc_path")
    # If the current Node version is different from the one in .nvmrc, switch versions
    if [ "$(nvm version)" != "$nvmrc_node_version" ]; then
      nvm use
    fi
  fi
}

PROMPT_COMMAND="autoload_nvmrc; $PROMPT_COMMAND"

```

### Developer's agreements

#### Code style
The code in java should respect the [Google Java Format](https://github.com/google/google-java-format).<br/>
- To check if the code is well formatted, you can use the ``mvn spotless:check``
- To format the code, you can use the ``mvn spotless:apply``

A naming convention for database tables has been adopted, with the aim of ensuring that table names are always in the singular.

Please respect the hexagonal architecture principles, which means that the code should be organized by feature and not by technical layer. This allows for better modularity and easier maintenance.

#### Performance
Try to optimize requests and responses, to lighten processing in services and limit the amount of non-essential data loaded into memory. For that, use DTOs projection and pagination when possible.

Use FetchType to limit the amount of data loaded in memory. For example, use FetchType.LAZY to load data only when needed, and avoid loading entire collections or entities unless necessary.

## Frontend[⇧](#table-of-contents) 

- [Vue.js 3](https://endoflife.date/vue){:target="_blank"}

TODO: [complete the list of libraries eg. Pinia, VueQuery, etc.](../arch-front/){:target="_blank"}


## Git[⇧](#table-of-contents) 

### Git workflow
The selected git flow is [GitHub Flow.](https://docs.github.com/en/get-started/quickstart/github-flow){:target="_blank"}

#### Branch naming convention
We use the following branch naming convention: <type>/<issue-number>-short-description

**Examples:** 
- feat/123-add-login
- fix/456-fix-header

### Versioning (SemVer)
The versioning strategy is [Semantic Versioning.](https://semver.org/#semantic-versioning-200){:target="_blank"}

>   Given a version number MAJOR.MINOR.PATCH, increment the:
>
>    MAJOR version when you make incompatible API changes\
>    MINOR version when you add functionality in a backward compatible manner\
>    PATCH version when you make backward compatible bug fixes

### Conventional commits
The commit messages should respect the [Conventional Commits specification.](https://www.conventionalcommits.org/en/v1.0.0/){:target="_blank"}\
This specification defines at least 2 types of messages, feat, fix and the notion of Breaking change. \
This allow to automate the SemVer management:
     
    fix             -> patch
    feat            -> minor
    Breaking change -> major

### Commitlint & husky
In order to facilitate the commit with command line, [commitlint](https://github.com/conventional-changelog/commitlint){:target="_blank"} can be used in conjunction to [husky](https://typicode.github.io/husky/){:target="_blank"} (client side git hooks).    

## Clean Code Checklist (cf Uncle Bob)[⇧](#table-of-contents) 

1. Name things clearly: use explicit and meaningful names, void obscure abbreviations or acronyms.

2. Keep functions short: keep it small, ideally between 5 and 15 lines.

3. Cohesion and separation of concerns: respect the **SRP** (*Single Responsibility Principle*).

4. Remove dead code

5. Avoid unnecessary comments: code should be **self-explanatory**; comments should explain the **why**, not the **how**.

6. Clear error handling: no silent failures, use specific and explicit exceptions.

7. Consistent formatting(cf linter section).

8. Automated testing: clean code is **testable**, ensure reasonable coverage.

9. Avoid side effects: prefer pure functions, minimize implicit state mutations.

10. Regular refactoring: refactoring is part of the development cycle (Use barrels for ts).

## Development environment[⇧](#table-of-contents) 
The development environment can be installed with docker compose. It deploys the services needed for the development: authentication, databases, API Manager, etc.
- [Repository: srv-dev](https://github.com/avenirs-esr/srv-dev){:target="_blank"}
- [Installation notes: Readme](https://github.com/avenirs-esr/srv-dev/tree/main#readme){:target="_blank"}

## Deployment[⇧](#table-of-contents)

This section explains how we deploy across our environments, how our self-hosted GitHub runners are set up, and how Continuous Deployment (CD) ties it all together.

> Variables that you will find in Vaultwarden:
> - `SRV_USER` — The Unix user that owns the deployment tree with sudo rights
> - `SRV_RUNNER_USER` — The Unix user that runs the self-hosted GitHub Actions runner
---

### Environments[⇧](#table-of-contents)

We operate **three environments** with the **same architecture**:

- **srv-dev** (development)
- **srv-qualif** (qualification)
- **srv-recette** (recette)

**Common traits**

- All servers share the same Unix user: **see in Vaultwarden**.
- SSH access is only allowed **over VPN** (OpenVPN).  
  The `.ovpn` client configuration is stored in **Vaultwarden**.
- You need an SSH keypair (ed25519) to access the servers (public key stored in server).
- Each server hosts:
  - **`/opt/avenirs-dev/`** — base path for deployment artifacts.
  - **`/opt/avenirs-dev/avenirs-deployment`** — the deployment repository [see repository](https://github.com/avenirs-esr/avenirs-deployment):
    - Config file: **`/conf/deployment.conf`** (selects the **branches/tags** to target for each repo during a deploy).
    - Scripts in **`/scripts`** — the main entry point to trigger a deployment is **`deploy_srv-dev.sh`**.
    - The detailed deployment sequence is described in the **README** of `avenirs-deployment` [see repository](https://github.com/avenirs-esr/avenirs-deployment).

> **Note:** The same repo & layout exist on **srv-dev**, **srv-qualif**, and **srv-recette** to keep parity.

---

### Self-hosted runners[⇧](#table-of-contents)

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

# Switch to runner user and configure
sudo -iu $SRV_RUNNER_USER
mkdir -p ~/actions-runner && cd ~/actions-runner
./config.sh --url https://github.com/avenirs-esr   --token "$token"   --name "$name"   --runnergroup Default   --work _work   --labels self-hosted,Linux,X64,"$name"

./run.sh
# Ctrl+C to stop foreground runner
exit

# Install as a service and start
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

**Sudo & groups (non-interactive runs):**

```bash
# Allow $SRV_RUNNER_USER passwordless sudo for everything (use with caution)
sudo bash -c 'cat >/etc/sudoers.d/99-$SRV_RUNNER_USER-all <<EOF
Defaults:$SRV_RUNNER_USER !requiretty
Defaults:$SRV_RUNNER_USER env_keep += "JASYPT_ENCRYPTOR_PASSWORD JAVA_HOME M2_HOME NVM_DIR SDKMAN_DIR NODE_OPTIONS"
$SRV_RUNNER_USER ALL=(ALL) NOPASSWD:ALL
EOF'
sudo chmod 0440 /etc/sudoers.d/99-$SRV_RUNNER_USER-all

# Docker access (either add to group or use sudo for docker)
sudo usermod -aG docker $SRV_RUNNER_USER
# Restart the runner service so new group membership is picked up
sudo systemctl restart "$UNIT"

# Optional: shared group ownership for the base path
sudo groupadd -f deploy
sudo usermod -aG deploy $SRV_RUNNER_USER
sudo usermod -aG deploy $SRV_USER
sudo chgrp -R deploy /opt/avenirs-dev
sudo chmod -R g+rwX /opt/avenirs-dev
sudo find /opt/avenirs-dev -type d -exec chmod g+s {} \;
```

**Environment variable (example):**

```bash
# Jasypt secret for deployments (only for the account that runs deployments)
echo "export JASYPT_ENCRYPTOR_PASSWORD='your-secret-value'" >> ~/.bashrc
# Or provide via GitHub Secrets to the workflow and preserve via sudo if needed:
# sudo --preserve-env=JASYPT_ENCRYPTOR_PASSWORD ...
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

### Continuous Deployment[⇧](#table-of-contents)

CD is driven by:
- A **shared reusable workflow** `deploy-workflow` that selects the target environment and triggers the deployment.
- A **lightweight composite action** that asserts the tag is a **pre-release** (`-qa` or `-rc`) and validates that tag exists in all dependent repositories.
- A **deployment action** that runs the server-side script (`/opt/avenirs-dev/avenirs-deployment/scripts/deploy_srv-dev.sh`).

#### How the common `deploy-workflow` behaves

- **Development**  
  Triggers **on every push** to a `develop` branch in the relevant repositories (**portfolio-api** and **cofolio-client**).  
  The job runs on the shared self-hosted runner on **srv-dev**.

- **Qualification / QA (srv-dev)**  
  Triggers **when a GitHub *pre-release*** is **published** with a tag that **contains `-qa`**.  
  Before deploying, the workflow **verifies the tag exists** in the repositories **portfolio-api** and **cofolio-client**.  
  The job runs on the shared self-hosted runner on **srv-qualif**.

- **Recette / RC (srv-dev)**  
  Triggers **when a GitHub *pre-release*** is **published** with a tag that **contains `-rc`**.  
  Before deploying, the workflow **verifies the tag exists** in **portfolio-api** and **cofolio-client**.  
  The job runs on the shared self-hosted runner on **srv-recette**.

> Tip: create the tag in **both** repositories first; the cross-repo check will fail fast if a tag is missing.

#### Tagging convention (summary)

- **Qualif**: `X.Y.Z-qa` (GitHub Release marked **pre-release**)
- **Recette**: `X.Y.Z-rc` (GitHub Release marked **pre-release**)
- **Production**: `X.Y.Z` (final release, **not** pre-release)



## TODOs[⇧](#table-of-contents) 
### Features templates by framework
- Spring-Boot
    - https://docs.spring.io/spring-boot/reference/using/structuring-your-code.html
    - https://github.com/Muddz/StructurebyFeatureTmpl
- NodeJS NestJS
- Vue.js
<br>
**Note:** for typescript use barrels to facilitate refactoring.
  
### Linters by technos - TODO Fix configurations and tooling.
- Java: https://github.com/google/google-java-format
- NodeJS: ESLint + config TypeScript
- NestJS: ESLint + config TypeScript + NestJS


- Plugins: @typescript-eslint, eslint-plugin-import, eslint-plugin-prettier
- https://github.com/prettier/prettier
- Vue.js: eslint-plugin-vue
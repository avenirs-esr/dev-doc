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
  - [Frontend stack](#frontend-stack)
  - [Cofolio monorepo](#cofolio-monorepo)
  - [Application architecture](#application-architecture)
  - [Shared package](#shared-package)
  - [API client and HTTP configuration](#api-client-and-http-configuration)
  - [Local frontend development](#local-frontend-development)
  - [Frontend testing and quality](#frontend-testing-and-quality)
  - [DSAV design system](#dsav-design-system)
  - [Where to make a frontend change](#where-to-make-a-frontend-change)
- [Git](#git)
  - [Git workflow](#git-workflow)
    - [Branch naming convention](#branch-naming-convention)
    - [Definition of done (DoD)](#definition-of-done-dod)
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
- [Java 25](https://endoflife.date/eclipse-temurin){:target="_blank"}
- [Maven 3.9](https://endoflife.date/maven){:target="_blank"}
- [Node.js 26](https://endoflife.date/nodejs){:target="_blank"}
- NestJS 10: latest major version (no LTS policy) 
- [Spring boot 4.1](https://endoflife.date/spring-boot){:target="_blank"}

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

For a coordinated Node.js upgrade across DSAV, Cofolio, Cofolio Backoffice, shared workflows, and Docker builders, follow the [Node.js version upgrades procedure](../devops/#nodejs-version-upgrades).

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

### Frontend stack

The frontend is split between the **Cofolio monorepo**, which owns the applications and their shared code, and **DSAV**, the separately maintained and published AVENIR(s) design system. DSAV is a dependency of the applications, not another application in the monorepo.

| Concern | Tools and responsibility |
| --- | --- |
| Application framework | Vue 3 Single-File Components and TypeScript |
| Development and bundling | Vite; application builds also run Vue TypeScript checking |
| Routing | Vue Router, with application-owned routes, layouts and guards |
| Server state | TanStack Vue Query for requests, mutations, cache and invalidation |
| Client state | Pinia for application and workflow state |
| Forms | TanStack Vue Form, with reusable fields and validators |
| Internationalization | Vue I18n, French/English messages and application-owned instances |
| API access | Orval-generated Vue Query/fetch client and shared HTTP infrastructure |
| UI | `@avenirs-esr/avenirs-dsav`, complemented by shared and feature-specific components |
| Quality | ESLint, Vitest, Vue Test Utils, MSW, Playwright and Axe |

Use the repository manifests, `pnpm-workspace.yaml` and `pnpm-lock.yaml` for dependency versions rather than copying version numbers from architecture examples. The [frontend architecture page](../arch-front/) provides additional design-system background; the workspace layout and commands below describe the inspected repositories.

### Cofolio monorepo

The `avenirs-cofolio-client` repository is a PNPM workspace containing **two independently built Vue applications** and a private shared source package:

```text
avenirs-cofolio-client/
├── apps/
│   ├── avenirs-cofolio-client/
│   │   ├── src/                     # Student and staff application
│   │   ├── public/
│   │   ├── a11y/                    # App-owned accessibility suite
│   │   └── e2e/                     # Separate Playwright-BDD workspace package
│   └── avenirs-cofolio-backoffice-client/
│       ├── src/                     # Backoffice application
│       ├── public/
│       └── a11y/                    # Backoffice accessibility suite
├── packages/
│   └── shared/
│       ├── api/specs/               # Shared Avenir ESR OpenAPI contract
│       ├── orval/                   # Reusable generation policy
│       ├── src/                     # Runtime, UI, messages and generated API
│       └── test-utils/              # Separate unit, MSW and a11y entries
├── .github/                        # Workflows for both apps and Shared
├── .husky/                         # Repository-level Git hooks
├── package.json                    # Workspace commands and Git tooling
├── pnpm-workspace.yaml             # Package selection and dependency catalogues
└── pnpm-lock.yaml                  # Single workspace lockfile
```

The workspace selects `apps/*`, `apps/avenirs-cofolio-client/e2e` and `packages/*`. The nested E2E package has its own manifest and runner dependencies, but participates in the same workspace installation and lockfile. Accessibility directories belong to their applications; they are not separate packages.

External dependency ranges are centralized in the default PNPM catalogue. Named `backoffice` and `e2e` catalogues retain package-specific requirements. Manifests use `catalog:` references for these dependencies and `workspace:*` for the private `@avenirs-esr/shared` package. Update the owning catalogue and lockfile together instead of duplicating dependency versions in application manifests.

### Application architecture

Both applications keep their own bootstrap, router, layouts, i18n instance, HTTP policies, Vite configuration, TypeScript projects and tests. They consume shared code through package exports rather than importing another application's `src` tree. The `@/` alias always belongs to the current application; `#shared/` is private to the shared package.

Features group their views, components, composables, queries, stores, routes, types and translations by functional domain. A feature's `index.ts` is its public boundary: import another feature's public exports rather than reaching into its private component hierarchy. Keep parent-only child components beside their parent and co-locate component tests and stubs with their implementation.

#### Cofolio: student and staff experiences

`apps/avenirs-cofolio-client` contains both user experiences; **staff is not the Backoffice application**. Its cross-cutting source lives in `src/common/`, while domain code lives under:

| Feature area | Responsibilities and domains |
| --- | --- |
| `src/features/auth/global` | Authentication and session lifecycle |
| `src/features/student/global` | Student layout, navigation, route aggregation and reusable student patterns |
| `src/features/student/*` | Activities, associations, project building, declared skills, kit, personal career, self-knowledge, skills, tools, traces and user profile |
| `src/features/staff/global` | Staff layout, navigation and route aggregation |
| `src/features/staff/*` | Activities, administration, assessments, feedback and user profile |

`src/router/index.ts` assembles auth, student and staff routes. `/student` and `/staff` lazy-load their respective layouts. Guards validate the session, enforce user categories/roles and redirect users who must accept the terms of service. The auth store shares concurrent session checks, clears cached queries on session invalidation and synchronizes session changes across tabs.

The role layouts under `src/features/{student,staff}/global/layouts/` own the application shell: header, role navigation, skip links, quick links, global alert, main content and footer. Keep student-only reusable patterns in `student/global`, not automatically in the cross-application shared package.

#### Backoffice: administration application

`apps/avenirs-cofolio-backoffice-client` is a separate Vue application, not a third Cofolio role. Its inspected feature tree contains:

- `src/features/global-alert`: view and edit the global alert, including activation and scheduling.
- `src/features/terms-of-service`: manage terms-of-service content.
- `src/features/national-tools`: the national-tools route; its current view is a placeholder.

Backoffice uses top-level `src/components/`, `src/layouts/`, `src/constants/`, `src/utils/`, `src/types/` and `src/tests/`, rather than Cofolio's `src/common/` layout. `src/App.vue` mounts `MainLayout` and the toaster. Its router declares `/global-alert/details`, `/global-alert/update`, `/national-tools` and `/terms-of-service`; it does not implement Cofolio's session/category/terms-acceptance guard. Do not assume the two applications have identical authentication behavior.

#### Bootstrap, state, translations and forms

In each app, `src/main.ts` starts browser MSW when enabled outside production, then mounts the application created by `src/bootstrap.ts`. Bootstrap imports the app's HTTP configuration, DSAV/application styles and installs Pinia, Vue Router, Vue Query and Vue I18n after locale initialization.

Both apps configure Pinia persistence and maintain their own Vue Query plugin. Use query hooks for server-backed state and Pinia for local/workflow state rather than duplicating server caches in stores. Feature form composables own defaults, validation, dirty/submitting state and submission; mutations coordinate cache invalidation and user feedback.

Each `src/plugins/vue-i18n/vue-i18n.ts` merges Shared's French/English message data with app-level dictionaries and dynamically loaded feature dictionaries before mounting. Domain translations stay with their feature. Use the existing application's i18n instance in shared components/composables rather than introducing another singleton.

### Shared package

`packages/shared` is the private, source-exported **`@avenirs-esr/shared`** package. It is not a published design-system replacement and does not require a separate JavaScript library build: application tooling compiles its TypeScript and Vue sources. Each application's solution TypeScript configuration references Shared's solution, which checks runtime, tooling and test projects separately.

Shared owns cross-application building blocks:

- **UI:** cards, form inputs and controls, `QuerySuspense`, `EmptyState`, `ErrorMessage`, `Loader`, page titles, skip links, confirmation modals and tooltips. Components, public prop types and co-located stubs are exported through the runtime root.
- **Composables and state:** API error mapping/toasts, date and form validation, breadcrumbs, language switching, modal/loading state, unsaved-change guards and the toaster store.
- **Messages:** shared French/English messages and locale definitions. Applications merge the shared message data into their own i18n instances; Shared does not install a global application i18n singleton.
- **API infrastructure:** the contract and generated client, fetch factory, interceptor manager, transport types, HTTP status codes and exception/error definitions.
- **Test support:** reusable mounting/setup helpers, browser-safe MSW helpers and accessibility audit helpers, with separate dependency boundaries.

| Public entry point | Intended consumers |
| --- | --- |
| `@avenirs-esr/shared` | Runtime components, props/stubs, composables, stores, locale data and HTTP/error utilities |
| `@avenirs-esr/shared/api/avenir-esr` | Generated requests, query/mutation hooks, query keys and API models |
| `@avenirs-esr/shared/api/specs/*` | Static OpenAPI contract files |
| `@avenirs-esr/shared/i18n/locales/*` | Static shared locale JSON, including Node-side translation loaders |
| `@avenirs-esr/shared/orval` | Tooling-only `AVENIR_ESR_ORVAL_OVERRIDE` configuration |
| `@avenirs-esr/shared/test-utils/unit` | Vue mounting helpers and explicit unit-test setup |
| `@avenirs-esr/shared/test-utils/msw` | Browser-safe request and pagination helpers |
| `@avenirs-esr/shared/test-utils/a11y` | Accessibility audit/wait helpers and timeout configuration |

There is no catch-all `/test-utils` entry. Select the appropriate subpath so browser MSW handlers do not load Playwright or unit-test tooling. Shared component stubs come from the runtime root, not the unit-test helper entry. The generated API and Orval tooling also have their own entries rather than being re-exported through the broad UI/runtime barrel.

### API client and HTTP configuration

The API source of truth is `packages/shared/api/specs/avenir-esr.swagger.json`. `packages/shared/orval.config.ts` generates a Vue Query client using fetch, grouped by tags, under `packages/shared/src/api/avenir-esr/generated/`, with models under `types/`. Generated files are ignored: update the contract or generator policy, then regenerate instead of editing generated controllers or models.

```sh
# From the monorepo root
pnpm --filter @avenirs-esr/shared run generate:api
```

Workspace installation also runs Shared's API generation through its `prepare` lifecycle. Both applications import requests and models from `@avenirs-esr/shared/api/avenir-esr`; neither owns a separate local API generator.

The generated functions delegate to the private `sharedFetch` transport. Each application initializes it with `configureSharedFetch(config, interceptorManager)` in its own HTTP configuration module. The application retains ownership of its base URL, headers, locale handling, authentication/session policy and interceptors. Bootstrap and unit-test setup load that configuration before API use; an unconfigured shared transport rejects requests before network access.

Keep business query composition, query invalidation, route reactions and feature-specific error presentation in the owning application feature. Reusable transport mechanics belong in Shared; authentication redirects and application context do not.

### Local frontend development

Use **Node 26** from the monorepo's `.nvmrc` and **PNPM 10.34.2** from its root `packageManager`. Install once at the workspace root:

```sh
nvm install
nvm use
pnpm install --frozen-lockfile
```

Create private environment files from each application's `.env.example` at `apps/avenirs-cofolio-client/.env` and `apps/avenirs-cofolio-backoffice-client/.env`. Cofolio's E2E package keeps its own environment at `apps/avenirs-cofolio-client/e2e/.env`. Do not place application Vite configuration in a root `.env` or commit credentials. Backend services and authentication are provided by the [development environment](#development-environment).

| Local application | Default browser base path | Development port | Accessibility preview port |
| --- | --- | --- | --- |
| Cofolio | `/cofolio/` | Vite default 5173, not pinned | 4173 |
| Backoffice | `/cofolio-back-office/` | 5174, strict | 4174, strict |

`VITE_BASE_PATH` overrides the application base path; `VITE_API_URL` configures its API base URL; `VITE_ENABLE_MSW` enables browser mocks outside production. Cofolio also configures `/api` and `/api/back-office` development proxies through `VITE_API_PROXY_TARGET` and `VITE_BACKOFFICE_API_PROXY_TARGET`. Backoffice does not declare equivalent Vite proxies. In the nested E2E package, `VITE_API_URL` instead means the **browser target URL**, including `/cofolio/`.

| Command from workspace root | Scope |
| --- | --- |
| `pnpm dev` | Start both direct applications |
| `pnpm build` | Run matching workspace build scripts; each app builds its own `dist/` |
| `pnpm build:development` | Build both applications in development mode |
| `pnpm preview` / `pnpm preview:development` | Preview both direct applications |
| `pnpm type-check` | Run direct-app type checks, including their Shared project references |
| `pnpm lint` / `pnpm format` | Run matching workspace scripts; `format` modifies files |
| `pnpm test` / `pnpm test:coverage` | Run matching workspace unit-test scripts, including Shared; not E2E |
| `pnpm test:a11y` | Run the direct applications' accessibility scripts |

Root script scope is explicit: development, preview, development-build, type-check and accessibility commands use the `./apps/*` selector; `build`, lint/format and unit-test/coverage commands dispatch across matching workspace packages. Packages without a matching script are skipped. For focused work, filter by package name; commands then run in that package's directory:

```sh
pnpm --filter avenirs-cofolio-client run dev
pnpm --filter avenirs-cofolio-backoffice-client run dev
pnpm --filter avenirs-cofolio-client run generate:feature
pnpm --filter avenirs-cofolio-backoffice-client run generate:feature
pnpm --filter @avenirs-esr/shared run type-check
pnpm --filter @avenirs-esr/shared run test:coverage
```

### Frontend testing and quality

- **Unit/component tests:** Vitest and Vue Test Utils; keep `.test.ts` files near their implementation. Application setup supplies the local i18n/Pinia context, HTTP configuration and MSW handlers. Shared's Vitest configuration separates `runtime` (Node), `test-utils`, `composables` and `ui` (JSDOM) projects.
- **HTTP mocks:** MSW provides application-owned browser and Node handlers. Import only browser-safe shared MSW helpers from the dedicated subpath.
- **Accessibility:** each application's `a11y/` suite uses Playwright and Axe, with application-owned route selection, configuration and reports. Cofolio previews use port 4173 and Backoffice accessibility previews use 4174.
- **End-to-end:** Cofolio's nested `e2e/` package owns student and staff Playwright-BDD scenarios under `e2e/tests/`, with PageObjects, ComponentObjects and fixtures under `e2e/framework/`. Its app-level `e2e`, `e2e:chrome`, `e2e:install` and `e2e:report` shortcuts delegate to that package. The runner generates BDD tests before Playwright execution and supports desktop and mobile projects; it does not start a web server itself. Backoffice has no equivalent nested E2E package.
- **Static checks:** run the owning package's type-check and ESLint commands; a successful Vite bundle alone does not establish TypeScript correctness.

For local accessibility checks against mocked services:

```sh
# Install browser binaries through the package that owns each runner
pnpm --dir apps/avenirs-cofolio-client exec playwright install chromium
pnpm --dir apps/avenirs-cofolio-backoffice-client exec playwright install chromium
VITE_ENABLE_MSW=true pnpm build:development
CI=true pnpm test:a11y
```

For Cofolio desktop E2E, first build with MSW enabled, then keep its preview running in one terminal:

```sh
pnpm --filter avenirs-cofolio-client run preview:development --host 127.0.0.1 --port 4173 --strictPort
```

In another terminal:

```sh
pnpm --dir apps/avenirs-cofolio-client/e2e exec playwright install chromium
VITE_API_URL=http://localhost:4173/cofolio/ MSW_MODE_ON=true pnpm --filter avenirs-cofolio-client run e2e --project=chromium
```

The explicit localhost URL prevents a private E2E environment from selecting a remote service. Browser binaries are installed per runner because the nested E2E package can resolve dependencies separately from application accessibility tooling.

Repository-level Husky, Commitlint and lint-staged configuration covers both applications and Shared. `.github/workflows/pr-workflow.yml` declares Shared lint/coverage and separate application quality/accessibility jobs; `pr-e2e-develop.yaml` declares Cofolio's mocked-preview E2E workflow for PRs targeting `develop`.

### DSAV design system

DSAV is the AVENIR(s) design system, maintained separately and consumed as `@avenirs-esr/avenirs-dsav` by both applications and Shared.

For installation, components, styles, tokens, Storybook examples and contribution guidelines, refer to the <a href="https://avenirs-esr.github.io/avenirs-dsav/" target="_blank" rel="noopener noreferrer">DSAV documentation</a>.

### Where to make a frontend change

| Change | Owner |
| --- | --- |
| Student/staff workflow, page or feature query | `apps/avenirs-cofolio-client/src/features/` |
| Administration workflow or page | `apps/avenirs-cofolio-backoffice-client/src/features/` |
| Reusable cross-application UI, composable, message or test helper | `packages/shared`, exposed through an appropriate public entry |
| Avenir ESR contract or generated-client policy | `packages/shared/api/specs`, `orval.config.ts` and `orval/` |
| Authentication, headers, router or application bootstrap | The owning application's configuration and source |
| General design-system component, token, styling or story | The separate `avenirs-dsav` repository |

Check DSAV and Shared before adding a new local primitive. Keep domain-specific vocabulary and behavior in the feature; promote code to Shared only when it is genuinely reusable across applications, and to DSAV when it is a general design-system concern.
## Git[⇧](#table-of-contents) 

### Git workflow
The selected git flow is [GitHub Flow.](https://docs.github.com/en/get-started/quickstart/github-flow){:target="_blank"}

#### Branch naming convention
We use the following branch naming convention: <type>/<issue-number>-short-description

**Examples:** 
- feat/123-add-login
- fix/456-fix-header

#### Definition of done (DoD)

➡️ The Definition of Done is documented on a [dedicated page EN 🇬🇧](../dod/definition-of-done/) / [Version FR 🇫🇷](../dod/definition-of-done-fr/)

⚠️ In case of discrepancy, the English version shall prevail.
⚠️ En cas de divergence, la version anglaise fait foi.

---

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

- All servers share the same Unix user: **see `SRV_USER` in Vaultwarden**.
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

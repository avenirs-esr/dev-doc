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
- [Frontend](#frontend)
- [Git](#git)
  - [Git workflow](#git-workflow)
  - [Versioning (SemVer)](#versioning-semver)
  - [Conventional commits](#conventional-commits)
  - [Commitlint & husky](#commitlint--husky)
- [Clean Code Checklist](#clean-code-checklist-cf-uncle-bob)
- [Development environment](#development-environment)
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

## Frontend[⇧](#table-of-contents) 

- [Vue.js 3](https://endoflife.date/vue){:target="_blank"}

TODO: [complete the list of libraries eg. Pinia, VueQuery, etc.](../arch-front/){:target="_blank"}


## Git[⇧](#table-of-contents) 

### Git workflow
The selected git flow is [GitHub Flow.](https://docs.github.com/en/get-started/quickstart/github-flow){:target="_blank"}

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
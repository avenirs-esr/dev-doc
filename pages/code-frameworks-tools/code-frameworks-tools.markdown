---
layout: page
title: Code, Frameworks & Tools
permalink: /code-frameworks-tools/
is_menu_entry: true
position: left
order: 30
---
[Services to open for newcomers](../new-comers){:target="_blank"}
### Versions
#### Backend

- [Java 21](https://endoflife.date/eclipse-temurin)
- [Maven 3.9](https://endoflife.date/maven)
- [Spring boot 3.3](https://endoflife.date/spring-boot)
- [Node.js 20](https://endoflife.date/nodejs)

**Java & Mave version managment with sdkman**

[Sdkman](https://sdkman.io/) can help to manage the version of different tools.

```
cat .sdkmanrc
# Enable auto-env through the sdkman_auto_env config
# Add key=value pairs of SDKs to use below
java=21.0.4-oracle
maven=3.9.8  # Ajoute apres
```

Switching to the required version can be automated:
```
cat $HOME/.sdkman/etc/config
sdkman_auto_env=true
```

**Node version managment with nvm**

[Nvm](https://github.com/nvm-sh/nvm) can help to manage node.js version.<br/>
Switching to the target version can be automated by adding this snippet in .bashrc file.


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

## Git webfflow
The selected git flow is [GitHub Flow.](https://docs.github.com/en/get-started/quickstart/github-flow){:target="_blank"}

## Versioning (SemVer)
The versioning strategy is [Semantic Versioning.](https://semver.org/#semantic-versioning-200){:target="_blank"}

>   Given a version number MAJOR.MINOR.PATCH, increment the:
>
>    MAJOR version when you make incompatible API changes\
>    MINOR version when you add functionality in a backward compatible manner\
>    PATCH version when you make backward compatible bug fixes

## Conventional commits
The commit messages should respect the [Conventional Commits specification.](https://www.conventionalcommits.org/en/v1.0.0/){:target="_blank"}\
This specification defines at least 2 types of messages, feat, fix and the notion of Breaking change. \
This allow to automate the SemVer management:
     
    fix             -> patch
    feat            -> minor
    Breaking change -> major

### Commitlint & husky
In order to facilitate the commit with command line, [commitlint](https://github.com/conventional-changelog/commitlint){:target="_blank"} can be used in conjunction to [husky](https://typicode.github.io/husky/){:target="_blank"} (client side git hooks).    

## Continuous Integration with GitHub Actions
- [Avenirs-portfolio-security example](../ci-gh-actions/){:target="_blank"}

## API 

### API Manager
- [API Management tools](../apim-list/)

## Storage 

- [PostegreSQL](../storage-posgresql/)

#### Tests & POC

- [Gravitee](../apim-gravitee/)
- [Apache APISIX](../apim-apisix/)
- [Kong](../apim-kong/)

## Docker & Docker-compose
- [Docker & Docker-compose installation notes](../docker-install/)
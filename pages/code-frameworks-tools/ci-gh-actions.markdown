---
layout: page
title: CI - GitHub Actions
permalink: /ci-gh-actions/
---

## Exemple with the project Avenirs-portfolio-security

### Key points
- Use standalone composite actions: reusable for other projects (via git submodule).
- Handles cache, and a digest if needed (use of latest key word for versions).
- Debug steps leaved in files but disabled.
- For CAS the docker image could not be used with OIDC. It is rebuilt from CAS repository.
- [Main GihtHub actions directory](https://github.com/avenirs-esr/avenirs-portfolio-security/tree/main/.github){:target="_blank"}
- [Entrypoint: workflow file](https://github.com/avenirs-esr/avenirs-portfolio-security/blob/main/.github/workflows/main.yaml){:target="_blank"}

### TODO
- Move cas and ldap directories (contains settings / fixtures) to the folder of the action.
- Try to use cache for maven dependencies.
- Uses repository secrets, somme of secrets should be moved in environment secrets.


### Global diagram

{% include img.html
        src="assets/images/ci-gh-actions.svg"
        alt="Front Office - Exemple Workflow"
        caption="Avenirs-portfolio-security - workflow"
%}


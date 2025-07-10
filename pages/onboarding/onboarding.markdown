---
layout: page
title: Onboarding
permalink: /onboarding/
is_menu_entry: true
position: left
order: 3000
---
## Table of Contents  
- [Introduction](#introduction)  
- [Services to Open](#services-to-open)  
- [APC](#apc)  
- [Team Organization & Project Management](#team-organization--project-management)  
- [Rocket Chat](#rocketchat)
- [Repositories](#repositories)
- [Developer Resources](#developer-resources)


## Introduction[⇧](#table-of-contents) 
This section is designed to guide you through your first steps on the project.
You'll find key information and useful resources to help you get started in the best possible conditions.

See the presentation given at the [ESUP Days in April 2025.](https://videos.esup-portail.org/esup-days/esupdays39/video/1776-esupdays39-01042025-apres-midi/){:target="_blank"}

Feel free to let us know if anything is missing or unclear.

## Services to Open[⇧](#table-of-contents) 
Note: these services need to be opened by our team (manual labor required!).
- Create account in RECIA LDAP
- add to group for RENATER services
- add to group for Nextcloud RECIA
- Send email to explain/provide:
   - RECIA login + password
   - activating RECIA account
   - checking access on [RENATER account attributes](https://test-sp.federation.renater.fr/) using the rigth IDP.
   - first access [RocketChat](https://rocket.esup-portail.org)
- RocketChat ESUP channels (development and functional).
- BBB ESUP.
- VPN RECIA: required to access the development server via ssh with a key ED25519.
- Team's Password manager: VaultWarden (invitation sent by email) - Organization + collection access
- GitHub (add account to group).
- Dev-doc.
- Srv-dev.

## APC[⇧](#table-of-contents) 
- In French, APC: Approche Par Compétences.
- In English, CBA: Competency-Based Approach.

- [Avenirs-ESR Official site](https://avenirs-esr.fr/){:target="_blank"}
- [ESUP-ORA (CBA Modeling Tool for Teachers)](https://avenirs-esr.fr/ora/){:target="_blank"}
- [CBA Glossary](https://avenirs-esr.fr/reperes-apc/glossaire/){:target="_blank"}
- [FAQ](https://avenirs-esr.fr/reperes-apc/faq/){:target="_blank"}


## Team Organization & Project Management[⇧](#table-of-contents)

- [Project roadmap](../#roadmap){:target="_blank"}

**[TODO]:** *This section needs to be finalized, and some details need to be clarified.*


- Sprint duration: 3 weeks.
- AMOA Backlog management: [AMOA project board on GitHub](https://github.com/orgs/avenirs-esr/projects/3){:target="_blank"}
- Backlog management: [project board on GitHub](https://github.com/orgs/avenirs-esr/projects/3){:target="_blank"}
- Weekly Meetings: [ESUP BBB](https://greenlight.esup-portail.org/rooms/tic-lgh-n9r-wkf/join){:target="_blank"}
- Daily: [ESUP BBB](https://greenlight.esup-portail.org/rooms/tic-lgh-n9r-wkf/join){:target="_blank"}
- Chat: 
   - [Development Channel on RocketChat](https://rocket.esup-portail.org/group/Avenirs_developpement){:target="_blank"}
   - [Functional Channel on RocketChat](https://rocket.esup-portail.org/group/Avenirs_fonctionnel){:target="_blank"}

## RocketChat[⇧](#table-of-contents) 

When you log on to RocketChat, we strongly advise you to add a `front_team` or `back_team` tag (depending on your preference) to your user settings. That way, you'll be notified along with the whole team whenever you need to be.

If you need to tag either team, simply write `front_team` or `back_team` in your message.
⚠️ Be careful not to put `@` before it, as this will prevent the tag from working.

<img width="1000" height="574" alt="image" src="https://github.com/user-attachments/assets/009850f3-589c-452e-8d52-b7454849d29b" />


## Repositories[⇧](#table-of-contents)    
- [AVENIRS-Project](https://github.com/avenirs-esr/AVENIRS-Project/projects?query=is%3Aopen){:target="_blank"}: hosts GitHub projects for project management.
- [dev-doc](https://github.com/avenirs-esr/dev-doc){:target="_blank"}: repository for this documentation (based on GitHub Pages).
- [srv-dev](https://github.com/avenirs-esr/srv-dev){:target="_blank"}: repository for the development environment.
- [avenirs-deployment](https://github.com/avenirs-esr/avenirs-deployment){:target="_blank"}: scripts to deploy the srv-dev for different environments.
- [avenirs-portfolio-api](https://github.com/avenirs-esr/avenirs-portfolio-api){:target="_blank"}: repository for the portfolio domain API.
- [avenirs-cofolio-client](https://github.com/avenirs-esr/avenirs-cofolio-client){:target="_blank"}: repository for the user interface.
- [avenirs-workflows](https://github.com/avenirs-esr/avenirs-workflows){:target="_blank"}: repository for reusable GitHub actions & workflows.
- [avenirs-portfolio-security](https://github.com/avenirs-esr/avenirs-portfolio-security){:target="_blank"}: repository for the security module (RBAC). Needs deep refactoring.

## Developer resources[⇧](#table-of-contents) 
- [Dev Handbook](../dev-handbook/){:target="_blank"}: versions, github workflow, tools, etc.
- [Architecture](../arch/#simplified-architecture-overview){:target="_blank"}: see the simplified architecture overview diagram.
- [Progress report](../pages/03_2025_rapport_avancement){:target="_blank"}: an opinionated overview of the current status as of April 2025.

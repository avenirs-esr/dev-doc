---
layout: page
title: Architecture - Frontend
permalink: /arch-front/
---

_Last updated: 2025-10-08_

# Technical stack definition

This document defines the technical stack used in frontend projects.
It ensures consistency, maintainability, and compatibility across all applications built on the company’s frontend ecosystem.

| Layer | Library | Version | Purpose |
| --- | --- | --- | --- |
| Framework | Vue.js | 3.5.13 | Reactive frontend framework |
| Language | TypeScript | 5.4.5 | Static typing and tooling |
| UI | @gouvfr/dsfr / @gouvminint/vue-dsfr | 1.14.0 / 8.4.0 | Base design system |
| State (server) | @tanstack/vue-query | 5.74.9 | Server state & cache |
| State (local) | pinia | 3.0.3 | Reactive store |
| API | orval | 7.9.0 | OpenAPI client generator |
| Build | vite | 6.3.3 | Fast dev/build tool |

## Language and framework

- [Vue.js 3](https://endoflife.date/vue) (current version: 3.5.13)
- Typescript (current version: 5.4.5)

## Librairies

### Server state management
- [@tanstack/vue-query](https://tanstack.com/query/v5/docs/framework/vue/overview) (current version: 5.74.9)
- [axios](https://axios-http.com/) (current version: 1.9.0)

### API generation
- [orval](https://orval.dev/) (current version: 7.9.0)

### Local state management
- [pinia](https://pinia.vuejs.org/) (current version: 3.0.3)

### Form handling
- [@tanstack/vue-form](https://tanstack.com/form/latest) (current version: 1.15.0)

### Design system foundation 

These libraries provide the official French government design system foundation.
They are extended through our internal component library (`@avenirs-esr/avenirs-dsav`) to ensure consistent UI/UX across all projects.

- [@gouvfr/dsfr](https://github.com/GouvernementFR/dsfr) (current version: 1.14.0)
- [@gouvminint/vue-dsfr](https://github.com/dnum-mi/vue-dsfr) (current version: 8.4.0)

### Code format
- [eslint](https://eslint.org/) (current version: 9.9.1)

### Tests
- [playwright](https://playwright.dev/): [@playwright/test](https://www.npmjs.com/package/@playwright/test) (current version: 1.52.0)
- [vitest](https://vitest.dev/) (current version: 3.1.3)

### Build
- [vite](https://vite.dev/) (current version: 6.3.3)

# Architecture

Each frontend project follows a consistent structure to ensure scalability, maintainability, and alignment with our design system.

## DSAV

DSAV extends the DSFR design foundations and provides additional Vue components, composables, and styles specific to AVENIR(s) projects.

### Folder structure

The DSAV (Design System AVENIR(s)) repository contains the source code, documentation, accessibility tests, and Storybook configuration used to build and distribute our design system.

```text
.storybook/                     # Storybook configuration and setup
├── main.ts                     # Storybook configuration
├── preview.scss                # Storybook styling configuration
├── preview.ts                  # Storybook preview configuration
.a11y/                          # Accessibility tests and helpers
├── tests/                      # All accessibility tests files ordered according to components architecture
├── playwright.a11y.config.ts   # Accessibility tests configuration
├── tsconfig.a11y.json          # Accessibility tests configuration
├── utils.ts                    # Methods used for testing accessibility on components stories
docs/                           # Markdown documentation for design tokens and components (some parts are auto-generated)
├── components/                 # Documentations for all design system components
├── public/                     # Assets for documentations
├── tokens/                     # Documentations for design tokens
src/                            # Source code of the design system
├── assets/                     # Static assets (image, icons, etc.)
├── components/                 # Reusable design system components
├── composables/                # Reusable Vue composables (logic only)
├── config/                     # Enums and constants global to frontend projects
├── stories/                    # Non design system components stories and storybook documentations
├── styles/                     # Reusable design styles (dimensions, palette, radius, spacing, typography, etc.)
├── tests/                      # Reusable test utils (stubs, helpers, etc.)
├── tokens/                     # Reusable design tokens (icons, etc.)
├── utils/                      # Utility functions and helpers
└── App.vue                     # Application root component (not used)
```

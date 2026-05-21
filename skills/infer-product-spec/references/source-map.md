# Source Map

## Web apps

Check:

- `app/`, `pages/`, `routes/`, router configs
- layout shells, nav bars, dashboards, settings pages
- form schemas, server actions, client actions
- marketing copy, empty states, onboarding flows
- Storybook stories, screenshots, Playwright or e2e tests

Map:

- routes and nav labels -> surface map and feature areas
- form fields and buttons -> inputs, triggers, outputs
- loading / success / error states -> user flow steps and edge cases

## APIs and backend-heavy products

Check:

- route handlers, controllers, RPC procedures
- OpenAPI specs, schema files, SDK clients
- background jobs, queues, cron tasks
- domain models, event names, permission middleware

Map:

- public endpoints -> externally visible capabilities
- job pipelines -> asynchronous product behavior
- schemas and enums -> core entities and lifecycle states

## Mobile / desktop apps

Check:

- navigation stacks, screen registrations, menus
- view models, controllers, page components
- local persistence, sync flows, notification handlers

Map:

- screen transitions -> user flows
- menu structure -> information architecture
- offline / sync logic -> product constraints and promises

## Cross-cutting evidence

Always check when present:

- README and setup docs
- fixtures, seed data, demo content
- analytics events
- permission and billing code
- feature flags
- localization strings
- tests that encode happy paths

Prefer user-visible strings and reachable flows over internal helper names.

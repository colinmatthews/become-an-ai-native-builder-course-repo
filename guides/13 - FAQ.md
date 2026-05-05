# FAQ

This guide explains terms used in the rollout plan and gotcha checklist.

Use it with the course MCP when you run into an unfamiliar term.

Prompt:

`Use the FAQ from the course. Explain this term in plain language, tell me why it matters for a PM using AI agents, and give me an example.`

## AI agent

An AI tool that can take a task, inspect files or data, make changes, and report back.

In this course, agents include tools like Claude Code, Codex, Cursor, and background agents.

## Background agent

An AI agent that works asynchronously while you do something else.

For example, you can ask a background agent to make a small code change, then come back later to review the diff.

## Repo

Short for repository.

A repo is where a codebase lives, usually in Github.

## Main repo

The primary repo for the product.

This may include the frontend, backend, database schema, tests, configuration, and deployment setup.

Many PMs should not start here unless the app is easy to run and engineering is comfortable with draft PRs.

## Component repo

A repo that contains reusable UI components.

This is often a safer place to start because you can prototype product changes without running the entire app.

## Design system

The shared set of components, styles, colors, spacing, typography, and interaction patterns used by a product.

If an AI creates new UI without checking the design system, the result may look inconsistent or create unnecessary engineering work.

## Storybook

A tool teams use to view and test UI components outside the full app.

Storybook is useful when the full product is hard to run locally but the frontend components can still be developed safely.

## Monolith

An app where most of the product lives in one codebase or service.

Monoliths are often easier for beginners to understand than systems split across many services, though they can still be large.

## Microservices

A product architecture where different parts of the app run as separate services.

For example, payments, authentication, analytics, search, and notifications may each have their own service.

If a product uses many microservices, it may be hard or impossible for a PM to run the full app locally.

## Local development

Running the product on your own machine.

For AI agents, local development can also mean running the product inside the agent's remote development environment.

## Docker

A tool for running software in containers.

Teams use Docker to make dependencies like databases, queues, and services easier to run consistently.

If Docker setup is fragile, do not make it your first blocker. Ask for a smaller starting environment.

## Environment variables

Configuration values used by an app.

Examples:

- `DATABASE_URL`
- API keys
- Feature flag settings
- Service URLs

Environment variables should not be committed to the repo if they contain secrets.

## Secrets

Sensitive values like API keys, database passwords, private tokens, and credentials.

Do not paste production secrets into unapproved AI tools.

## VPN

A secure network connection some companies require before accessing internal tools or services.

Cloud-based agents may not be able to access VPN-only resources unless the company has configured that intentionally.

## Database

Where product data is stored.

Common examples include Postgres, MySQL, MongoDB, and Snowflake.

If an app needs a database to run, ask whether there is a safe local, development, or preview database.

## Postgres

A common relational database.

Many modern apps use Postgres for core product data.

## Seed data

Fake or sample data used to make a local app useful.

Without seed data, the app may technically run but still be hard to test because the screens are empty.

## Fixtures

Saved sample data used in tests or local development.

Fixtures let teams test predictable scenarios without relying on production data.

## Mock

A fake version of a service, API, or data source.

Mocks are useful when the real service is hard to access, risky, slow, or unnecessary for the task.

## Staging

A test environment that is usually closer to production than local development.

Teams use staging to test changes before releasing them to customers.

## Preview deployment

A temporary version of the app created for a branch or PR.

Preview deployments are useful because reviewers can click through a change without running the app locally.

## PR

Short for pull request.

A PR is a proposed code change that other people can review before it is merged.

## Draft PR

A PR that is not ready to merge yet.

Draft PRs are useful for getting early feedback without implying the change is finished.

## Diff

The set of changes made to files.

When reviewing AI-generated work, always inspect the diff before accepting the result.

## CI

Short for continuous integration.

CI automatically runs checks when code is pushed, such as tests, linting, typechecks, or builds.

## Lint

A check that catches code style problems and some common bugs.

For example, lint may catch unused variables or invalid formatting.

## Typecheck

A check that verifies typed code is internally consistent.

In TypeScript, typecheck can catch many issues before the code runs.

## Test suite

The collection of automated tests for a project.

Large test suites can be slow or flaky, so ask engineers which tests matter for a small change.

## Flaky test

A test that sometimes passes and sometimes fails without a real code change.

If tests are flaky, ask engineering how they decide whether a change is actually safe.

## Verification command

The command used to check whether a change works.

Examples:

- `npm run lint`
- `npm run typecheck`
- `npm test`
- `npm run build`

Agents should report the exact command they ran and whether it passed.

## Manual test plan

A short description of what a human should click or check.

Manual test plans are useful when automated tests are unavailable or incomplete.

## API

The interface software uses to talk to other software.

For example, a frontend may call an API endpoint to fetch a user's projects.

## API endpoint

A specific URL or function the app calls to get or change data.

Before creating a new endpoint, an AI agent should check whether an existing endpoint already does the job.

## Backend

The server-side part of the product.

The backend usually handles data, business logic, permissions, and integrations.

## Frontend

The user-facing part of the product.

The frontend usually includes screens, components, user interactions, and calls to APIs.

## Data model

The way a product represents important concepts in code and the database.

Examples:

- User
- Account
- Team
- Project
- Subscription

AI agents often invent new models unless they are told to search for existing ones first.

## Schema

The structure of a database.

A schema defines tables, columns, relationships, and constraints.

Schema changes can be risky and should usually not be a first AI-assisted workflow.

## Migration

A code file or command that changes the database schema.

For example, a migration may add a column or create a table.

Migrations should be reviewed carefully because they can affect real data.

## Source of truth

The system or data model that should be treated as authoritative.

For example, billing status may come from Stripe, while account roles may come from your own database.

AI agents can make bad changes if they guess the wrong source of truth.

## Analytics event

A tracked user action or system event.

Examples:

- `project_created`
- `invite_sent`
- `checkout_started`

If a product change affects important user behavior, it may need analytics.

## Event property

Extra information sent with an analytics event.

For example, `project_created` might include plan type, workspace ID, or source.

Do not include sensitive personal data unless the tool and policy allow it.

## PII

Personally identifiable information.

Examples include names, emails, phone numbers, addresses, and other data that identifies a person.

Avoid sending PII to analytics or AI tools unless explicitly approved.

## Funnel

A sequence of steps users take toward an outcome.

For example:

1. Visit pricing page
2. Start checkout
3. Complete payment

Analytics events are often used to measure funnels.

## Dashboard

A saved view of product metrics.

If a change adds new analytics events, dashboards may need to be updated.

## Feature flag

A control that lets a team turn a feature on or off without deploying new code.

Feature flags are useful for testing, gradual rollouts, and rollback.

## Rollback

Reverting a change after something goes wrong.

For risky changes, ask what the rollback plan is before launch.

## Permissions

Rules that determine who can see or do something.

For example, admins may be able to invite teammates while regular users cannot.

## Roles

User categories that affect permissions.

Examples:

- Admin
- Member
- Viewer
- Owner

AI-generated work often forgets role-specific behavior.

## Auth

Short for authentication and authorization.

Authentication checks who the user is. Authorization checks what the user is allowed to do.

Auth changes are risky and should not be a first AI-assisted workflow.

## Billing

The part of the product that handles plans, subscriptions, invoices, payments, and entitlements.

Billing changes are high-risk and should involve engineering review.

## Infrastructure

The systems that run and deploy the product.

Examples include servers, databases, queues, deployment pipelines, networking, and cloud configuration.

Most PMs should not start with infrastructure changes.

## Dependency

An external library or package the codebase uses.

AI agents may add dependencies unnecessarily. Ask whether the repo already has something that does the job.

## Lockfile

A file that records exact dependency versions.

Examples:

- `package-lock.json`
- `yarn.lock`
- `pnpm-lock.yaml`

Unexpected lockfile changes can create noisy PRs and should be reviewed carefully.

## Abstraction

A reusable layer or helper created to simplify repeated logic.

Abstractions are useful when they remove real duplication, but AI agents often create them too early.

## Design token

A named design value used across the product.

Examples:

- Color
- Spacing
- Font size
- Border radius

Using design tokens keeps UI consistent.

## Empty state

What a user sees when there is no data yet.

For example, a projects page should handle the case where the user has not created any projects.

## Loading state

What a user sees while data is being fetched.

Without a loading state, the UI may look broken or confusing.

## Error state

What a user sees when something fails.

Good error states help users understand what happened and what to do next.

## Accessibility

Designing and building software so people with disabilities can use it.

Basic accessibility includes labels, keyboard navigation, focus states, contrast, and screen reader support.

## Breaking change

A change that causes existing behavior or integrations to stop working.

Breaking changes require extra review and communication.

## Customer-visible change

Any change customers can see or experience.

Customer-visible changes may require support notes, documentation, analytics, launch planning, or rollback.

## Support notes

Internal notes that help customer-facing teams answer questions about a change.

Support notes are useful when a feature changes behavior or may create customer confusion.

## Launch readiness

The work required before a change goes live.

This can include docs, analytics, support notes, feature flags, rollout plans, and rollback plans.

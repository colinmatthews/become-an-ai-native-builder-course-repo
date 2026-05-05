
Before you use AI agents at work, decide where to start. Most PMs should not start by asking for full access to the production repo.

Choose one:

- Main repo
- Component-only repo
- Docs/specs workflow
- Staging or preview environment
- No-code analysis workflow

The right answer depends on how your company builds software.

## Setup Checklist

Ask these questions before you try to run an agent against your company's codebase.

### Product and repo structure

- Is the product a monolith or many services?
- How many repos are involved in a typical feature?
- Which repo contains the frontend?
- Is there a component library, design system repo, or Storybook?
- Is there a docs repo or specs repo?
- Which repo is safest for a PM to start in?

What the answer means:

- If it is a monolith that runs locally, the main repo may be fine.
- If it is many microservices, do not start by trying to run the whole product.
- If there is a component repo or Storybook, start there for UI prototypes.
- If there is no safe code surface yet, start with docs, specs, or analysis.

Good redirect:

`It sounds like the full product is hard to run locally. Is there a component repo, frontend-only repo, or Storybook where I can prototype safely first?`

### Local development

- Can engineers run the full app locally?
- How long does setup usually take?
- Is setup documented?
- Does setup require Docker?
- Does setup require private package access?
- Does setup require VPN access?
- Does setup require local secrets?
- What usually breaks for new engineers?

What the answer means:

- If engineers struggle to run it, a PM should not start there.
- If Docker is required, ask whether Docker setup already works reliably.
- If VPN or internal services are required, cloud background agents may not work.
- If secrets are required, do not give them to an agent unless there is a clear approval path.

Good redirect:

`If the full local setup is fragile, what is the smallest environment where I can still produce useful work?`

### Data and services

- Does the app need a database to run?
- Can the database run locally?
- Is there seed data?
- Is there a safe dev database?
- Are schema changes allowed in branches?
- Are there mocks or fixtures?
- Are there customer data restrictions?

What the answer means:

- If there is no safe database, avoid DB-backed changes at first.
- If schema changes are risky, do not make them in your first workflow.
- If mocks or fixtures exist, use those before using real data.
- If customer data is involved, confirm the tool is approved for that data.

Good redirect:

`Can I use mocks, fixtures, seed data, or a local-only database instead of touching shared or production data?`

### Verification

- What command should run before opening a PR?
- Is there a fast check for frontend-only changes?
- Are tests reliable locally?
- Does CI catch most issues?
- Are there flaky tests?
- What counts as verified for copy, docs, UI, or prototype work?

What the answer means:

- If the full test suite is slow, ask for the smallest relevant check.
- If local tests are unreliable, ask how engineers verify small changes.
- If CI is the real source of truth, still run the fastest local check available.
- If there is no relevant verification, the agent should say that explicitly.

Good redirect:

`What is the smallest check that would make you comfortable reviewing a PM-generated draft PR in this area?`

### Review and ownership

- Who owns this part of the product?
- Who should review PM-generated work?
- Are draft PRs welcome?
- Should I start with an issue/spec before code?
- What areas should I avoid?
- What is a safe first change?

What the answer means:

- If ownership is unclear, do not start with code.
- If the team is nervous, start with analysis, specs, or prototypes.
- If draft PRs are welcome, keep the first one small.
- If engineering gives you a safe area, stay inside it.

Good redirect:

`I want the first attempt to be easy to review. What is the safest kind of change for me to propose?`

## Where To Start

Use the answers above to choose the right environment.

### Start in the main repo if:

- The app runs locally
- Setup is documented
- You have the right access
- There are clear verification commands
- The change is small
- Engineering is comfortable reviewing draft PRs

Good first tasks:

- Copy change
- Empty state
- Small UI polish
- Docs update
- Test update
- Release note draft

### Start in a component-only repo if:

- The full app has many services
- The frontend can be developed separately
- Storybook exists
- The team has a design system
- You want to prototype before touching product logic

Good first tasks:

- Prototype a new component state
- Improve an empty state
- Adjust layout or copy
- Create a static version of a feature idea
- Build a design-system-aligned mock

### Start with docs or specs if:

- Code access is not available
- Local setup is too hard
- Ownership is unclear
- The team needs alignment first
- The change is high-risk

Good first tasks:

- Problem brief
- Feature spec
- Research synthesis
- Support-ticket theme analysis
- Launch plan
- Engineering handoff doc

### Start in staging or preview if:

- Local setup is difficult
- The team already has preview deployments
- You can test safely without production data
- The change is mostly product or UI validation

Good first tasks:

- QA a flow
- Compare prototype to live behavior
- Capture bugs with screenshots
- Draft a small issue list

### Start with no-code analysis if:

- You do not have repo access
- The product requires production secrets
- The system cannot run outside the company network
- No one has agreed to review code output

Good first tasks:

- Analyze customer feedback
- Summarize product usage
- Draft hypotheses
- Prepare a roadmap recommendation
- Create a stakeholder update

## Troubleshooting

### The repo has too many microservices

Do not try to run the entire product.

Ask for:

- Component repo
- Frontend-only repo
- Storybook
- Mock data
- Staging or preview deployment
- One small service boundary

Prompt:

`The product has many services. Help me find a smaller surface area where I can still create useful product work without running the whole system.`

### Docker is required

Ask whether Docker setup already works for new engineers.

If it does, ask for the documented setup commands.

If it does not, avoid making Docker the first blocker. Start in a component repo, docs workflow, or staging environment.

Prompt:

`Docker is required for the full app. Help me decide whether this is worth setting up now or whether I should start with a smaller workflow.`

### The app needs secrets

Do not copy production secrets into an AI tool.

Ask for:

- Local-only secrets
- Development environment variables
- Mock services
- Seed data
- A staging-safe setup

Prompt:

`This app needs secrets to run. Help me identify which secrets are actually needed and whether there is a safer local or mock setup.`

### The app needs a database

Ask whether there is:

- Local Postgres
- Dockerized Postgres
- Seed data
- Fixtures
- Safe dev database
- Preview database

If none of these exist, avoid DB changes at first.

Prompt:

`The app needs a database. Help me choose between local Postgres, mocks, seed data, a dev database, or avoiding DB-backed changes for the first workflow.`

### Tests are slow or flaky

Ask for the smallest useful verification command.

Examples:

- Typecheck
- Lint
- Unit test for one package
- Storybook build
- Component test
- CI only, with a clear note

Prompt:

`The test suite is slow or flaky. Help me identify the smallest verification step that would still make this change reviewable.`

### Engineering is nervous

Do not argue.

Reduce the scope.

Start with:

- Read-only analysis
- A spec
- A prototype
- A docs update
- A draft PR that cannot ship without review

Prompt:

`Engineering is nervous about PM-generated code. Help me choose a safer first workflow that builds trust.`

### You cannot get code access

You can still use the workflow.

Start with:

- Customer feedback synthesis
- Product data analysis
- Spec drafting
- Prototype prompts
- Release communication
- Experiment planning

Prompt:

`I do not have code access yet. Help me apply the course workflow to product discovery, specs, prototypes, or team communication instead.`

## AI-Generated Work Gotchas

Use this list before you share a spec, prototype, or PR created with an AI agent.

The most common failure mode is not that the AI produces nothing.

The common failure mode is that it produces something plausible that ignores how your company actually works.

Ask the MCP:

`Use the gotcha list from the rollout guide. Review my AI-generated spec, prototype, or PR. Ask me for the diff or artifact, then check it for product, design, analytics, data model, testing, and rollout risks.`

### Before opening a PR

- Did the AI explain what changed and why?
- Is the diff small enough for a human to review?
- Did it touch unrelated files?
- Did it rename, move, or reformat files unnecessarily?
- Did it create new abstractions that were not needed?
- Did it leave TODOs, mock data, console logs, or debug code?
- Did it update docs or comments if behavior changed?
- Did it run the smallest relevant verification command?

Prompt:

`Review this diff like a senior engineer. Focus on unnecessary changes, missing verification, invented abstractions, and places where the agent should have reused existing code.`

### Existing patterns

- Did the AI search for existing components before creating a new one?
- Did it use the existing design system?
- Did it follow existing file naming patterns?
- Did it follow existing route, API, and state management patterns?
- Did it reuse existing hooks, helpers, services, or utilities?
- Did it copy-paste logic that already exists elsewhere?
- Did it introduce a new library when an existing dependency could do the job?

Prompt:

`Before accepting this change, search the repo for existing components, hooks, helpers, and patterns that should have been reused. Tell me where this change diverges from the codebase.`

### Components and UI

- Did the AI create a new component when an existing component could be extended?
- Did it create a one-off style instead of using design tokens?
- Did it hardcode spacing, colors, text sizes, or breakpoints?
- Did it handle loading, empty, error, and permission states?
- Did it work on mobile and desktop?
- Did it preserve accessibility basics like labels, focus states, keyboard navigation, and contrast?
- Did it introduce copy that sounds different from the rest of the product?

Prompt:

`Check this UI change against the existing design system. Identify any new components, hardcoded styles, missing states, accessibility issues, or copy that does not match the product.`

### Data models

- Did the AI invent a new data model instead of finding the existing one?
- Did it check existing database tables, API schemas, types, or interfaces?
- Did it duplicate a concept that already exists under a different name?
- Did it add fields without understanding who owns the source of truth?
- Did it assume relationships between objects that may not be true?
- Did it create schema changes when the same outcome could be achieved in the frontend or API layer?

Prompt:

`Before changing the data model, search for existing tables, API schemas, TypeScript types, and domain concepts related to this feature. Tell me whether this change is reusing the existing model or inventing a new one.`

### Analytics and events

- If the change affects user behavior, did it add or update analytics events?
- Did it reuse existing event names and properties?
- Did it check the team's analytics naming conventions?
- Did it avoid sending PII or sensitive data?
- Did it update dashboards, funnels, or experiment tracking if needed?
- Did it create events that product and data teams can actually interpret?

Prompt:

`Check whether this product change needs analytics. Search for existing event naming conventions and similar events. Recommend the smallest useful event instrumentation, or explain why none is needed.`

### Permissions, roles, and edge cases

- Did the AI consider user roles and permissions?
- Did it assume every user can see or edit the same thing?
- Did it handle deleted, archived, disabled, or incomplete records?
- Did it handle first-time users?
- Did it handle large accounts or long lists?
- Did it handle slow network, failed requests, and retry behavior?
- Did it change behavior for existing customers unintentionally?

Prompt:

`Review this change for permission, role, empty-state, error-state, and existing-customer edge cases. Tell me what scenarios need to be tested before review.`

### API and backend changes

- Did the AI use an existing API endpoint before creating a new one?
- Did it follow existing request and response patterns?
- Did it handle errors consistently?
- Did it introduce a breaking API change?
- Did it add validation where needed?
- Did it consider performance for large accounts?
- Did it change authorization logic?

Prompt:

`Review this backend or API change against existing endpoint patterns. Look for duplicated endpoints, missing validation, breaking changes, authorization issues, and performance risks.`

### Tests and verification

- Did the AI add or update tests for changed behavior?
- Did it run the relevant checks?
- Did it only claim tests passed, or did it show the command and result?
- Did it skip tests because setup failed?
- If setup failed, did it explain the failure clearly?
- Is there a manual test plan?
- Would CI catch the important issues?

Prompt:

`Check whether this change has enough verification. Ask for the commands run, results, missing tests, and a manual test plan if automated tests are not practical.`

### Product and launch readiness

- Does the change match the original problem?
- Is it solving the smallest useful version?
- Does it require customer communication?
- Does support need to know?
- Does documentation need to change?
- Does pricing, packaging, onboarding, or permissions change?
- Does the change need a feature flag?
- Does it need a rollback plan?

Prompt:

`Review this change as a PM before launch. Check whether it matches the problem, whether the scope is too broad, and whether it needs docs, support notes, analytics, a feature flag, or a rollback plan.`

### Red flags

Stop and ask for help if the AI:

- Touches auth, billing, permissions, security, infrastructure, or data deletion
- Creates a new data model without checking existing models
- Creates new components without checking the design system
- Adds analytics events without checking naming conventions
- Changes many unrelated files
- Updates lockfiles unexpectedly
- Adds a new dependency without a clear reason
- Removes tests
- Disables lint rules or type errors
- Says it could not run checks but still recommends merging
- Makes production-data assumptions

Prompt:

`This change has red flags. Help me reduce the scope, identify what needs human review, and decide whether I should abandon, revise, or split the work.`

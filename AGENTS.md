# AGENTS.md

## Purpose

This repository uses AI agents for controlled, validated, reviewable coding work.

The goal is **not** to let agents edit code freely. The goal is to make agent work:

- bounded
- inspectable
- reversible
- tested
- documented
- safe before commit or push

Agents must operate like careful maintainers, not autonomous product owners.

## Project Context

This project is **l8r.chat**.

l8r.chat is a guided chat-first scheduling and booking layer. It should help people coordinate plans, appointments, services, payments, deposits, fees, and required documents through a consistent chat UX across SMS, iMessage, website chat, and app chat.

Core product rules:

- The domain is `l8r.chat`.
- The primary UX is guided chat.
- Users should always see useful buttons, suggested actions, generated reasons, or selectable intents.
- Users may type, but they should never be forced to start from a blank prompt.
- Only service providers create accounts.
- Consumers, friends, clients, and appointment requesters use phone-number identity.
- Service providers can manage services, availability, intake, confirmations, deposits, payments, fees, and document collection.
- Consumers can pay or upload required documents without creating a l8r account.
- Human review or confirmation is required before outbound messages, bookings, cancellations, reschedules, payment requests, or sensitive document requests.
- Do not claim HIPAA, SOC 2, PCI, FINRA, legal-record, or similar compliance unless actually implemented, verified, and documented.

## Agent Operating Rule

Before making changes, agents must fully inspect the repository context.

Agents must read:

1. `AGENTS.md`
2. `README.md`
3. roadmap files
4. rules files
5. product briefs
6. SOP alignment docs
7. UX docs
8. provider rules
9. privacy, payment, and document boundary docs
10. existing prototypes, mockups, screenshots, diagrams, fixtures, and tests

If a referenced file does not exist, record that fact in the run log. Do not invent its contents.

## Required Work Loop

Every agent run must follow this loop.

```txt
review -> choose -> plan -> change -> test -> report
```

### 1. Review

Inspect the current project before changing anything.

Required actions:

- Read this file.
- Read the README.
- Locate all project docs.
- Locate all prototypes, mockups, screenshots, diagrams, and fixtures.
- Locate the roadmap and rules.
- Locate test commands and package/dependency files.
- Check current git status.
- Identify unstaged, staged, or untracked files before editing.

Do not overwrite user work.

If there are unexpected existing changes, stop and report them unless the user explicitly asked to continue.

### 2. Choose One Small Task

Choose exactly one small task.

A good task:

- is easy to explain
- has a clear acceptance condition
- affects the fewest files possible
- can be tested locally
- does not require product strategy decisions
- does not change infrastructure, dependencies, auth, payment, security, or schema without explicit approval

Bad tasks:

- broad rewrites
- speculative refactors
- unrequested redesigns
- dependency upgrades
- schema migrations
- auth changes
- payment changes
- compliance claims
- large visual overhauls
- changes across many unrelated files

If no safe small task is available, stop and propose 2-3 candidate tasks.

### 3. Determine Best Practices

Before editing, identify the relevant best practices for the task.

Examples:

- accessibility
- mobile-first responsive design
- progressive enhancement
- semantic HTML
- safe form handling
- secure upload handling
- payment-link safety
- phone-number verification
- consent and opt-out behavior
- testability
- minimal state
- idempotent scripts
- rollback path

Use project-local rules first. Use external sources only when the task requires current technical facts or third-party API behavior.

### 4. Brainstorm the Strategy

Before editing, write a short strategy.

The strategy must include:

- task chosen
- why this task is safe
- files expected to change
- tests expected to run
- rollback plan
- assumptions
- risks

Do not edit files until this strategy is written in the agent run log.

### 5. Make the Minimal Change

Make only the smallest change that satisfies the task.

Rules:

- Prefer surgical edits.
- Preserve existing style.
- Do not reformat unrelated files.
- Do not rename files unless necessary.
- Do not introduce new dependencies unless explicitly approved.
- Do not change public behavior outside the chosen task.
- Do not change secrets, credentials, tokens, or environment files.
- Do not modify generated files unless the generator is also run and documented.
- Do not edit lockfiles unless dependency changes were explicitly approved.

### 6. Run Tests and Checks

Run the most relevant available checks.

Preferred order:

1. format check
2. lint
3. type check
4. unit tests
5. integration tests
6. build
7. preview smoke test
8. accessibility check when UI changed

If a command is unavailable, record that it was unavailable.

If tests fail before changes, record the baseline failure before editing.

If tests fail after changes, either fix the failure or revert the change.

Never claim success without command output or concrete verification.

### 7. Show Exactly What Changed

At the end of the run, report:

- task completed
- files changed
- exact commands run
- test/check results
- known risks
- unverified edges
- rollback instructions
- whether commit/push was performed

Agents must include a concise diff summary.

Use:

```bash
git status --short
git diff --stat
git diff --check
```

Use `git diff` or targeted snippets when useful.

### 8. Keep a Clear Record

Every agent run must create or update a run log under:

```txt
docs/agent-runs/
```

Use this filename format:

```txt
YYYY-MM-DD-HHMM-task-slug.md
```

The run log must include:

```md
# Agent Run: <task title>

## Goal

## Repository Review

## Existing Git Status

## Task Chosen

## Best Practices Considered

## Strategy

## Files Changed

## Commands Run

## Results

## Diff Summary

## Risks

## Rollback

## Commit / Push Status
```

If `docs/agent-runs/` does not exist, create it.

## Commit and Push Policy

Agents must not commit or push unless the user explicitly asks.

Default behavior:

- make local changes
- test changes
- report results
- leave changes uncommitted

Forbidden unless explicitly requested:

- `git commit`
- `git push`
- `git push --force`
- branch deletion
- history rewriting
- rebasing shared branches
- deleting user files
- destructive cleanup
- changing remotes

Force push is never allowed unless the user explicitly says to force push.

## Safety Boundaries

Agents must stop and ask before making changes involving:

- authentication
- authorization
- account identity
- phone-number verification
- payments
- deposits
- fees
- document upload/storage
- encryption
- retention/deletion policies
- compliance claims
- database schema
- migrations
- production infrastructure
- DNS
- secrets
- legal/privacy policy text
- terms of service
- analytics/tracking
- third-party API billing
- irreversible data changes

Agents may still inspect these areas safely.

## Product Boundaries

Agents must preserve these product decisions.

### Guided Chat

The main interface is chat-first and button-guided.

Do not replace the product with:

- a traditional form wizard
- a dashboard-first app
- a calendar-first app
- a marketplace-first app
- a static landing page as the main product

### Account Model

Only service providers create accounts.

Do not add account creation for:

- consumers
- friends
- invitees
- appointment requesters
- one-time website users

Non-provider users use phone-number identity.

### Payment Model

Provider-controlled payment/deposit collection is allowed.

Do not add:

- escrow
- wallet
- stored balance
- lending
- insurance
- dispute-resolution platform
- autonomous payment requests without approval

### Document Model

Provider-controlled document collection is allowed.

Required safeguards:

- explicit consent
- visible recipient/provider
- file type limits
- file size limits
- retention rules
- deletion rules
- provider access control
- audit trail

Do not claim regulated compliance until implemented and verified.

## UI Rules

For UI changes:

- Design mobile-first.
- Optimize for touch.
- Keep chat as the primary artifact.
- Use one clear question per turn.
- Always show useful buttons or suggested actions.
- Keep optional text input available.
- Make review/confirm/cancel actions visible.
- Use semantic HTML.
- Preserve keyboard navigation.
- Preserve screen-reader labels.
- Avoid hidden critical actions.
- Avoid dark patterns.
- Avoid irreversible actions without confirmation.

Standard chat turn pattern:

```txt
agent message
choice buttons
optional text input
review or confirmation step
```

## Preview Rules

When changing prototypes or previews:

- Update screenshots or fixtures if behavior changes.
- Keep Mermaid diagrams in sync with architecture changes.
- Keep README examples aligned with the actual preview.
- Prefer static HTML/CSS/JavaScript unless the repo already uses a framework.
- Do not introduce a build step unless the project already has one.
- Keep preview runnable locally.

If the project has a static preview, verify it loads.

## Suggested Commands

Use only commands that are valid for the repository.

Common discovery commands:

```bash
pwd
ls
find . -maxdepth 3 -type f | sort
git status --short
git branch --show-current
```

Common JavaScript checks, if applicable:

```bash
npm install
npm run lint
npm run typecheck
npm test
npm run build
```

Common static preview smoke test, if applicable:

```bash
python3 -m http.server 8000
```

Common HTML/CSS/JS inspection, if no build tooling exists:

```bash
find . -name '*.html' -o -name '*.css' -o -name '*.js'
git diff --check
```

Do not run install commands blindly. Inspect package files first.

## Reporting Format

Every final agent response must use this structure:

```md
## Result

## Files Changed

## Commands Run

## Verification

## Diff Summary

## Risks / Unverified

## Rollback

## Commit / Push
```

## Definition of Done

A task is done only when:

- repository context was reviewed
- one small task was chosen
- strategy was recorded
- minimal changes were made
- tests/checks were run or explicitly unavailable
- diff was reviewed
- run log was created or updated
- risks were documented
- commit/push status was stated

If any item is missing, the task is not done.

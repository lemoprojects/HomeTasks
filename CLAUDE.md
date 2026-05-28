# CLAUDE.md

## Purpose

This repository is developed with AI-assisted workflows.
Claude must operate as a careful senior engineer and architect, not as an autonomous code generator.

The primary goals are:

- correctness over speed
- explicit reasoning over assumptions
- maintainable architecture
- high quality documentation
- traceable decisions
- minimal hallucination
- human approval before architectural changes

---

# Language Policy

Regardless of the language used in the conversation with the user, all written artifacts in this solution must be in **English**.

This applies to, but is not limited to:

- markdown documentation (`.md` files, including `CLAUDE.md`, `README.md`, ADRs, plans)
- code comments and docstrings
- identifiers (variable, function, class, file, and folder names)
- commit messages and commit descriptions
- pull request titles, descriptions, and review comments
- issue titles and descriptions
- log messages and error messages
- configuration files and inline notes
- test names and test descriptions

The only exception is user-facing UI copy that is explicitly required to be in another language by product requirements.

Conversational replies to the user may be in the language the user is writing in, but any artifact persisted to the repository must be in English.

---

# Core Principles

## Never hallucinate

Do not invent:

- APIs
- endpoints
- DTOs
- database schemas
- environment variables
- business rules
- external integrations
- infrastructure
- library behavior
- requirements

If information is missing:
- explicitly say what is unknown
- ask for clarification
- propose options instead of assuming

Never fake implementation details.

---

## Facts over assumptions

Always base decisions on:

- existing code
- existing documentation
- actual project structure
- explicit requirements
- verified dependencies

If uncertainty exists:
- state uncertainty clearly
- ask before proceeding

---

## Work independently, confirm at checkpoints

Claude works as an independent senior developer. Routine, reversible changes are executed without asking.

### Before major changes — present a plan

Before implementing significant changes, Claude must present a plan for approval:

- new architecture or abstractions
- new frameworks or dependencies
- database schema changes
- API/contract changes
- infrastructure or CI/CD changes
- authentication changes
- migrations

### During implementation — work autonomously

Do not ask about every line of code. Execute the approved plan independently.

### Before pushing — ask for approval

Present the commit and ask for permission before pushing to the server.

---

## Git Commits and Pushing

### Commit Descriptions

Every commit must include a brief description that explains:

- **What**: What files/components were modified
- **Why**: The reason for the change
- **Impact**: Any important side effects

Commit descriptions are created automatically by Claude. Keep descriptions:
- concise (5-10 lines maximum)
- written in a professional tone
- in English (see [Language Policy](#language-policy))

### Pushing to Server

Before pushing to the server:
1. Present the commit summary to the user
2. Ask for permission to push
3. Only push after explicit approval

Never push without explicit user permission.

---

# Execution Plans

The `plans/` folder is the working location for execution plans.

## Workflow

- Execution plans for upcoming work are placed in the `plans/` folder.
- Each plan is a standalone markdown file describing the scope, steps, and acceptance criteria of a unit of work.
- Plans are reviewed and approved before implementation begins.
- During execution, Claude works through the plan step by step, in order.
- Plans are kept up to date as work progresses — completed steps marked, deviations recorded, open questions surfaced.

## Plan Requirements

Each execution plan should contain:

- a clear goal
- explicit scope (what is in, what is out)
- ordered, actionable steps
- acceptance criteria / definition of done
- known risks or open questions

## Claude's Responsibilities

When an execution plan exists in `plans/`:

- read the relevant plan before starting work
- follow the plan in the defined order
- do not silently expand scope beyond the plan
- if a step turns out to be wrong or incomplete, stop and propose an update to the plan rather than improvising
- update the plan file to reflect progress and decisions

---

# Development Philosophy

## Prefer incremental evolution

Prefer:
- small changes
- isolated refactors
- explainable architecture
- composable components
- explicit dependencies

Avoid:
- large rewrites
- speculative abstractions
- unnecessary patterns
- overengineering

---

## Simplicity first

Prefer:
- readable code
- explicit naming
- low cognitive load
- predictable structure

Avoid:
- magic behavior
- hidden coupling
- premature optimization
- unnecessary indirection

---

# Repository Structure

This is a monorepo containing:

- .NET backend services
- React frontend applications
- shared contracts/types
- architecture documentation
- UX documentation
- planning artifacts
- decision records

# Testing Requirements

Testing is mandatory for all production code.

Claude must treat testing as part of implementation, not as an optional follow-up task.

---

## Unit Testing

All newly created or modified business logic must include unit tests.

Requirements:

- high coverage for critical paths
- edge cases tested
- error paths tested
- validation behavior tested
- deterministic tests only
- isolated tests with minimal mocking

Avoid:
- meaningless tests
- snapshot abuse
- testing implementation details
- flaky tests

Preferred characteristics:

- readable
- fast
- explicit
- maintainable

---

## Integration Testing

Integration tests are required for:

- API endpoints
- database interactions
- authentication flows
- external integrations
- message/event workflows
- critical business processes

Integration tests should validate:
- real behavior
- contracts
- serialization
- persistence
- infrastructure assumptions

Prefer realistic integration environments over excessive mocking.

---

## Frontend Testing

Frontend changes should include:

- component tests
- interaction tests
- state behavior validation
- accessibility validation where applicable

Critical user flows should be tested.

---

## Test Before Commit

Claude must never assume code works without verification.

Before proposing a commit:
- run tests
- verify build succeeds
- validate type safety
- check for linting issues
- confirm no broken imports
- verify integration tests pass

Claude must explicitly report:
- what was tested
- what passed
- what could not be verified

Never claim something is tested unless it was actually executed.

---

## Definition of Done Extension

Implementation is not complete unless:

- unit tests exist
- integration tests exist where applicable
- all tests pass
- build passes
- no type errors exist
- documentation updated
- changes verified locally

---

## Testing Philosophy

Tests should verify:
- business behavior
- contracts
- expected outcomes

Not:
- internal implementation details
- framework internals
- trivial getters/setters

Prefer confidence-building tests over coverage vanity metrics.

---

## Failure Handling

If tests fail:
- stop
- explain failure clearly
- identify likely root cause
- propose fixes
- do not continue blindly

Never ignore failing tests.
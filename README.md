# HomeTasks

Monorepo for the HomeTasks product. See [CLAUDE.md](CLAUDE.md) for AI-assisted workflow rules and engineering principles that apply to this repository.

## README index

This repository uses a per-area `README.md` convention. Each `README.md` is the entry point for its folder and documents purpose, local usage, and conventions specific to that area.

| Path | Scope |
|---|---|
| [README.md](README.md) | Repository root — overview and index of all READMEs |
| [docs/README.md](docs/README.md) | Documentation index — architecture, ADRs, decisions |
| [docs/adr/README.md](docs/adr/README.md) | Architecture Decision Records — list, statuses, template |
| [apps/backend/README.md](apps/backend/README.md) | .NET backend services — local run and structure |
| [apps/web/README.md](apps/web/README.md) | React web application — local run and notes |
| [apps/mobile/README.md](apps/mobile/README.md) | Mobile client — local run and stack notes |
| [apps/integration tests/README.md](apps/integration%20tests/README.md) | Integration and end-to-end tests |
| [packages/shared/README.md](packages/shared/README.md) | Shared contracts, DTOs, and generated types |

## Conventions

- Each significant area of the monorepo has its own `README.md`.
- A `README.md` in a folder documents that folder; it does not duplicate global rules.
- Global rules and AI workflow are defined in [CLAUDE.md](CLAUDE.md).
- All written artifacts are in English (see Language Policy in [CLAUDE.md](CLAUDE.md)).
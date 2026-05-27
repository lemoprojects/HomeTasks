# Shared packages (contracts / types)

Purpose
- Store API contracts, DTOs, and shared types used by backend and frontend.

Recommended contents
- OpenAPI / Swagger specs
- protobuf definitions (if using gRPC)
- generated DTOs / clients for C# and TypeScript

Local usage
- Keep a single source-of-truth spec and generate language-specific artifacts into consumers.
- Example: generate TypeScript types for `apps/web` and C# DTOs for `apps/backend`.

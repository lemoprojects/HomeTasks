# Integration Tests

Purpose
- Integration and end-to-end tests that cover interactions between services and clients.

Local run (example):

```bash
cd "apps/integration tests"
# run test runner or orchestration (docker-compose, test scripts)
./scripts/run-integration-tests.sh
```

Notes
- Keep tests deterministic and isolated where possible.
- Prefer realistic integration environments as described in `../../CLAUDE.md`.

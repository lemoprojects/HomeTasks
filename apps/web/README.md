# Web (React)

Purpose
- Frontend web application built with React.

Local run (example):

```bash
cd apps/web
npm ci
npm start
# tests (if present)
npm test -- --watchAll=false
npm run build
```

Notes
- Ensure generated types from `../../packages/shared` are available before running.
- Keep component tests and accessibility checks as required by `../../CLAUDE.md`.

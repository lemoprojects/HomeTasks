# ADR-0009: Frontend (web) — Cloudflare Pages

- **Status:** Accepted
- **Date:** 2026-05-28
- **Deciders:** Piotr Glowacki

## Context

Choice of hosting for the web app (React, administration panel — [ADR-0003](0003-web-admin-only-scope.md)).

The web frontend is static (SPA) — it builds to HTML/JS/CSS files and can be served from any CDN.

## Decision

**Cloudflare Pages**, automatically deployed from the GitHub repository.

Cloudflare is also used for DNS, WAF, DDoS protection, and optionally a tunnel to the backend.

## Considered alternatives

- **Azure Static Web Apps** — comparable, but Cloudflare has a more generous free tier and better edge performance.
- **Vercel** — excellent DX for Next.js, but for plain React Vite/CRA, Cloudflare is just as good. Free tier comparable.
- **Netlify** — comparable. Chose Cloudflare for stack consistency (DNS + WAF + CDN + Pages in one place).
- **Backend serving the frontend (Azure Functions with static content)** — rejected; wastes API invocations on serving static files.

## Consequences

### Positive
- **Free tier fully sufficient** (unlimited static, 500 builds/month, 100k requests/day)
- Global edge CDN — low latency for users in Europe
- DNS, WAF, DDoS protection in the same dashboard
- Automatic SSL/TLS
- Preview deployments per PR
- Custom domains free

### Negative / Trade-offs
- Frontend and backend are in **different clouds** (cross-cloud) — CORS must be set up correctly on the backend
- Cloudflare Pages Functions (edge functions) use the Workers runtime, which does not support .NET — if we wanted a BFF/middleware near the frontend, we would have to write it in TypeScript/JS

## Related

- [ADR-0003](0003-web-admin-only-scope.md) — web scope
- [ADR-0005](0005-backend-azure-functions-consumption.md) — backend (cross-cloud, CORS required)

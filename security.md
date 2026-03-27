---
description: Security checklist — secrets, validation, XSS, environment
alwaysApply: false
---

# Security Checklist

## Secrets

- **No hardcoded secrets** — no API keys, tokens, passwords, or connection strings in source code
- Environment variables via `.env` files (gitignored) or CI/CD secrets
- If you spot a secret in code, flag it immediately and suggest `.env` extraction

## Input Validation

- Validate **all** external input (API responses, URL params, form data) with Zod schemas
- Never trust client-side validation alone — it's UX, not security
- Sanitize before rendering: escape HTML in user-generated content

## XSS Prevention

- Never use `dangerouslySetInnerHTML` without sanitization (use `DOMPurify` if needed)
- Avoid string interpolation in URLs — use `URL`/`URLSearchParams` constructors
- CSP headers should be configured at the server/proxy level

## Dependencies

- Review new dependencies before adding: check maintenance, bundle size, known vulnerabilities
- Keep `pnpm audit` clean — address high/critical vulnerabilities promptly
- Prefer well-maintained packages from the existing stack over new additions

## Auth & Network

- Never store tokens in `localStorage` — prefer `httpOnly` cookies or in-memory
- API calls go through the configured client (with auth interceptors), not raw `fetch`
- Log out / redirect on 401 — don't silently swallow auth failures

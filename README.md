# NOSINT — Secure OSINT Investigator Console

A runnable, login-protected web application that takes investigative selectors (name, email, phone, handle) as input and returns a correlated intelligence profile — built as a **functional prototype with production-grade security** and a demonstration (synthetic) data engine.

> Designed to show how authorized, auditable open-source-intelligence tooling should be built — **security and oversight first**, data integrations second.

## Why it's interesting
Most OSINT demos ignore the hard part: doing it *lawfully and accountably*. NOSINT leads with the controls a real law-enforcement or compliance deployment would require, and keeps the data layer synthetic so the prototype is safe to run and share.

## Security — real, production-grade patterns
- **PBKDF2-HMAC-SHA256** password hashing (240,000 iterations, per-user salt)
- Server-side sessions with a **hashed** token delivered as an `HttpOnly`, `SameSite=Strict` cookie
- **Role-based access control** — investigator / supervisor / admin
- **Tier-based authorization gate** — sensitive (Tier 3) collection requires both an authorized case *and* supervisor+ role, enforced server-side
- Login **rate-limiting** with temporary lockout; **CSRF** double-submit protection
- **Append-only, hash-chained audit log** of every security-relevant action
- Security headers (CSP, `X-Frame-Options`, `nosniff`) and path-traversal protection

## Data — demonstrated (synthetic)
All intelligence results are **synthetic**, generated deterministically from the input selectors by the `ConnectorEngine` in `server.py`. No external source is contacted. In a real deployment each connector is replaced by an authorized, licensed integration — **nothing else in the app changes**.

## Run it
Requires **Python 3.8+ only — no dependencies** (standard library).

```bash
cd nosint-app
python3 server.py        # then open http://localhost:8000
```
A local SQLite database is created on first run, with seeded demo accounts (change before any real use). A Docker Compose + HTTPS pilot path is also included.

## Architecture
```
nosint-app/
├── server.py        # zero-dependency backend: auth, RBAC, audit, search API
└── static/
    └── index.html   # single-file frontend (login + console)
```

## Role
Sole designer and developer — security architecture, backend, frontend, and the compliance/oversight framework.

---
*For authorized, lawful use only.*

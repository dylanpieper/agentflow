---
paths:
  - "**/*.py"
  - "**/pyproject.toml"
  - "**/*.{ts,tsx,js,jsx}"
  - "**/package.json"
---

# Python and web stack

A project `CLAUDE.md` or `AGENTS.md` overrides this file.

## Python

- Use `uv` for Python versions, environments, dependencies, and scripts (`uv add`, `uv run`). Do not use `pip`, `poetry`, or `conda`.

## Web app stack

When you build a web app with an API and a UI, use this stack. For a Shiny or shinyreact app with an R server, use `~/.claude/rules/r.md` instead. For the app database, it overrides the data stack in the global `~/.claude/CLAUDE.md`. For analytical or file-based work, including work inside the app, use that data stack.

- Use FastAPI for the API and Pydantic for request and response schemas.
- Use PostgreSQL for the database. Use SQLAlchemy 2.0 with typed `select()` queries, and Alembic for migrations.
- Use React for the frontend and Node for the frontend tooling.

## Layer separation

In a web app, keep each layer distinct. A layer calls only the layer below it. Never import from a layer above.

| Layer | Owns | Must not |
|-------|------|----------|
| Frontend (React) | UI, client state, HTTP calls to the API | Contain business rules or touch the database |
| Routes (FastAPI routers) | HTTP: parse, validate, call a service, return a response | Contain SQL or business rules |
| Services | Business rules as plain functions | Import FastAPI or know about HTTP |
| Repositories | All SQLAlchemy queries | Contain business rules |
| Models and migrations | SQLAlchemy tables and Alembic history | Contain logic |

- Pydantic schemas are the shared API contract. Routes and services can import them. Keep them separate from SQLAlchemy models, and map between the two in the service layer.
- A service gets its repository as an argument. The one exception to the layer order: dependency providers in `api/deps.py` build repositories, sessions, and settings. Route handlers get them with FastAPI `Depends` and give them to services. Handlers do not call repository methods. Do not use global sessions.
- Use this layout: `backend/app/{api,services,repositories,models,schemas}/` and `frontend/`.
- Test each layer alone. Test services with a fake repository. Test repositories against a real PostgreSQL test database.

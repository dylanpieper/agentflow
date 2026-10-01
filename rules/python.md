---
paths:
  - "**/*.py"
  - "**/pyproject.toml"
  - "**/*.{ts,tsx,js,jsx}"
  - "**/package.json"
---

# Python and web stack

## Stack

- Use `uv` for Python versions, environments, dependencies, and scripts (`uv add`, `uv run`). Do not use `pip`, `poetry`, or `conda`.
- Use FastAPI for the API and Pydantic for request and response schemas.
- Use PostgreSQL for the database. Use SQLAlchemy 2.0 with typed `select()` queries, and Alembic for migrations.
- Use React for the frontend and Node for the frontend tooling.

## Layer separation

Keep each layer distinct. A layer calls only the layer below it. Never import from a layer above.

| Layer | Owns | Must not |
|-------|------|----------|
| Frontend (React) | UI, client state, HTTP calls to the API | Contain business rules or touch the database |
| Routes (FastAPI routers) | HTTP: parse, validate, call a service, return a response | Contain SQL or business rules |
| Services | Business rules as plain functions | Import FastAPI or know about HTTP |
| Repositories | All SQLAlchemy queries | Contain business rules |
| Models and migrations | SQLAlchemy tables and Alembic history | Contain logic |

- Keep Pydantic schemas (the API contract) separate from SQLAlchemy models (the storage). Map between them in the service layer.
- Give sessions and settings to routes with FastAPI `Depends`. Do not use global sessions.
- Use this layout: `backend/app/{api,services,repositories,models,schemas}/` and `frontend/`.
- Test each layer alone. Test services with a fake repository. Test repositories against a real PostgreSQL test database.

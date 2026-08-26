# AGENTS.md
<!-- → project root. Loaded from CWD at session start; subdirectory copies load lazily.
     Project facts and rules only — personality lives in ~/.hermes/SOUL.md. -->

## What this is
<!-- one paragraph: what the project does, who uses it -->
A FastAPI service backing the mobile app. Postgres, deployed on Fly.

## Layout
- `app/main.py` — entrypoint
- `app/routers/` — HTTP surface
- `app/services/` — business logic; put new logic here, not in routers
- `tests/` — pytest
- `migrations/` — Alembic

## Conventions
- Dependencies via `uv`. Never hand-edit `requirements.txt`.
- Tests: `pytest -q`. New behavior needs one test.
- Format/lint: `ruff format . && ruff check --fix .` before committing.
- Type hints on anything public.

## Hard rules
- Never commit `.env`, `secrets/`, or anything matching `*_key*`.
- Never modify `migrations/` unless explicitly asked.
- Never run `alembic upgrade` against anything but local.
- Don't add a dependency without saying why in the same message.

## Useful commands
```bash
uv run uvicorn app.main:app --reload   # dev server
pytest -q                              # tests
fly deploy --config fly.staging.toml   # staging
```

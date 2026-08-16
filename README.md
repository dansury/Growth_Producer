# autopuplicator

AI Autopublisher Bot — Telegram-driven content pipeline.

Specs: see `spec.md` (navigation index), `BRD.md` (business requirements), `METHOD.md` (content methodology), `DEV_PLAN.md` (phased execution).

## Repository layout

```
src/         Python package (bot, vault, interview, tools, agents, graph, analytics)
tests/       pytest suite
config/      runtime configs (channel_config.json, model_prices.json, ...)
scripts/     one-off dev / ops scripts
spec/        per-module specs (read one at a time per CLAUDE.md)
sources/     reference materials from related projects
changelog/   per-cycle change logs
```

## Dev setup

Requires **Python ≥ 3.11**. The project uses [uv](https://docs.astral.sh/uv/) by default.

```bash
uv sync --extra dev
uv run pytest
```

Or with `pip`:

```bash
python -m venv .venv
source .venv/bin/activate
pip install -e ".[dev]"
pytest
```

### Session bootstrap (auto in Claude Code)

`.claude/settings.json` SessionStart hook runs `scripts/setup_session.sh` on every fresh container — it installs Python dev-deps (idempotent). Run manually if needed:

```bash
bash scripts/setup_session.sh
```

### Reference sources (on-demand)

`sources/gbrain/` and `sources/openclaw/` are gitignored and NOT cloned automatically. Fetch them when you need them:

```bash
bash scripts/fetch_sources.sh         # both
bash scripts/fetch_sources.sh gbrain  # one
```

## Working on the codebase

Read `CLAUDE.md` first. The spec-driven workflow is:

1. Pick a task from `TODO.md` (ordered by `DEV_PLAN.md`).
2. Read exactly one `/spec/<module>.md`.
3. Update the spec if the change requires design updates.
4. Implement.
5. Append an entry to `changelog/changes-{YYYY-MM-DD}-{HH-MM}.md`.

The `/develop` skill (`.claude/skills/develop/SKILL.md`) automates one cycle of this loop.

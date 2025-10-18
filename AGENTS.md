# Repository Guidelines

## Project Structure & Module Organization
- `src/server.py` exposes the MCP entrypoint that wires prompt templates, Mealie clients, and registered tools. Treat this as the primary runtime surface.
- `src/mealie/` contains thin HTTP client wrappers for Mealie domains (`recipe.py`, `mealplan.py`, `group.py`, `user.py`) that translate REST responses into Python objects.
- `src/tools/` defines the MCP tools surfaced to clients; extend these when adding new agent capabilities.
- `src/models/` holds shared Pydantic schemas used across tools and clients; keep schemas in sync with Mealie’s OpenAPI spec (`openapi.json`) when updating.
- Environment samples live in `.env.template`; copy to `.env` when running locally. Docker assets reside under `docker/`.

## Build, Test, and Development Commands
- `uv sync` installs runtime and dev dependencies declared in `pyproject.toml`.
- `uv run mcp dev src/server.py` launches the MCP dev inspector for interactive testing against Claude Desktop.
- `uv run python -m tools.recipe_tools` is a quick way to smoke-test recipe tool wiring without a client UI.
- `uv run black src` and `uv run isort src` format imports and code; run before pushing to keep style consistent.

## Coding Style & Naming Conventions
- Use Black defaults (88-character lines, double quotes, 4-space indentation); avoid manual line wrapping unless readability degrades.
- Imports should be sorted with `isort`'s Black profile; keep third-party libraries (`httpx`, `pydantic`, `mcp`) grouped.
- Favor descriptive, lowercase module names (`recipe_tools.py`) and PascalCase Pydantic models. Tool names exposed via MCP should remain kebab-case to match existing patterns.

## Testing Guidelines
- The project currently relies on manual validation; when adding automated tests, place them under `tests/` mirroring the `src/` layout.
- Use `pytest` with `uv run pytest` to execute suites; name test files `test_<module>.py` and use descriptive test function names (`test_recipe_search_returns_matches`).
- Mock network calls to Mealie to avoid leaking credentials and to keep runs deterministic.

## Commit & Pull Request Guidelines
- Commits in history (`Add MseeP.ai badge to README.md`, `Upgrade to mcp 1.12.0`) follow short, imperative summaries (~50 chars). Reference issues with `#<id>` when relevant.
- For PRs, provide: goal-oriented description, configuration changes (`.env`, Docker), screenshots or CLI transcripts for new tools, and links to Mealie docs if behavior depends on upstream versions.
- Ensure `AGENTS.md`, `README.md`, and any new tool docs stay aligned; call out required env vars and new MCP capabilities so downstream agents stay informed.

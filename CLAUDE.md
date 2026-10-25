# CLAUDE.md

All project instructions live in AGENTS.md (shared by every coding agent) and are imported here:

@AGENTS.md

## Claude Code specifics

- **context7:** use the `mcp__context7__resolve-library-id` and `mcp__context7__query-docs` tools for every library lookup, as required by `docs/guidelines_rules.md` §2. If context7 is unavailable, use the library's official docs and say so.
- **Docs to read:** before the first code change in a session, read the four docs listed in AGENTS.md. Re-read the current phase in `docs/phases.md` when starting a new task.
- **Plans:** for multi-file changes, state the files you will touch and the tests you will add before editing.
- **Verification:** after changes, run the relevant `make` targets and report the actual output. Never claim a phase criterion is met without evidence.

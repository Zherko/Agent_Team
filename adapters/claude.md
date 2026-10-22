# Adapter claude-code

- Runtime: claude-code (Anthropic)
- Agente plan: `Explore` + `Plan` (read-only) — mapea a `plan` nativo opencode
- Skills: `SKILL.md` genérico vía `Read` tool, frontmatter `compatibility: claude-code`
- Paths: mismo `.opencode/goals/<slug>/` o fallback `.claude/goals/` si detecta `CLAUDE_CODE=1`, con symlink env `GOAL_DIR`
- Discovery: polyfill BM25 sobre `~/.config/opencode/skills/team/` + cache `.tmp/find-skills-team.json` (degrada graceful offline)
- Logger: `logger/index.js` usa `fs` Node genérico (sin api opencode), hash chain igual
- Tests: mismo harness L0-L3, Playwright si UI
- Feature-detect: `if (process.env.CLAUDE_CODE) use claude adapter else opencode`

Comandos: `/team` mapea a `/goal` en claude slash.

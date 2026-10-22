# Adapter codex

- Runtime: codex (OpenAI)
- Agente plan: `codex plan` read-only (sin write) — mapea a `plan` nativo opencode
- Skills: `AGENTS.md` + `SKILL.md` genérico via `read` tool, frontmatter `compatibility: codex`
- Paths: `.opencode/goals/<slug>/` o `.codex/goals/` fallback via `CODEX_RUNTIME` env, hash por contenido no fecha igual que Goal
- Discovery: polyfill BM25 + registry_hash local, stateless, TTL 24h, output `.tmp/find-skills-team.json` efímero
- Logger: Node fs genérico, hash chain identico, verify gaps
- Tests: mismo harness L0 inversión (revierte patch), L1 AST, L2 fast-check 100
- Feature-detect: `if (process.env.CODEX) use codex adapter`

Comandos: `/team` → codex skill invoke.

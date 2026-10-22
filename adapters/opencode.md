# Adapter opencode (source of truth)

- Runtime: opencode
- Agente plan: `plan` nativo (edit:deny, bash:deny)
- Skills: `skill({name:"goal-plan"})` + `goal-discovery` stateless
- Paths: `.opencode/goals/<slug>/`, `LOG.jsonl` hash chain en `logger/index.js`
- Discovery cache: `~/.cache/opencode/find-skills/` TTL 24h
- Tests: L0 inversión + L1 AST + L2 propiedad/Playwright + L3 mutación

Este adapter mapea FASE -1..F2 tal cual SKILL.md 1:1.

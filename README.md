# Agent_Team — clon universal de Goal

> Orquestador universal opencode + claude-code + codex con plan seccional y comité al atrancarse. Clon 1:1 de Goal validado 25/25.

## Instalación
- **Opencode:** copia SKILL.md + gent.md a ~/.config/opencode/skills/team/ y ~/.config/opencode/agents/team.md
- **Claude Code:** compatibility: claude-code via dapters/claude.md (Explore/Plan read-only)
- **Codex:** compatibility: codex via dapters/codex.md

## Validación 1:1 vs Goal
`
node lab-opencode/runs/2026-10-22-team-universal/harness.mjs  # 25/25 verde
node lab-opencode/runs/2026-10-22-team-universal/test-polyfill.mjs  # 7/7
`
- Goal intacto 1ACB4CEE, Team 0ae1698, LOG.jsonl 6/6 gaps 0
- L0 inversión 22→25 falsable, flaky 3x 0%

## Estructura
- SKILL.md — workflow FASE -1→2 (115 líneas)
- gent.md — picker (Team junto a build/plan/goal)
- dapters/ — opencode/claude/codex
- ENJAMBRE-RULES.md — paralelización
- mini-skills/ — plan/discovery/comite/pro/research/test
- logger/ — LOG.jsonl hash chain
- esearch/controller.md, lock.schema.json, STATE.schema.json

## Licencia
MIT — derivado de Goal.


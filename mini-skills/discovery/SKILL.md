---
name: team-discovery
description: "Descubrimiento interno universal (opencode/claude/codex) — BM25 + cache TTL 24h, stateless. Polyfill para FASE 0."
license: MIT
compatibility: opencode, claude-code, codex
metadata:
  internal_for: "team"
  opencode/slash: "false"
  opencode/autoinvoke: "false"
---

# Skill: goal-discovery (interna, compartimentada)

Origen: comit� find-skills 2026-10-21 + comit� escalado. Vive dentro de Goal, no externa Vercel.

## Prop�sito
Buscar skill aplicable sin instalarla, stateless con cache single-flight, para FASE 0 condicional ANTES del plan (solo SMALL/LARGE).

## Cu�ndo usar
- FASE 0 clasifica >=SMALL y hay ~400 items en registro interno
- Query LARGE con alta incertidumbre, multi-stack o multi-archivo

## Cu�ndo NO
- TRIVIAL (=500 tok, 1 archivo) ? skip log 'skip: trivial'
- Cache hit TTL 24h ? usa ~/.cache/opencode/find-skills/

## Contrato
- Input: query string
- Output: .tmp/find-skills-<hash>.json {skills:[{id, confidence, registry_hash}], registry_hash}
- Invariantes: no muta block.json/STATE, output ef�mero, degrada graceful si offline/0 hits, coste 800-1500 tok solo en SMALL+
- Tests: P1 hit_rate@3, P2 dedup 0.9 single-flight, P3 content_hash invalidation, P4 recall negativo, P5 offline BM25

## Uso
\skill({name:"goal-discovery"})\ en FASE 0 ? si hit usa contexto read-only en planner, sin escribir block.json. Solo \goal-skill-add\ escribe.

## Archivo
Implementaci�n: skills/goal-discovery/SKILL.md + skills/goal-discovery/discovery.mjs (mock 400 + BM25 fallback)

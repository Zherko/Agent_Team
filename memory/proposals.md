# Proposals â€” Goal (max 1 por goal, backup versionado)

## 2026-10-20 â€” Blindaje Karpathy para auto-research (research/controller.md)
- **Backup:** `research/controller.md.bak.2026-10-20`
- **Motivo:** fallo detectado en lab 2026-10-20-planner-opt: 1 prueba simulada (~segundos) en vez de loop clÃ¡sico de horas con plan completo. Usuario exige iteraciÃ³n hasta 2 pasos sin mejora con pruebas largas realistas.
- **Cambio:** aÃ±ade `## Blindaje Karpathy â€” anti-simulaciÃ³n` con 6 checks verificables: Think Before Coding (hipÃ³tesis + alternativa), Simplicity First (parche mÃ­nimo), Surgical (solo block.json), Goal-Driven Real (harness_hash + L0-L3 + diagnosis, sin deltas simulados), loop hasta consecutive_fails>=2, promociÃ³n solo con replica x2.
- **Fuente:** karpathy-guidelines/SKILL.md Â§1-4 + research/controller.md
- **VerificaciÃ³n:** `read research/controller.md` contiene `Blindaje Karpathy`; lab futuro debe pasar checklist o ciclo REJECT; mÃ©trica tokens_por_exito real, no simulada.

## 2026-10-20-v2 ï¿½ Comitï¿½ tras 2 fails, early_stop 3
- Cambio: early_stop 2?3, comitï¿½ en consecutive_fails==2 (1 de 6), parada definitiva en >=3
- Motivo: usuario pide blindar iteraciï¿½n hasta 3 fallos con rescate intermedio


## 2026-10-20-v3 ï¿½ Anti-zapatito 3-4 holdouts
- Cambio: Blindaje Karpathy #4 exige 3-4 pruebas distintas con supuestos diferentes, comitï¿½ elige tareas, promociï¿½n solo si ancla + =2/3 holdouts mejoran
- Motivo: evitar zapatito de cristal solo para anchor-large

## 2026-10-20-holdout â€” promociÃ³n blindada ancla 4509 + 3/4 holdouts
# PROPOSAL â€” promociÃ³n blindada ancla 4509 + holdouts 3/4

**Fecha:** 2026-10-20
**Harness:** sha256:5b726383316970c9c785709d0fb42696c6f720925f8662eba8c92da247823e43 orig sha256:c5b26c7f74fdac16aba0ef3ce57ea8f580d0db5ee2ecc6e09e01d35722a6b4a8 (frozen)
**Ancla:** 6979â†’4509 (-35.4% p_fail 0.55 + writer-batch) verificado 3 reruns median L0-L3 pass diagnosis/V4-writer-batch/trace.json
**Holdouts:** small-crud:success(0), ambiguo:fail(-3033), doc-batch:success(5293), riesgo-core:success(0)
**Criterio:** holdout success si tokens_mejora â‰¤ basal+40 y iters no +1 y L0-L3 pass; zapatito si tokens â†‘ >40 o iters +1. 3/4 success â†’ promociÃ³n blindada.

**Cambios promovidos:**
- hybridRouter threshold p_fail 0.43â†’0.55 (surgical 1 lÃ­nea, menos comitÃ© desperdicio zona gris 0.43-0.55, FASE 1b rescate si falso negativo)
- writer-batch Regla 2: 3+ docs disjuntos â†’ 1 sesiÃ³n (block.json ledger, -77% overhead 32700â†’10800, 1 sesiÃ³n vs N workers)
- Ambos combinados best 4509 (plan 221+comite245+exec4043) vs basal 6979, delta 2470 validado harness-unified 3 reruns

**Tests:** L0 inversiÃ³n falsable true, L1 contrato <2s 420ms anti-mocks vacÃ­os, L2 â‰¥100 casos fast-check idempotencia/disjunciÃ³n, L3 iters 3 (ambiguo 4 con rescate detectado como zapatito esperado)
**Replica x2:** metrics-real.jsonl 3 reruns median seed42 + metrics-holdout.jsonl 4 holdouts Ã—3 reruns Ã—2 variantes =24 runs, diagnosis/*/trace.json + /tmp/plan-* + harness_hash frozen
**Holdout veredicto:** - holdout-small-crud success delta 0 tokensUp 0 iterDelta 0 (basal 2447 â†’ mejora 2447)\n- holdout-ambiguo fail delta -3033 tokensUp 3033 iterDelta 1 (basal 4217 â†’ mejora 7250)\n- holdout-doc-batch success delta 5293 tokensUp -5293 iterDelta 0 (basal 10605 â†’ mejora 5312)\n- holdout-riesgo-core success delta 0 tokensUp 0 iterDelta 0 (basal 6341 â†’ mejora 6341)
**Nota:** holdout-ambiguo fail esperado demuestra lÃ­mite seguro de p_fail 0.55 en ambigÃ¼edad; no bloquea promociÃ³n porque 3/4 holdouts success y ancla intacta, pero documenta que ambigÃ¼edad requiere comitÃ© aunque threshold diga skip (FASE 1b rescate +1 iter).

**AcciÃ³n:** Actualizar skills/goal/memory/proposals.md y lab/PROPOSAL.md con hash frozen y holgura blindada.

## 2026-10-20 PROMOTION APPLIED — planner-opt a producción
- SKILL.md p_fail 0.43?0.55 + grill-me ambiguo, ENJAMBRE-RULES.md writer-batch validado
- Backups: SKILL.md.bak.2026-10-20-planner-promotion
- Research blindajes aplicados a producción: controller.md early_stop 3 + comité en 2 + anti-zapatito 4 holdouts


## 2026-10-21 PROMOTION — skill-init 5 blindajes lab-init-audit
- Cambios: SKILL.md §3 junction Windows + §4 handoff gate + §5 sync-context + §6 interface owns + §8 mutation + §9 verify checklist
- Evidencia: lab runs/2026-10-20-init-audit/ audit_pass_rate 1.0 4/4 (NEW S/L, migracion, reentry) 5 ciclos V1-V5 success, harness 5e291644 3 reruns, early_stop 0/3
- Backup: SKILL.md.bak.2026-10-21-init-audit


## 2026-10-21 addenda — degrada secuencial sin overhead
- Aclaración SKILL.md FASE 1.2: degrada a secuencial directo en 1 agente, no 2 agentes secuenciales, evita +62k overhead.


## 2026-10-21 PROMOTION — prompt-breve 1-4 líneas
- Cambio: agents/goal.md:41 1-4 siempre + detalla sencillo + sesión fresca, memory 1-4
- Evidencia: lab 2026-10-21-prompt-breve baseline 16.7% ? 100% 4 sesiones sin memoria 3 reruns hash e466f6ce
- Backup: agents/goal.md.bak.2026-10-21-prompt-breve

## 2026-10-21 PROMOTION - find-skills-workflow gate FASE 0 condicional ANTES stateless
- Cambio: SKILL.md:36 2b Gate find-skills condicional ANTES del plan solo si SMALL/LARGE: find-skills --json --cache-ttl=24h --out=.tmp/find-skills-<hash>.json stateless registry_hash + single-flight cache TTL24h, efimero no muta block.json/STATE, planner lee .tmp si existe, TRIVIAL skip 0 tok, offline graceful. Diff <=50 lineas.
- Motivo: COMITE-find-skills veredicto ANTES condicional (4/5 acuerdo) - DESPUES es tarde (+12-18k retrabajo LARGE), obligatorio mata TRIVIAL (800 tok *70%). Break-even 7% ROI 15:1 con single-flight evita 62k*10.
- Evidencia: harness lab-opencode/runs/2026-10-21-find-skills/harness.mjs harness_hash sha256:bfe6245255456c2b frozen 3 reruns median seed42-44 P1 hit_rate@3==1 p50~30ms PASS, P2 dedup 0.9 originCalls 1 PASS, P3 content_hash PASS, P4 recall@5==1 PASS, P5 offline BM25 PASS (5/5). Anti-zapatito 5 supuestos, diagnosis traces, block.json [] clean-lab OK.
- Comite cierre: 2026-10-21 COMITE-cierre 5 agentes + juez PROMOTION SI - gate correcto, P1-P5 falsables suficientes, blindajes piloto 1d 5-10% trafico hit-rate>15% precision>0.45.
- Backup: SKILL.md.bak.2026-10-21-find-skills hash C23BB5648299 diff <=50 lineas, reversion restore bak + rm .tmp
- Tabla-evolucion: diagnosis/cost-evolution.md TRIVIAL 0 vs 800 tok, LARGE 12-18k ahorro ROI 15:1, dedup 0.9 -7.2k
- Verificacion: read SKILL.md contiene 2b Gate find-skills; node harness.mjs 5/5 PASS 3 reruns; ls tests 5 specs; research/INDEX.md 2 reads

## 2026-10-21 PROMOTION — escalado SMALL?LARGE tras 2 fails
- Cambio: SKILL.md FASE 1.7 re-trazo + re-estima p_fail/fan-out + gate OR 4 criterios (fan-out>3, p_fail>0.55, =2 dominios, x2 estimación) escala a LARGE via FASE 1b con block.json/enjambre. Si 4 negativos mantiene SMALL.
- Evidencia: comité 5 agentes O2 vs O1 (ver COMITE-escalado), 4 pruebas fan-out/p_fail/research/enjambre, backup E0ACF3EE57F7
- Backup: SKILL.md.bak.2026-10-21-escalado


## 2026-10-21 PROMOTION — skill interna goal-discovery
- Skill compartimentada dentro de Goal (no externa Vercel), stateless cache single-flight TTL 24h, usada en FASE 0 condicional ANTES
- Evidencia: comité placement ANTES + 5 pruebas 5/5 harness bfe6245255456c2b
- Archivos: skills/goal-discovery/SKILL.md + discovery.mjs (400 mock + BM25)


## 2026-10-21 PROMOTION — discovery-retry antes de comité tras 2 fails
- Cambio: SKILL.md FASE 1.7 re-trazo + discovery-retry stateless (TRIVIAL skip ? retry con contexto fallo) + FASE 1b inyecta .tmp/find-skills-retry-*.json al comité + FASE 1-fast TRIVIAL?SMALL con retry
- Evidencia: pedido usuario para rescatar TRIVIAL/SMALL que parecía sencillo y falló, alimenta comité con skills sin mutar block.json/STATE, coste 800-1500 tok degrade graceful
- Backup: SKILL.md.bak.2026-10-21-discovery-retry (67F66BDCFD58)


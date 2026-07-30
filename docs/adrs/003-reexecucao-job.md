# ADR-003 — Reexecução de job

**Status:** Aceito  
**Data:** 2026-07-30

## Contexto

Job em `falha` precisa poder rodar de novo. Job `concluido` reexecutado duplicaria inconsistências e confundiria a demo.

## Decisão

- Reexecução permitida apenas de `pendente` e `falha`.
- Ao reexecutar a partir de `falha`: remove eventos anteriores e inconsistências daquele `job_id`, depois roda limpo.
- `concluido` → `409`.

## Consequências

Histórico de falha se perde na reexecução. Aceitável no MVP; produção usaria nova tentativa versionada.

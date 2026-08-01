# ADR-005 — Dois repositórios de código + um de docs

**Status:** Aceito  
**Data:** 2026-07-30

## Contexto

A disciplina exige **dois** repositórios (API e front), cada um com README. Planejamento longo não cabe misturado no root da API sem poluir a correção.

## Decisão

- `blind-spot` → documentação e planejamento (este repo)
- `blind-spot-api` → implementação Flask
- `blind-spot-web` → implementação SPA

READMEs da api/web linkam de volta para este repo de docs.

## Consequências

Três URLs na entrega. Na mensagem ao professor, destacar os dois de código; docs como suporte/portfólio.

# ADR-004 — Varredura síncrona simulada

**Status:** Aceito  
**Data:** 2026-07-30

## Contexto

Worker real (fila, thread, Celery) foge do tamanho do MVP e complica o vídeo.

## Decisão

`POST /executar` roda na request HTTP, gera inconsistências em memória/persistência na mesma transação, responde com o resultado.

## Consequências

Não escala; timeout se N for absurdo. Limitar `quantidade` (max 20). Suficiente para demonstrar o fluxo job → achados.

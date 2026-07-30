# ADR-002 — Exclusão de job

**Status:** Aceito  
**Data:** 2026-07-30

## Contexto

Job pode ter inconsistências filhas. Apagar tudo sem critério quebra auditoria da demo; impedir delete atrapalha o RF de DELETE.

## Decisão

1. Se o job está `pendente` ou `cancelado` **e** não tem inconsistências: `DELETE` físico (cascata só em eventos).
2. Se o job já tem inconsistências ou está `concluido`/`em_execucao`: não apaga; retorna `409` sugerindo cancelar **somente** se `pendente`/`falha` sem filhos — para `concluido`, exclusão não é permitida no MVP.
3. Alternativa de limpeza: excluir inconsistências uma a uma e depois o job, ou usar seed que recria a base.

## Consequências

DELETE de job existe (rubrica) mas é conservador. O DELETE “fácil” para a nota fica também em inconsistência (`204`), sempre permitido.

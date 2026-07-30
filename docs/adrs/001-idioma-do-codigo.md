# ADR-001 — Idioma do código

**Status:** Aceito  
**Data:** 2026-07-30

## Contexto

O produto fala com usuário brasileiro (status, erros, UI). O código convive com portfólio em inglês (Node/Python).

## Decisão

- Identificadores de código (módulos, classes, funções) em inglês.
- Valores de domínio persistidos e mensagens de API em português (`em_analise`, `Competência é obrigatória.`).
- Nomes de colunas do banco em português snake_case alinhados ao dicionário de dados (`criado_em`, `parecer`) para o schema refletir o domínio do produto.

## Consequências

Leve mistura EN/PT. Aceitável no MVP acadêmico; documentado aqui para não “corrigir” no meio do caminho.

# Padrões, Clean Code, SOLID e PEPs — Cruzamento

## 1. Idioma do código (ADR-001)

Back-end em **inglês técnico** para módulos/classes (`JobService`, `ConflictError`) e **português** para mensagens de API e labels de domínio expostos ao usuário (`status: "em_analise"`, textos de erro).

Motivo: combina com o restante do portfólio Node/Python e mantém o produto falando a língua do usuário brasileiro.

## 2. SOLID no back-end

| Princípio | Aplicação |
|-----------|-----------|
| **S** | `JobService` orquestra job; `VarreduraGenerator` só gera inconsistências; controllers só HTTP |
| **O** | Novos tipos de inconsistência via registro/enum, sem reescrever o endpoint |
| **L** | Repositórios com interface estreita; testes usam fake in-memory se fizer sentido |
| **I** | Nada de “God repository”; `JobRepository` ≠ `InconsistenciaRepository` |
| **D** | Services dependem de abstrações/repos injetados na factory Flask |

## 3. Design patterns previstos

| Pattern | Onde |
|---------|------|
| **Factory** | `create_app()` Flask |
| **Repository** | Acesso a `Job`, `Inconsistencia` |
| **Service / Application service** | Casos de uso (executar, fechar) |
| **Strategy** (leve) | Políticas de geração de inconsistência (determinística vs. lista fixa de demo) |
| **DTO / Schema** | Validação de entrada (Marshmallow ou validação manual clara) |
| **State** (implícito) | Transições validadas em funções de domínio |
| **Facade** (front) | `api/client.js` esconde fetch/headers/erros |

Evitar pattern por vaidade. Se um arquivo de 40 linhas resolve, não inventa AbstractSingletonFactory.

## 4. Clean Code (checklist de PR)

- Nomes que dizem o porquê: `marcar_job_concluido` > `update2`
- Funções < ~40 linhas quando possível
- Early return em validação
- Sem comentários que repetem o código
- Sem código morto / prints de debug no main
- Commits pequenos com mensagem útil

## 5. PEPs relevantes

| PEP | Uso |
|-----|-----|
| PEP 8 | Estilo |
| PEP 257 | Docstrings só em APIs públicas de service |
| PEP 484 | Type hints em services/domain |
| PEP 518/621 | Opcional; `requirements.txt` basta no MVP |
| PEP 20 | Legibilidade > esperteza |

Ferramentas sugeridas na API: `ruff` ou `flake8` + `pytest`. Não obrigar black se o curso não pede — mas formatar consistente.

## 6. Front — disciplina sem framework

- Um “módulo” por arquivo; sem namespace global gigante (`window.app` mínimo).
- Separar: `client` (HTTP), `views` (DOM), `state` (dados em memória).
- Evitar jQuery.
- IDs estáveis só onde o JS precisa; preferir `data-` attributes.

## 7. Anti-padrões proibidos neste projeto

- Controller com SQL solto
- Cópia > 50% do exemplo de aula
- Comentários tipo “# cria o job” em cima de `criar_job`
- README gerado genérico (“This project was built with… AI…”)
- Variáveis `data1`, `temp`, `foo`

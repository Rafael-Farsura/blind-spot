# Requisitos não funcionais — Blind Spot

---

## RNF-01 — Separação cliente/servidor

Front e API em processos/repos distintos. Comunicação apenas via HTTP/JSON.  
**Origem:** restrição REST (Fielding) + rubrica da disciplina.

## RNF-02 — Interface uniforme (REST)

Recursos nomeados no plural, verbos HTTP semânticos, códigos de status coerentes (`200`, `201`, `204`, `400`, `404`, `409`, `500`). Payload JSON UTF-8.

## RNF-03 — Stateless

A API não guarda sessão de usuário. Cada request carrega o necessário. (Sem cookie de sessão no MVP.)

## RNF-04 — Camadas

Organização em camadas no back-end: apresentação (rotas) → aplicação/serviços → domínio → infraestrutura (ORM/SQLite). Front: UI → api-client → render.

## RNF-05 — Code on demand

O front carrega JS sob demanda lógica de telas (módulos por arquivo), sem bundler. Bootstrap e CSS próprios via `<link>` / CDN permitido para Bootstrap.

## RNF-06 — Execução do front

Abrir `index.html` no navegador deve funcionar. Sem Vite/Webpack/Live Server obrigatório. Penalidade da disciplina se depender de servidor de front.

## RNF-07 — CORS

API libera origem do front (`null` para `file://` e/ou `*` em ambiente acadêmico local). Documentado no README da API.

## RNF-08 — Persistência

SQLite em arquivo local (`blindspot.db`). Schema criado na subida da app (ou migration simples). Sem PostgreSQL no MVP.

## RNF-09 — Documentação de API

Swagger/OpenAPI obrigatório, com descrição por rota.

## RNF-10 — Qualidade de código

- Python: PEP 8, type hints onde ajudar, nomes em português do domínio ou inglês técnico consistente (escolher **um** e manter — ver ADR-001).
- JS: ES5/ES6 sem transpile; funções pequenas; sem framework SPA.
- Sem comentários óbvios; comentários só para decisão não trivial.

## RNF-11 — Testes

Cobertura mínima acordada no [plano de testes](09-plano-testes.md):
- domínio/serviços ≥ 80%
- rotas críticas com testes de API ≥ 70%
- front: checklist manual + smoke de integração documentado

## RNF-12 — Performance (MVP)

Execução de varredura síncrona < 2s para N ≤ 20 inconsistências geradas. SQLite local, volume de demo.

## RNF-13 — Segurança (mínimo acadêmico)

- Sem secrets no repositório.
- Validação de entrada em todas as rotas de escrita.
- SQL apenas via ORM (sem concatenar SQL cru).

## RNF-14 — Usabilidade

Feedback visual de loading/erro/sucesso. Formulários com labels. Status com cor/badge conforme design system.

## RNF-15 — Observabilidade local

Logs simples no stdout da API (nível INFO para request relevante e execução de job). Sem ELK.

## RNF-16 — Entrega acadêmica

- 2 repositórios GitHub públicos (api + web)
- README em cada um (instalação, run, exemplos)
- Vídeo ≤ 4 min com roteiro: objetivo → Swagger → front
- Reuso do exemplo de aula < 50%

## RNF-17 — Acessibilidade básica

Contraste legível, foco em botões, `alt` se houver ícones significativos. Não é WCAG completo.

## RNF-18 — Idempotência parcial

Seed de demo e reexecução de job documentados (quando permitido). Evitar duplicar inconsistências na mesma execução.

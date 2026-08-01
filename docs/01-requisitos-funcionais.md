# Requisitos funcionais — Blind Spot

Prioridade: **Must** (obrigatório no MVP) · **Should** (desejável se couber) · **Could** (só se sobrar tempo)

Cada RF traz critério de aceite testável.

---

## RF-01 — Criar job de varredura

**Prioridade:** Must  
**Descrição:** O sistema permite registrar um job com tipo, competência (mês/ano ou rótulo) e observação opcional. Status inicial: `pendente`.

**Aceite:**
- `POST /api/jobs` com JSON válido retorna `201` e o recurso criado.
- Campos obrigatórios ausentes retornam `400` com mensagem clara.
- Job aparece em `GET /api/jobs`.

---

## RF-02 — Listar jobs

**Prioridade:** Must  
**Descrição:** Lista todos os jobs, dos mais recentes para os mais antigos. Filtro opcional por status.

**Aceite:**
- `GET /api/jobs` retorna array (pode ser vazio).
- `GET /api/jobs?status=concluido` filtra corretamente.
- Front exibe jobs em lista ou cards.

---

## RF-03 — Detalhar job

**Prioridade:** Must  
**Descrição:** Exibe um job pelo id, incluindo eventos da execução e quantidade de inconsistências geradas.

**Aceite:**
- `GET /api/jobs/{id}` retorna `200` com payload completo.
- Id inexistente retorna `404`.

---

## RF-04 — Executar varredura do job

**Prioridade:** Must  
**Descrição:** Dispara execução **síncrona simulada** do job. A execução:
1. muda status para `em_execucao` e depois `concluido` (ou `falha`);
2. grava eventos em `job_evento`;
3. cria N inconsistências vinculadas ao job (N configurável, default 3).

**Aceite:**
- `POST /api/jobs/{id}/executar` só aceita job `pendente` ou `falha` (reexecução).
- Após sucesso, existem eventos e inconsistências ligadas ao `job_id`.
- Job já `concluido` sem flag de reexecução retorna `409`.

---

## RF-05 — Cancelar / excluir job

**Prioridade:** Must  
**Descrição:** Remove job que ainda não gerou inconsistências **ou** faz exclusão em cascata documentada. Decisão de modelagem: exclusão física do job só se status ∈ {`pendente`, `cancelado`}; caso contrário, soft status `cancelado` se já houver filhos — ver ADR-002.

**Aceite:**
- `DELETE /api/jobs/{id}` comporta-se conforme ADR-002.
- Front reflete a remoção/atualização sem reload completo da página (SPA).

---

## RF-06 — Listar inconsistências

**Prioridade:** Must  
**Descrição:** Lista inconsistências com filtro por status e, opcionalmente, por `job_id`.

**Aceite:**
- `GET /api/inconsistencias` e query params `status`, `job_id`.
- Front em cards/lista com badge de status.

---

## RF-07 — Criar inconsistência manual

**Prioridade:** Should  
**Descrição:** Permite abrir inconsistência sem job (origem `manual`), para cobrir o caso “achei na mão”.

**Aceite:**
- `POST /api/inconsistencias` cria com `origem=manual` e `job_id` nulo.
- Validação de tipo e severidade.

---

## RF-08 — Detalhar inconsistência

**Prioridade:** Must  
**Descrição:** Retorna inconsistência com comentários ordenados por data.

**Aceite:**
- `GET /api/inconsistencias/{id}` inclui array `comentarios`.
- `404` se não existir.

---

## RF-09 — Alterar status / parecer

**Prioridade:** Must  
**Descrição:** Atualiza status (`aberta`, `em_analise`, `resolvida`, `descartada`) e, quando resolvida/descartada, exige parecer textual.

**Aceite:**
- `PATCH /api/inconsistencias/{id}` valida transição.
- Fechar sem parecer → `400`.
- Front mostra parecer no card/detalhe.

---

## RF-10 — Comentar inconsistência

**Prioridade:** Must  
**Descrição:** Acrescenta comentário (autor livre no MVP + texto).

**Aceite:**
- `POST /api/inconsistencias/{id}/comentarios` → `201`.
- Comentário vazio → `400`.

---

## RF-11 — Excluir inconsistência

**Prioridade:** Must  
**Descrição:** Remove inconsistência e comentários associados (cascade).

**Aceite:**
- `DELETE /api/inconsistencias/{id}` → `204`.
- Sumiu da listagem.

---

## RF-12 — Painel no front

**Prioridade:** Must  
**Descrição:** SPA com navegação interna (sem trocar de HTML): visão Jobs, visão Inconsistências, detalhe.

**Aceite:**
- Um único `index.html`.
- Todas as rotas da API são acionadas em algum fluxo da UI.
- Listas/cards + formulários + feedback de erro/sucesso.

---

## RF-13 — Documentação Swagger

**Prioridade:** Must  
**Descrição:** Cada rota documentada (método, body, responses, códigos).

**Aceite:**
- UI Swagger acessível (ex.: `/api/docs`).
- Exemplos mínimos de request/response.

---

## RF-14 — Seed de demonstração

**Prioridade:** Should  
**Descrição:** Comando ou rota de desenvolvimento que popula 1 job concluído + 3 inconsistências + 2 comentários para o vídeo.

**Aceite:**
- README descreve como carregar o seed.
- Idempotente ou claramente “apaga e recria” em ambiente local.

---

## Rastreabilidade (RF → UC)

| RF | Caso de uso |
|----|-------------|
| RF-01..05 | UC-01, UC-02 |
| RF-06..11 | UC-03, UC-04, UC-05 |
| RF-12 | UC-06 |
| RF-13 | UC-07 |
| RF-14 | UC-08 |

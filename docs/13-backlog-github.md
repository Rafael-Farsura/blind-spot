# Blind Spot — backlog draft (source for GitHub Issues)

Tickets ordered for kanban. Each body has objective, DoD, and step-by-step.

---

## BS-01 · Freeze de escopo (Fase 0)

**Labels:** docs, fase-0  
**Objective:** Congelar RF/RNF antes de código.

### Passo a passo
1. Abrir `docs/01-requisitos-funcionais.md` e `docs/02-requisitos-nao-funcionais.md` no repo `blind-spot`.
2. Confirmar que NÃO entram: auth, Celery, upload, ERP/SPED/HANA, conciliação financeira (#5).
3. Marcar no `docs/10-roadmap.md` o item “Revisar escopo (freeze)” como `[x]`.
4. Se precisar mudar algo, abrir ADR novo em `docs/adrs/` (não editar RF sem ADR).
5. Commit + push no `blind-spot`.

### Definition of Done
- [ ] Freeze explícito no roadmap
- [ ] Escopo fora listado e alinhado

---

## BS-02 · Criar repositório blind-spot-api

**Labels:** setup, fase-1, api  
**Objective:** Repo público vazio com README e estrutura base.

### Passo a passo
1. `gh repo create blind-spot-api --public --clone`
2. Criar árvore mínima:
   - `app/__init__.py`, `app/config.py`, `app/extensions.py`
   - `tests/`, `requirements.txt`, `run.py`, `.gitignore`
3. README com: o que é, Python 3.x, `python -m venv`, `pip install -r requirements.txt`, `python run.py`, URL Swagger futura, link para docs em `blind-spot`.
4. Push na `main`.

### Definition of Done
- [ ] Repo público https://github.com/Rafael-Farsura/blind-spot-api
- [ ] README instalável

---

## BS-03 · Criar repositório blind-spot-web

**Labels:** setup, fase-1, web  
**Objective:** Repo público do SPA sem build.

### Passo a passo
1. `gh repo create blind-spot-web --public --clone`
2. Criar: `index.html`, `css/tokens.css`, `css/base.css`, `css/components.css`, `js/config.js`, `js/app.js`, `.gitignore`
3. README: abrir `index.html` no browser; configurar `BASE_URL` da API; sem Live Server obrigatório; link docs.
4. Push na `main`.

### Definition of Done
- [ ] Repo público
- [ ] `index.html` abre sem erro de path

---

## BS-04 · Bootstrap Flask: factory, CORS, health, Swagger vazio

**Labels:** api, fase-1  
**Depends:** BS-02  
**Objective:** API sobe e responde health + docs vazios.

### Passo a passo
1. Dependências: `flask`, `flask-sqlalchemy`, `flask-cors`, `flasgger` (ou apispec+swagger-ui).
2. Implementar `create_app()` (factory).
3. CORS liberando origem `null` (file://) e localhost.
4. Rota `GET /api/health` → `{ "status": "ok" }`.
5. Swagger em `/api/docs` (mesmo vazio).
6. `run.py` na porta 5000.
7. Testar no browser/curl.

### Definition of Done
- [ ] Health 200
- [ ] `/api/docs` abre
- [ ] CORS ok para file://

---

## BS-05 · Esqueleto SPA + design tokens

**Labels:** web, fase-1, design  
**Depends:** BS-03  
**Objective:** Shell visual Blind Spot (header + nav hash).

### Passo a passo
1. Aplicar tokens de `docs/07-design-system.md` em `css/tokens.css`.
2. Header com texto “Blind Spot” e nav `#/jobs` | `#/inconsistencias`.
3. Bootstrap via CDN + CSS próprio (obrigatório).
4. Fonte IBM Plex Sans (ou stack documentada).
5. Área `#app` vazia com empty state.
6. Abrir `index.html` e validar visual.

### Definition of Done
- [ ] Tokens aplicados
- [ ] Nav hash sem reload full
- [ ] Não parece tema Bootstrap puro

---

## BS-06 · Models SQLAlchemy + enums

**Labels:** api, fase-2, db  
**Depends:** BS-04  
**Objective:** Tabelas job, job_evento, inconsistencia, comentario.

### Passo a passo
1. Seguir dicionário em `docs/05-modelagem-dados.md`.
2. Criar models com FKs e cascade de comentário.
3. Enums/status conforme doc (job e inconsistência).
4. `db.create_all()` no startup (SQLite `blindspot.db`).
5. Índices básicos (status, job_id, criado_em).

### Definition of Done
- [ ] DB criado ao subir a app
- [ ] Relacionamentos batem com o ER

---

## BS-07 · Domínio: transições + VarreduraGenerator

**Labels:** api, fase-2, domain  
**Depends:** BS-06  
**Objective:** Regras puras sem Flask.

### Passo a passo
1. Funções de transição de status (job e inconsistência) com erros claros.
2. Fechar inconsistência exige parecer ≥ 10 chars (RF-09).
3. `VarreduraGenerator`: gera N inconsistências a partir de job (tipos/severidades do seed).
4. Limitar N (max 20).
5. Testes unitários TD-01..TD-04 (`docs/09-plano-testes.md`).

### Definition of Done
- [ ] Domínio testável sem HTTP
- [ ] pytest domínio ≥ 80% nesses módulos

---

## BS-08 · Repositories + Services (casos de uso)

**Labels:** api, fase-2  
**Depends:** BS-07  
**Objective:** Application layer (SOLID).

### Passo a passo
1. `JobRepository` / `InconsistenciaRepository`.
2. `JobService`: criar, listar, detalhar, executar (transação), delete conforme ADR-002/003.
3. `InconsistenciaService`: CRUD, comentar, patch status.
4. Services não importam `flask.request`.
5. Testes de service com SQLite memory.

### Definition of Done
- [ ] Executar job cria eventos + inconsistências na mesma transação
- [ ] Reexecução/falha conforme ADRs

---

## BS-09 · Rotas REST de jobs

**Labels:** api, fase-3  
**Depends:** BS-08  
**Objective:** Endpoints RF-01..05.

### Passo a passo
1. Blueprint `/api/jobs`.
2. POST, GET list (+filter status), GET id, POST `/{id}/executar`, DELETE.
3. Status codes: 201/200/204/400/404/409.
4. Payload JSON em PT nas mensagens de erro.
5. Testes TA-01, TA-02, TA-03, TA-07.

### Definition of Done
- [ ] Todas rotas de job cobertas por teste
- [ ] Executar gera inconsistências

---

## BS-10 · Rotas REST inconsistências + comentários

**Labels:** api, fase-3  
**Depends:** BS-08  
**Objective:** RF-06..11.

### Passo a passo
1. Blueprint `/api/inconsistencias`.
2. GET list (status, job_id), POST manual, GET id, PATCH, DELETE.
3. POST `/{id}/comentarios`.
4. Validar parecer no fechamento.
5. Testes TA-04, TA-05, TA-06.

### Definition of Done
- [ ] Comentários ordenados por data no detalhe
- [ ] DELETE cascade comentários

---

## BS-11 · Seed de demonstração + Swagger completo

**Labels:** api, fase-3, docs  
**Depends:** BS-09, BS-10  
**Objective:** Demo em 1 minuto + OpenAPI preenchido.

### Passo a passo
1. `POST /api/dev/seed` (ou CLI): 1 job concluído + 3 inconsistências + comentários (dados fictícios do doc 05).
2. Documentar cada rota no Swagger (método, body, responses, códigos).
3. Teste TA-08.
4. Atualizar README da API com seed.

### Definition of Done
- [ ] Seed idempotente ou “recria” documentado
- [ ] Swagger com ≥ 4 rotas bem descritas (todas, de preferência)

---

## BS-12 · Front: api client + toast + config

**Labels:** web, fase-4  
**Depends:** BS-05, BS-04  
**Objective:** fetch wrapper para todas as rotas.

### Passo a passo
1. `js/config.js` com `BASE_URL` (default `http://127.0.0.1:5000`).
2. `js/api/client.js`: JSON, erros → objeto `{ erro }`.
3. Módulos `jobs.js` e `inconsistencias.js` chamando TODOS os endpoints.
4. Toast sucesso/erro (`aria-live`).
5. Mensagem clara se API estiver off.

### Definition of Done
- [ ] Uma função por rota da API
- [ ] Erro de rede visível na UI

---

## BS-13 · Front: views de jobs

**Labels:** web, fase-4  
**Depends:** BS-12, BS-09  
**Objective:** Listar/criar/executar/excluir jobs em cards.

### Passo a passo
1. Form criar job (tipo, competência, observação).
2. Cards com StatusBadge.
3. Botão Executar (chama POST executar) + refresh lista.
4. Detalhe `#/jobs/:id` com eventos.
5. Delete conforme regra da API (tratar 409).

### Definition of Done
- [ ] Fluxo criar → executar visível sem F5
- [ ] Badge de status colorido + texto

---

## BS-14 · Front: views de inconsistências + comentários

**Labels:** web, fase-4  
**Depends:** BS-12, BS-10  
**Objective:** Triagem completa.

### Passo a passo
1. Lista/cards com filtro status e job_id.
2. Detalhe com timeline de comentários.
3. Form comentar (autor + texto).
4. PATCH status + parecer ao resolver/descartar.
5. DELETE inconsistência.

### Definition of Done
- [ ] Checklist smoke itens de inconsistência ok
- [ ] Todas rotas de inconsistência acionadas na UI

---

## BS-15 · Front: hash router + polish visual

**Labels:** web, fase-4  
**Depends:** BS-13, BS-14  
**Objective:** SPA coerente + design system.

### Passo a passo
1. Router `#/jobs`, `#/jobs/:id`, `#/inconsistencias`, `#/inconsistencias/:id`.
2. Empty states com CTA.
3. Revisar CSS próprio (não só Bootstrap).
4. Rodar checklist smoke completo (`docs/09-plano-testes.md` §4).

### Definition of Done
- [ ] Checklist 100%
- [ ] Sem dependência de Live Server

---

## BS-16 · Qualidade: testes cobertura + limpeza

**Labels:** api, qualidade, fase-5  
**Depends:** BS-11  
**Objective:** Cobertura alvo + código limpo.

### Passo a passo
1. `pytest --cov=app --cov-report=term-missing`
2. Atingir ≥80% domain/services e ≥70% api.
3. Remover prints/debug; rodar ruff/flake8 se configurado.
4. Revisar nomes (ADR-001).
5. Atualizar README com comando de teste.

### Definition of Done
- [ ] Cobertura no alvo
- [ ] CI local documentada (mesmo sem GitHub Actions)

---

## BS-17 · Vídeo de entrega ≤ 4 min

**Labels:** entrega, fase-5  
**Depends:** BS-15, BS-16  
**Objective:** Vídeo conforme rubrica PUC.

### Passo a passo
1. Seguir `docs/12-roteiro-video.md`.
2. Subir API + seed.
3. Gravar: objetivo → Swagger (4 rotas chave) → SPA.
4. Cortar para ≤ 4:00.
5. Publicar (YouTube unlisted ok) e guardar URL.

### Definition of Done
- [ ] Vídeo ≤ 4 min
- [ ] Três blocos do script presentes

---

## BS-18 · Mensagem de entrega (links públicos)

**Labels:** entrega, fase-5  
**Depends:** BS-17  
**Objective:** Submissão da disciplina.

### Passo a passo
1. Confirmar repos `blind-spot-api` e `blind-spot-web` públicos.
2. Montar mensagem:
   - Link vídeo
   - Link back-end (https://…)
   - Link front-end (https://…)
3. Opcional: link docs `blind-spot`.
4. Revisar READMEs finais.

### Definition of Done
- [ ] Três links https válidos
- [ ] READMEs com install/run

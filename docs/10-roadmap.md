# Roadmap de entrega — Blind Spot

Ordem fechada. **Não pular para código de feature antes do passo 0 estar no GitHub.**

---

## Fase 0 — Documentação (este repo) ← atual

- [x] Kick-off
- [x] RF / RNF
- [x] Arquitetura e system design
- [x] Modelagem e diagramas
- [x] Casos de uso
- [x] Design system
- [x] Padrões / SOLID
- [x] Plano de testes
- [x] Publicar repo `blind-spot` no GitHub
- [x] Revisar escopo (freeze) — 01/08/2026

## Fase 1 — Bootstrap dos repos de código

1. [x] Criar `blind-spot-api` (público)
2. [x] Criar `blind-spot-web` (público)
3. [x] README de cada um (install/run)
4. [x] Factory Flask + CORS + health + Swagger vazio
5. [x] `index.html` esqueleto + tokens CSS

## Fase 2 — Domínio e persistência

1. [x] Models SQLAlchemy
2. [x] Enums e regras de transição
3. [x] Repositórios + services
4. [x] Testes unitários domínio

## Fase 3 — API completa

1. [x] Rotas jobs
2. [x] Rotas inconsistências/comentários
3. [x] Executar + seed
4. [x] Testes de API
5. [x] Swagger preenchido (docstrings flasgger nas rotas)

## Fase 4 — Front

1. [x] client fetch + toast
2. [x] Views jobs
3. [x] Views inconsistências
4. [x] Hash router
5. [x] Checklist smoke (manual: abrir index.html com API + seed)

## Fase 5 — Polimento e entrega acadêmica ← em andamento

1. [x] Cobertura pytest + limpeza
2. Seed + roteiro de vídeo (≤ 4 min)
3. Gravar: objetivo → Swagger → SPA
4. Mensagem de entrega com links https dos 2 repos + vídeo

## Roteiro do vídeo (esqueleto)

| Tempo | Bloco |
|------:|-------|
| 0:00–0:40 | Problema e objetivo do Blind Spot |
| 0:40–2:10 | Swagger: criar job, executar, listar, patch, delete |
| 2:10–3:40 | Front: mesmo fluxo em cards |
| 3:40–4:00 | Encerramento |

## Freeze de escopo

Depois da Fase 0 publicada, mudanças de RF só via ADR novo. Sem “já que estamos aqui” (conciliação, auth, upload).

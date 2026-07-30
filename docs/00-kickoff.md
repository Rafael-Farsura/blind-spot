# Kick-off — Cruzamento

**Data:** 30/07/2026  
**Produto:** Cruzamento  
**Tipo:** MVP acadêmico (PUC-Rio — Full Stack básico) + portfólio técnico  
**Duração prevista de implementação:** após fechamento desta documentação

---

## 1. Contexto

O MVP da disciplina pede um sistema web inovador, com separação clara cliente/servidor, API REST em Flask e SPA sem frameworks (React/Vue/Angular). O front deve rodar abrindo o `index.html` direto no browser.

Além da nota, o produto precisa fazer sentido para um perfil de engenharia em domínio fiscal/operacional: jobs de processamento, diagnóstico de divergência e fechamento com parecer.

## 2. Problema de negócio (hipótese)

> Analistas perdem tempo rastreando “quem rodou o quê” e “qual divergência ainda está aberta”, porque job e achado ficam em lugares diferentes.

**Hipótese a validar com o MVP:** um painel único job → inconsistência → comentário → fechamento reduz atrito e deixa o fluxo auditável.

## 3. Objetivo do MVP

Entregar um protótipo funcional que:

1. permita criar e listar jobs de varredura;
2. execute uma varredura **simulada** (síncrona) gerando inconsistências;
3. permita triar inconsistências (listar, detalhar, comentar, alterar status, excluir);
4. exponha a API documentada em Swagger;
5. consuma todas as rotas a partir do front SPA.

## 4. Fora de escopo (explícito)

- Autenticação / autorização / multi-tenant
- Worker assíncrono real (Celery, threads persistentes, filas)
- Integração com ERP, SPED, HANA ou qualquer sistema corporativo
- Upload de arquivos fiscais
- Dados reais de cliente
- Deploy em nuvem (opcional depois; não é requisito da entrega)

## 5. Papéis

| Papel | Quem | Responsabilidade |
|-------|------|------------------|
| Product owner / dev | Rafael | Escopo, implementação, vídeo, repositórios |
| Avaliador | Disciplina PUC | Nota conforme rubrica MVP-2 |
| Usuário-alvo (persona) | Analista fiscal/ops | Triagem diária de divergências |

## 6. Persona (resumo)

**Ana, 32 anos** — analista fiscal. Recebe alerta de que “a varredura da competência rodou”. Quer ver o job, abrir as divergências novas, deixar um comentário curto e marcar como resolvida ou em análise.

## 7. Métricas de sucesso do MVP

| Métrica | Alvo |
|---------|------|
| Rotas documentadas no Swagger | ≥ 4, com pelo menos 1 POST |
| Tabelas no SQLite | ≥ 2 relacionadas |
| Front consome | 100% das rotas expostas |
| Vídeo de demonstração | ≤ 4 minutos, script completo |
| Repositórios | 2 públicos (api + web) + este de docs |
| Reuso do código-exemplo da aula | < 50% |

## 8. Riscos e mitigações

| Risco | Impacto | Mitigação |
|-------|---------|-----------|
| Escopo inchado (job + inconsistência + match financeiro) | Entrega incompleta | Híbrido só #1+#2; conciliação fora |
| CORS / `file://` | Front não chama API | Flask-CORS desde o dia 1; README com URL da API |
| Demo longa demais | Penalidade no vídeo | Roteiro fixo: 3 inconsistências seed + 1 job |
| Cheiro de código gerado | Credibilidade | Naming humano, commits pequenos, sem comentários genéricos |

## 9. Princípios de trabalho

1. Diff mínimo e nomes do domínio (job, inconsistência, parecer).
2. Contrato da API antes da UI.
3. Testes no núcleo de domínio e nas rotas críticas.
4. Documentação viva neste repo; código nos repos de entrega.
5. Nada de dado sensível ou referência a cliente real.

## 10. Próximo passo após este kick-off

Congelar RF/RNF → modelagem → arquitetura → só então abrir `cruzamento-api` e `cruzamento-web`.

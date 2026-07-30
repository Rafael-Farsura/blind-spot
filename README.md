# Cruzamento

Sistema de triagem de inconsistências fiscais alimentadas por jobs de varredura.

Este repositório concentra o **planejamento e a documentação de produto**. A implementação será entregue em dois repositórios separados (exigência da disciplina):

| Repositório | Conteúdo |
|-------------|----------|
| `cruzamento-api` | Back-end Flask + SQLAlchemy + SQLite + Swagger |
| `cruzamento-web` | Front-end SPA (HTML/CSS/JS + Bootstrap + CSS próprio) |

## Problema

Equipes fiscais e operacionais costumam descobrir divergências em planilhas, e-mails e tickets soltos. Falta um lugar único para:

1. registrar e acompanhar execuções de varredura (jobs);
2. triar as inconsistências geradas;
3. registrar parecer e fechar o ciclo.

## Solução (MVP)

O analista dispara ou consulta um **job de varredura**. A execução gera **inconsistências**. Cada inconsistência pode receber **comentários**, mudar de **status** e ser encerrada com parecer.

Escopo deliberadamente pequeno: o suficiente para validar o fluxo ponta a ponta, sem fila real em background nem autenticação corporativa.

## Documentação

| Documento | Descrição |
|-----------|-----------|
| [Kick-off](docs/00-kickoff.md) | Contexto, objetivos, papéis, restrições |
| [Requisitos funcionais](docs/01-requisitos-funcionais.md) | RF com critérios de aceite |
| [Requisitos não funcionais](docs/02-requisitos-nao-funcionais.md) | RNF, restrições acadêmicas e técnicas |
| [Arquitetura](docs/03-arquitetura.md) | Visão C4, camadas, REST |
| [System design](docs/04-system-design.md) | Decisões de projeto, trade-offs |
| [Modelagem de dados](docs/05-modelagem-dados.md) | Entidades, ER, dicionário |
| [Casos de uso](docs/06-casos-de-uso.md) | Atores, UC, fluxos |
| [Diagramas](docs/diagramas/) | Fluxo, sequência, uso |
| [Design system](docs/07-design-system.md) | Tokens, componentes, UI |
| [Padrões e SOLID](docs/08-padroes-e-solid.md) | Clean Code, PEPs, patterns |
| [Plano de testes](docs/09-plano-testes.md) | Estratégia e cobertura-alvo |
| [Roadmap de entrega](docs/10-roadmap.md) | Passo a passo até o vídeo |
| [ADRs](docs/adrs/) | Decisões arquiteturais |
| [Glossário](docs/11-glossario.md) | Termos do domínio |
| [Roteiro do vídeo](docs/12-roteiro-video.md) | Script ≤ 4 min |

## Stack (fixada pela disciplina)

- **API:** Python 3, Flask, Flask-SQLAlchemy, SQLite, flasgger/apispec (Swagger)
- **Web:** HTML, CSS, JavaScript (vanilla), Bootstrap + folhas CSS próprias
- **Execução do front:** abrir `index.html` no navegador (sem bundler, sem servidor de front)

## Como contribuir neste repo

Documentação em Markdown. Diagramas em Mermaid dentro dos arquivos (renderizam no GitHub).

## Licença

Uso acadêmico / portfólio. Sem dados reais de clientes.

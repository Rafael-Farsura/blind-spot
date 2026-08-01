# Plano de testes — Blind Spot

## 1. Pirâmide

```
        /\
       /UI\        checklist manual + smoke (vídeo)
      /----\
     / API  \      pytest + Flask test client
    /--------\
   / Domínio  \    pytest unitário (transições, generator)
  /------------\
```

Front sem framework de teste obrigatório na rubrica; cobrimos com checklist e, se der tempo, 2–3 testes de `client` em Node **não** — manter simples: checklist versionado.

## 2. Cobertura-alvo

| Camada | Alvo | Ferramenta |
|--------|------|------------|
| `domain/` + `services/` | ≥ 80% linhas | pytest-cov |
| `api/` rotas | ≥ 70% | test client |
| Front | 100% dos fluxos do checklist | manual |

Comando previsto na API:

```bash
pytest --cov=app --cov-report=term-missing
```

## 3. Casos de teste (back)

### Domínio

| ID | Caso | Esperado |
|----|------|----------|
| TD-01 | Fechar sem parecer | erro de domínio |
| TD-02 | Transição `concluido` → executar | conflito |
| TD-03 | Generator com seed fixo | N itens, tipos válidos |
| TD-04 | Origem job sem job_id | inválido |

### API

| ID | Caso | Esperado |
|----|------|----------|
| TA-01 | POST job válido | 201 |
| TA-02 | POST job sem competência | 400 |
| TA-03 | POST executar | 200 + inconsistências |
| TA-04 | GET inconsistencia | comentarios [] |
| TA-05 | PATCH resolver com parecer | 200 |
| TA-06 | DELETE inconsistencia | 204 |
| TA-07 | GET job inexistente | 404 |
| TA-08 | Seed | base populada |

## 4. Checklist front (smoke)

- [ ] Abre `index.html` e carrega CSS
- [ ] Config aponta para API local
- [ ] Cria job e aparece no card
- [ ] Executa varredura e vê inconsistências
- [ ] Filtra por status
- [ ] Abre detalhe, comenta, resolve
- [ ] Exclui um registro
- [ ] Mensagem amigável com API desligada

## 5. Dados de teste

Fixtures em `tests/conftest.py`: app Flask em memória (`sqlite:///:memory:`), client, factory de job.

## 6. Critério de “pronto para vídeo”

- Todos TA-01..08 verdes
- Cobertura domínio/services no alvo
- Checklist front sem blockers
- Seed reproduzível em < 1 minuto

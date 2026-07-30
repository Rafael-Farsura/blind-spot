# Design system — Cruzamento (web)

Objetivo: Bootstrap como base estrutural + **CSS próprio obrigatório** (tokens e componentes), para não parecer o tema default.

---

## 1. Princípios

1. Uma composição clara por tela (jobs **ou** detalhe), sem dashboard lotado.
2. Status legível por cor **e** texto (não só cor).
3. Densidade operacional: cards compactos, tipografia sóbria.
4. Sem roxo “genérico de template”, sem glassmorphism.

## 2. Tokens (`css/tokens.css`)

```css
:root {
  --color-bg: #f4f6f8;
  --color-surface: #ffffff;
  --color-ink: #1c2430;
  --color-muted: #5c6b7a;
  --color-border: #d7dee7;
  --color-accent: #0f6e56;      /* verde-petrolio */
  --color-accent-hover: #0b5744;
  --color-danger: #b42318;
  --color-warn: #b54708;
  --color-info: #175cd3;

  --status-aberta: #b54708;
  --status-analise: #175cd3;
  --status-resolvida: #027a48;
  --status-descartada: #667085;
  --status-job-pendente: #667085;
  --status-job-run: #175cd3;
  --status-job-ok: #027a48;
  --status-job-fail: #b42318;

  --radius: 8px;
  --shadow: 0 1px 2px rgba(16, 24, 40, 0.06);
  --font-sans: "IBM Plex Sans", "Segoe UI", sans-serif;
  --space-1: 4px;
  --space-2: 8px;
  --space-3: 16px;
  --space-4: 24px;
}
```

Fonte: IBM Plex Sans via Google Fonts **ou** stack local — evita Inter/Roboto default de template.

## 3. Componentes

| Componente | Uso | Notas |
|------------|-----|-------|
| `AppHeader` | Nome do produto + nav hash | Logo texto “Cruzamento” |
| `StatusBadge` | Status job/inconsistência | Classe por status |
| `JobCard` | Lista de jobs | Tipo, competência, status, ações |
| `InconsistenciaCard` | Lista | Severidade + título + job ref |
| `DetailPanel` | Detalhe | Comentários em timeline |
| `FormPanel` | Criar job / comentar / parecer | Labels + erro inline |
| `Toast` | Feedback | Sucesso / erro |
| `EmptyState` | Lista vazia | CTA para criar/executar |

Bootstrap: grid, form controls, modal se necessário. Visual final vem dos tokens.

## 4. Layout

```
┌──────────────────────────────────────────┐
│ Header: Cruzamento          Jobs | Achados│
├──────────────────────────────────────────┤
│ Título da visão                          │
│ [filtros]                    [ação prim.]│
│ ┌────┐ ┌────┐ ┌────┐                     │
│ │card│ │card│ │card│                     │
│ └────┘ └────┘ └────┘                     │
└──────────────────────────────────────────┘
```

Breakpoints: mobile empilha cards; desktop 2–3 colunas.

## 5. Microcopy (tom)

- Botões: “Executar varredura”, “Registrar parecer”, “Salvar comentário”.
- Erros: “Competência é obrigatória.” (direto, sem “Oops!”).
- Vazio: “Nenhum job ainda. Crie o primeiro para rodar uma varredura.”

## 6. Acessibilidade mínima

- Contraste AA nos textos principais.
- Foco visível em botões/links.
- `aria-live` no toast.

## 7. O que evitar

- Cards com sombra tripla e border-radius 24px.
- Gradiente roxo no header.
- Ícones emoji como status.
- Dashboard com KPI inventado no hero.

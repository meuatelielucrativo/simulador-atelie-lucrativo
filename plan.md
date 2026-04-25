# Arquitetura — Simulador de Lucratividade para Artesãs (v2)

## 1. Estrutura do Projeto

```
Simulador de Lucratividade/
├── index.html        ← SPA completa (HTML + CSS inline + JS inline)
├── foto-ivana.jpg    ← Foto da Dona Ivana (proporção 9:16, fornecida externamente)
├── plan.md
└── claude.md
```

Chart.js carregado via CDN. Sem dependências locais além da imagem.

---

## 2. Lógica de Cálculo

```
Inputs (4 variáveis):
  materialCost  → Custo Total de Materiais (R$)
  hours         → Tempo de Produção (h)
  hourlyRate    → Valor da Hora de Trabalho (R$/h)
  margin        → Margem de Lucro Desejada (%) [slider]

Cálculos:
  laborCost    = hours × hourlyRate
  baseCost     = materialCost + laborCost
  profitAmount = baseCost × (margin / 100)
  salePrice    = baseCost + profitAmount

Segmentos do Doughnut Chart (valores absolutos em R$):
  Materiais     = materialCost
  Mão de Obra   = laborCost
  Lucro Líquido = profitAmount
```

Guard clause: se `salePrice === 0`, exibir estado neutro (sem divisão por zero no chart).

---

## 3. Estrutura do DOM

```
<body>
  ┌─ <section#hero> ──────────────────────────────────────────┐
  │  Split layout: coluna da foto (esq) + coluna de copy (dir) │
  │  .hero-photo → <img src="./foto-ivana.jpg">               │
  │  .hero-copy  → headline + subtítulo + âncora ↓            │
  └───────────────────────────────────────────────────────────┘

  ┌─ <main> ──────────────────────────────────────────────────┐
  │  <section#inputs>                                          │
  │    input[type=number] #materialCost                        │
  │    input[type=number] #hours                               │
  │    input[type=number] #hourlyRate                          │
  │    input[type=range]  #margin + <output>                   │
  │                                                            │
  │  <section#dashboard>                                       │
  │    .card → Preço de Venda (Pix)                            │
  │    .card → Lucro Líquido (R$)                              │
  │    .chart-card → <canvas#chart> Doughnut (3 segmentos)     │
  │                                                            │
  │  <section#funcionalidades-premium>                         │
  │    .feature-grid (4 cards)                                 │
  │      .feature-card → ícone + título + descrição            │
  │      [Orçamentos Instantâneos]                             │
  │      [Cálculos Automáticos]                                │
  │      [Divisão Detalhada]                                   │
  │      [Configuração Única]                                  │
  └───────────────────────────────────────────────────────────┘

  ┌─ <footer> ────────────────────────────────────────────────┐
  │  micro-copy + <a.cta-primary> "Quero automatizar meus      │
  │  cálculos" → https://go.hotmart.com/N104438479J            │
  └───────────────────────────────────────────────────────────┘
```

---

## 4. Seção Hero — Especificações de Layout

### Desktop (≥ 720 px)
- CSS Grid: `grid-template-columns: 380px 1fr` (foto fixa, copy cresce)
- `.hero-photo`: altura 100% da seção, `object-fit: cover`, `object-position: center top` para preservar o rosto
- `.hero-copy`: centralizado verticalmente com `align-self: center`; padding generoso

### Mobile (< 720 px)
- Layout em coluna única; foto aparece no topo
- Altura da foto: `min(65vw, 420px)` — não ocupa a tela inteira
- Gradiente de saída na borda inferior da foto:
  ```css
  .hero-photo::after {
    content: '';
    position: absolute;
    bottom: 0; left: 0; right: 0;
    height: 120px;
    background: linear-gradient(to bottom, transparent, var(--bg-primary));
  }
  ```
- `.hero-photo` precisa de `position: relative` para o pseudo-elemento funcionar

### Imagem
- Arquivo: `./foto-ivana.jpg`
- Proporção esperada: 9:16 (vertical)
- CSS: `object-fit: cover; object-position: center top`
- Fallback de cor se a imagem não carregar: `background-color: var(--bg-card)`

---

## 5. Seção Funcionalidades Premium — Especificações

### Estrutura de cada `.feature-card`
```
[ ícone SVG inline ]
[ título ]
[ descrição de uma linha ]
```

### Os 4 cards

| # | Título | Descrição |
|---|---|---|
| 1 | Orçamentos Instantâneos | Gere orçamentos prontos para enviar via WhatsApp em segundos. |
| 2 | Cálculos Automáticos | Seus custos de materiais e tempo são puxados do catálogo automaticamente. |
| 3 | Divisão Detalhada | Veja exatamente para onde vai cada centavo: lucro, materiais e mão de obra. |
| 4 | Configuração Única | Configure seu ateliê uma vez. A ferramenta trabalha por você para sempre. |

### Layout
- Mobile: grid 1 coluna
- Tablet (≥ 480 px): grid 2 colunas
- Desktop (≥ 720 px): grid 4 colunas (linha única)

---

## 6. Mapeamento de Eventos DOM

| Evento | Elemento(s) | Handler |
|---|---|---|
| `input` | `#materialCost`, `#hours`, `#hourlyRate` | `recalculate()` |
| `input` | `#margin` | `recalculate()` + atualiza label do slider |
| `DOMContentLoaded` | `document` | Inicializa Chart.js + chama `recalculate()` com valores padrão |

Fluxo do handler `recalculate()`:
1. Lê e valida todos os inputs (fallback `0` se vazio/NaN)
2. Executa a lógica matemática
3. Atualiza os cards de output (`textContent`)
4. Atualiza `chart.data.datasets[0].data` e chama `chart.update()`

---

## 7. Cronograma de Execução

| Etapa | Entregável | Status |
|---|---|---|
| 1 | `plan.md` + `claude.md` (v1) | Concluído |
| 1b | `plan.md` + `claude.md` (v2 — atualização de arquitetura) | Concluído |
| 2 | `index.html` inicial (estrutura + CSS + JS + Chart.js) | Concluído |
| 3 | Reescrita do `index.html` com hero split, vitrine premium e novo CTA | Pendente |
| 4 | Revisão mobile (hero gradient, grid features), testes manuais | Pendente |

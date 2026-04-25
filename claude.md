# Diretrizes do Projeto — Simulador de Lucratividade para Artesãs (v2)

## Paleta de Cores — Soft Dark

Estética: "paz financeira". Tons escuros quentes com dourados suaves e acentos pastéis.
Evitar contraste agressivo. O site deve parecer organizado e acolhedor, não técnico.

| Token CSS | Hex | Uso |
|---|---|---|
| `--bg-primary` | `#0D0C0A` | Fundo da página (preto quente, não frio) |
| `--bg-card` | `#1C1A17` | Fundo dos cards e seções |
| `--bg-input` | `#26231E` | Fundo dos campos de input |
| `--accent` | `#D4AA5F` | Dourado suave — destaques, borders ativas, slider thumb |
| `--accent-dim` | `#8B6A2A` | Dourado escuro — hover, ícones secundários |
| `--text-primary` | `#EDE8E0` | Texto principal (creme quente) |
| `--text-secondary` | `#8C8680` | Labels, placeholders, descrições |
| `--positive` | `#6DBF8A` | Lucro líquido (verde pastel, não saturado) |
| `--border` | `#302C26` | Divisores e bordas de cards |
| `--hero-gradient` | linear-gradient(to bottom, transparent, `#0D0C0A`) | Gradiente de saída da foto no mobile |

### Segmentos do Doughnut Chart (Chart.js)

| Segmento | Cor |
|---|---|
| Materiais | `#D4AA5F` |
| Mão de Obra | `#8B6A2A` |
| Lucro Líquido | `#6DBF8A` |

---

## Micro-Copy e Tom

O texto da interface deve soar como uma amiga de confiança que entende de dinheiro.
Foco em dois eixos: **tempo economizado** e **fim da dor de cabeça com cálculos**.

### Exemplos de micro-copy aprovados

| Elemento | Texto |
|---|---|
| Headline do Hero | "Descubra em segundos quanto cobrar pelo seu trabalho." |
| Sub-headline do Hero | "Sem planilha complicada. Sem cálculo na cabeça. Só você e o número certo." |
| Label âncora do Hero | "Calcule agora — é grátis" |
| Section label Inputs | "Dados do seu produto" |
| Placeholder (inputs) | Deixar vazio — não usar "0,00" como placeholder |
| Label do slider | "Quanto de lucro você quer?" |
| Card Preço de Venda | "Preço justo para cobrar no Pix" |
| Card Lucro Líquido | "Entra no seu bolso" |
| Section label Premium | "O que você desbloqueia na versão completa" |
| CTA Principal | "Quero automatizar meus cálculos" |
| Micro-copy abaixo do CTA | "Acesso imediato. Funciona no celular e no computador." |

### CTA Principal

```html
<a class="cta-primary" href="https://go.hotmart.com/N104438479J" target="_blank" rel="noopener">
  Quero automatizar meus cálculos
</a>
```

- Cor de fundo: `var(--accent)` (`#D4AA5F`)
- Cor do texto: `#0D0C0A` (escuro sobre dourado)
- Font-weight: 700
- Sem border-radius excessivo — máximo 8px

---

## Regras de Estilo de Código

1. **Arquivo único**: todo CSS em `<style>` e todo JS em `<script>` dentro do `index.html`.
2. **Sem frameworks**: zero imports de React, Vue, Angular ou similares.
3. **Chart.js exclusivamente via CDN**: `<script src="https://cdn.jsdelivr.net/npm/chart.js">`.
4. **JS**: ES6+ (const/let, arrow functions, template literals). Sem classes desnecessárias.
5. **Sem comentários óbvios**: comentar apenas lógica não evidente.
6. **CSS**: variáveis CSS (`--token`) para todas as cores. Mobile-first: breakpoints via `min-width`.
7. **Nomeação**: camelCase para variáveis JS; kebab-case para IDs e classes HTML/CSS.
8. **Formatação monetária**: `toLocaleString('pt-BR', { style: 'currency', currency: 'BRL' })` nativo.
9. **Ícones**: SVG inline (não usar emoji, não usar bibliotecas de ícones externas).

---

## Restrições Arquiteturais

- **Nenhum backend, API ou chamada de rede** além do CDN do Chart.js.
- **Nenhum localStorage ou cookie**: a ferramenta é stateless entre reloads.
- **Nenhuma dependência npm**: zero `package.json`, zero `node_modules`.
- **Sem build step**: o `index.html` abre diretamente no navegador (`file://`) e funciona em qualquer servidor estático.
- **Inputs sem formulário de submit**: cálculo 100% reativo ao evento `input`.
- **Taxa zero**: cenário base = pagamento via Pix. Nenhum campo, variável ou lógica de taxas no código.
- **Imagem local**: `./foto-ivana.jpg` referenciada por caminho relativo. Sem upload para CDN externo.

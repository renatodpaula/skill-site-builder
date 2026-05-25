# Extraction Protocol — Design System via Firecrawl

## Workflow Completo

### Step 1: Scrape Inicial

```bash
# Scrape padrão — markdown + screenshot
firecrawl scrape "<url>" --format markdown,screenshot -o .firecrawl/ref-page.md

# Se a página for SPA / JavaScript-heavy
firecrawl scrape "<url>" --wait-for 3000 --only-main-content -o .firecrawl/ref-page.md

# Para múltiplas páginas (ex: home + pricing + docs)
firecrawl scrape "<url>/pricing" --format markdown -o .firecrawl/ref-pricing.md
firecrawl scrape "<url>/docs"    --format markdown -o .firecrawl/ref-docs.md
```

Adicione ao `.gitignore` se em um projeto:
```
.firecrawl/
```

### Step 2: Leitura Incremental do Output

Não leia o arquivo scrapeado inteiro de uma vez. Abordagem incremental:

1. Leia as primeiras 100 linhas para entender a estrutura geral
2. Grepe por valores de cor: `#[0-9a-fA-F]{3,8}`, `rgb(`, `hsl(`
3. Grepe por font names: `font-family`, `@import`, `fonts.googleapis`
4. Leia seções específicas conforme necessário

### Step 3: Identificar Sinais Indiretos

Markdown não preserva CSS computado. Use estas estratégias quando os valores diretos não estiverem disponíveis:

**Cores via class names Tailwind**:
- `.bg-slate-900` → background: `#0f172a` (dark slate)
- `.bg-white` → background: `#ffffff`
- `.text-gray-900` → texto escuro
- `.accent-yellow-400` → accent amarelo

**Cores via tokens de design system**:
- Procure por variáveis CSS: `--color-`, `--bg-`, `--primary-`
- Procure por `var(--` no conteúdo HTML

**Fontes via links**:
- `fonts.googleapis.com/css2?family=` → extrai os nomes de family
- `typekit.net`, `use.typekit.net` → Adobe Fonts

**Espaçamento via classes**:
- Tailwind: `p-8` = 32px, `py-20` = 80px, `gap-6` = 24px
- Classes `.container`, `.max-w-*` indicam max-widths

**Estética via conteúdo**:
- O tipo de produto/serviço informa a direção (fintech → austero, gaming → vibrante)
- Procure por palavras-chave no conteúdo que sugerem o tom

---

## Schema de Extração de Tokens

Após ler o conteúdo scrapeado, preencha este schema mentalmente antes de escrever qualquer código:

```
DESIGN SYSTEM EXTRACTION
========================

URL: [url scrapeada]
Data: [data de hoje]

TEMA
- Base: dark | light
- Contraste: alto | médio | sutil

PALETA DE CORES
- bg (página):        [valor ou "inferido: dark near-black"]
- surface (cards):    [valor]
- surface-raised:     [valor ou "não detectado"]
- border:             [valor]
- text-primary:       [valor]
- text-secondary:     [valor]
- accent/brand:       [valor + nome da cor, ex: "lime yellow #e8ff47"]
- accent secundário:  [valor se houver]
- sucesso/erro:       [valor se houver]

TIPOGRAFIA
- Font display (headings): [nome + weights usados]
- Font body (prosa/UI):    [nome + weights usados]
- Font mono (code/labels): [nome se detectado, ou "não detectado"]
- Tamanho body:            [aprox em px]
- Heading H1:              [aprox em px]
- Letter-spacing H1:       [tight/normal/wide ou valor]
- Line-height body:        [1.4 / 1.6 / 1.8]

ESPAÇAMENTO
- Section padding-y:  [aprox em px]
- Section padding-x:  [aprox em px]
- Card padding:        [aprox em px]
- Max-width conteúdo:  [aprox em px]
- Gap entre cards:     [aprox em px]
- Border-radius cards: [aprox em px]

MOTION
- Velocidade:   fast (100–150ms) | normal (200–300ms) | slow (400ms+)
- Estilo:       snappy | smooth | theatrical | ausente
- Signature:    [animação específica notável, se houver]

COMPONENTES DISTINTOS (3–5)
1. [nome] — [descrição breve]
2. [nome] — [descrição breve]
3. [nome] — [descrição breve]

RESUMO ESTÉTICO
[2–3 palavras que capturam a essência visual, ex: "minimal enterprise dark", "playful consumer pastel", "technical documentation mono"]
```

---

## Confirmação com o Usuário

Após preencher o schema, apresente um resumo compacto antes de gerar código:

```
Design system extraído de linear.app:
- Tema: dark
- Cores: bg #0a0a0b, surface #111113, accent #5e6ad2 (indigo)
- Fontes: display Inter (bold), body Inter, mono JetBrains Mono
- Espaçamento: seções 96px, cards 20px, radius 8px
- Motion: fast, snappy (150ms ease-in-out)
- Padrões: gradient text headings, outlined icon cards, split feature sections
- Estética: minimal productivity dark

Construindo: landing page inspirado nesse sistema.
Posso começar?
```

Se o usuário confirmar, inicie a construção. Se houver dúvidas ou ajustes, incorpore antes de codificar.

---

## Fallbacks por Qualidade do Scrape

### Scrape Excelente (HTML/CSS completo preservado)
→ Extraia valores diretos, construa com fidelidade ao sistema

### Scrape Bom (conteúdo + alguns estilos inline)
→ Use valores diretos + inferências de class names + julgamento visual

### Scrape Esparso (apenas texto, sem estilos)
→ Infira da **categoria do produto**: fintech (austero, sans, navy/white), gaming (vibrante, bold, dark/neon), B2B SaaS (clean, medium contrast), consumer app (colorido, arredondado)
→ Use o nome/logo do site como pista estética
→ Construa uma interpretação e deixe claro ao usuário: "O scrape foi esparso — construí baseado na categoria/tom do site. Ajuste se quiser."

### Scrape Falhou
→ Informe o usuário: "Não consegui scraper [url]. Pode fornecer um screenshot ou descrever o estilo que quer como referência?"

---

## Sites com Múltiplas Páginas

Para marketing sites complexos, scrape 2–3 páginas e triangule:

```bash
# Home page — tom geral, hero, branding
firecrawl scrape "https://exemplo.com" -o .firecrawl/home.md

# Pricing — componentes de card, tiers, CTA
firecrawl scrape "https://exemplo.com/pricing" -o .firecrawl/pricing.md

# Docs (se construindo docs page) — tipografia, step components
firecrawl scrape "https://exemplo.com/docs" -o .firecrawl/docs.md
```

Use a home como referência primária para tom e cor. Use páginas específicas para padrões de componentes relevantes ao tipo de página que está construindo.

---

## Extração com Screenshot

Quando o markdown for esparso mas um screenshot estiver disponível (formato `screenshot` do Firecrawl retorna URL de imagem):

1. Leia a imagem visualmente
2. Identifique: modo dark/light, cor dominante do background, cor do accent, família tipográfica aproximada
3. Estime o espaçamento pelo espaço visual entre elementos
4. Identifique 2–3 padrões de componente visíveis

O screenshot é especialmente útil para: animações, gradientes complexos, overlays, efeitos de blur/glassmorphism que não aparecem no markdown.

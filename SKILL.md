---
name: site-builder
description: |
  Build complete production-grade pages, landing pages, marketing sites, dashboards, and documentation pages. Use this skill when the user asks to "build a landing page", "create a site", "make a dashboard", "design a docs page", "build a marketing page", "clone the aesthetic of [URL]", "redesign this site", "extract the design system from [URL] and build a new page", or any request for a full-page or full-site output (not just a component or widget). Also triggers when the user provides a reference URL and wants something visually inspired by or functionally similar to it. Combines creative direction, Firecrawl-powered design system extraction, and structural page patterns to produce distinctive, production-ready HTML/CSS/JS output. Do NOT trigger for component-level or widget-level requests — use frontend-design for those.
allowed-tools:
  - Bash(firecrawl *)
---

# Site Builder

Build complete, production-grade pages and sites with distinctive aesthetics. Two modes:

- **Modo A — From Scratch**: usuário dá um concept ou brief → escolhe direção estética → constrói
- **Modo B — From Reference**: usuário fornece uma URL → Firecrawl extrai design system → constrói inspirado

---

## Step 1: Determinar Modo e Tipo de Página

Infira do contexto (ou pergunte uma vez antes de começar):

1. **Modo**: o usuário tem uma URL de referência, ou começa do zero?
2. **Tipo de página**: Landing page, dashboard, docs page, marketing site, ou outro?
3. **Stack**: HTML/CSS/JS puro (padrão), React, ou Vue?

Se não for claro, default para HTML/CSS/JS e faça uma pergunta antes de continuar.

**O tipo de página define a estrutura.** Leia `references/page-archetypes.md` para o template estrutural correspondente antes de escrever qualquer código. Cada arquétipo tem uma sequência de seções, inventário de componentes e grid de layout definidos.

---

## Modo A: From Scratch

### Design Thinking (antes de qualquer código)

Defina e comprometa-se com uma direção estética respondendo:

- **Propósito**: Que problema essa página resolve? Quem usa?
- **Tom**: Escolha uma direção e cometa-se. Opções: brutalmente minimal, editorial/magazine, retro-futuristic, industrial/utilitarian, luxury/refined, maximalist chaos, organic/natural, brutalist/raw, art deco/geometric, soft/pastel, playful/toy-like, sci-fi/terminal. Use como ponto de partida — construa uma direção verdadeira ao contexto.
- **Diferenciação**: O que torna essa página inesquecível? Qual é a única coisa que o usuário vai lembrar?
- **Tipografia**: Fontes caracterísitcas e inesperadas. Nunca Inter, Roboto, Arial ou system fonts. O triplo padrão (Syne + DM Mono + DM Sans) é um bom baseline, mas desvie quando o estilo exige. Nunca convirja para Space Grotesk entre gerações.
- **Cor**: Comprometa-se com uma paleta dominante com acentos nítidos. CSS custom properties para tudo. Dark e light são ambos válidos — varie entre eles. Nunca default para purple-gradient-on-white.

**Não comece a codar antes de ter uma direção estética clara e comprometida.**

### Design System Baseline

Inicie toda página a partir da arquitetura de CSS custom properties em `references/design-tokens.md`. Ela define:
- Tokens de cor semânticos para dark e light themes
- Escala tipográfica com `--font-display`, `--font-mono`, `--font-body`
- Escala de espaçamento como tokens `--space-*`
- Border radius, shadow, e transition tokens

Customize esses tokens para a direção estética escolhida. O baseline existe para ser substituído — mude os valores, não a estrutura.

### Implementação

Produza código funcional e production-grade:

- Todo CSS via custom properties — sem valores hex hardcoded em regras de componentes
- Hierarquia tipográfica: display font para headings, body font para prosa, mono para labels/code/metadata
- Backgrounds: crie atmosfera. Gradient meshes, noise textures, geometric patterns, transparências em camadas. Evite solid colors planas como background principal.
- Motion: foque em momentos de alto impacto — um page load bem orquestrado com staggered reveals supera micro-interações espalhadas. Use `animation-delay` para entrada escalonada. Hover states que surpreendem.
- Composição espacial: use assimetria, overlap, fluxo diagonal e elementos que quebram o grid intencionalmente. Espaço negativo generoso OU densidade controlada — escolha um.
- Para step/phase components, callout boxes e stat cards, use os padrões de componentes em `references/design-tokens.md`.

---

## Modo B: From Reference URL

### Step 1: Scrape da Referência

```bash
firecrawl scrape "<url>" --format markdown,screenshot -o .firecrawl/ref-page.md
```

Se o site for pesado (SPA, CSS-in-JS):

```bash
firecrawl scrape "<url>" --wait-for 3000 --only-main-content -o .firecrawl/ref-page.md
```

Salve output em `.firecrawl/`. Se em um projeto, adicione `.firecrawl/` ao `.gitignore`.

Para sites com múltiplas páginas distintas (marketing site com pricing separado, etc.), scrape 2–3 páginas-chave.

### Step 2: Extração de Tokens

Leia `references/extraction-protocol.md` para o schema completo de extração.

Aplique o Design Token Extraction Protocol ao conteúdo scrapeado:

**Tokens de cor**: background dominante, surface/card, texto primário, texto secundário, accent/brand, cores semânticas (sucesso, aviso, erro). Dark ou light?

**Tokens tipográficos**: font display/heading, font body, font mono (se houver). Escala aproximada — tamanhos de heading, body, line-height, letter-spacing. Escolhas incomuns de weight ou estilo.

**Tokens de espaçamento**: section padding, content max-width, tamanhos de gap entre componentes, border-radius.

**Motion**: Transições rápidas/snappy, lentas/teatrais, ou ausentes? Animações assinatura?

**Padrões de componente**: Identifique 3–5 padrões de UI distintivos (ex: pill badges, outlined cards, gradient CTAs, split hero, numbered steps).

**Resumo estético**: 2–3 palavras (ex: "minimal enterprise dark", "playful consumer light", "technical documentation mono").

### Step 3: Confirmação antes de construir

Após extração, apresente o resumo ao usuário antes de gerar código:

```
Design system extraído de [URL]:
- Tema: dark / light
- Cores: bg [valor], surface [valor], accent [valor]
- Fontes: display [nome], body [nome], mono [nome se houver]
- Espaçamento: seções [valor], cards [valor], radius [valor]
- Motion: [descrição]
- Padrões distinctivos: [lista]
- Estética: [2-3 palavras]

Construindo: [tipo de página] inspirado nesse sistema.
```

### Step 4: Build

Use os tokens extraídos como valores das CSS custom properties do baseline. A página deve ser uma prima espiritual da referência — mesmo DNA, execução diferente. Não copie layouts nem texto; reinterprete.

---

## Padrões de Output

Toda página gerada deve:

- Usar HTML5 semântico (`<header>`, `<main>`, `<section>`, `<article>`, `<nav>`, `<footer>`)
- Definir todos os valores visuais como CSS custom properties em `:root`
- Importar fontes do Google Fonts ou usar system font stacks com fallbacks específicos
- Incluir `<meta name="viewport">` e responsividade básica
- Produzir um único arquivo HTML self-contained (CSS inline em `<style>`, JS inline em `<script>`) por padrão — a menos que o usuário peça estrutura multi-arquivo
- Funcionar sem build step

Para output React, use um único `.jsx` com inline styles ou um `.css` companion com custom properties.

---

## Tipos de Página — Referência Rápida

Veja `references/page-archetypes.md` para os templates estruturais completos.

| Tipo | Seções-chave | Padrão de Layout |
|---|---|---|
| Landing Page | Hero, features, social proof, CTA, footer | Coluna única, seções full-bleed |
| Dashboard | Sidebar, header, stat cards, charts, tables | Sidebar fixa + main grid fluido |
| Docs Page | Sidebar TOC, content, callouts, steps | Sidebar + coluna de conteúdo principal |
| Marketing Site | Nav, hero, features, pricing, FAQ, footer | Multi-seção com nav ancorada |

---

## Sistema Tipográfico Baseline

O triplo padrão: **Syne** (display, headings) + **DM Mono** (labels, code, metadata, captions) + **DM Sans** (body prose, UI text).

Use DM Mono não apenas para code blocks, mas também para: números de step, stat labels, timestamps, version numbers, qualquer texto "de dado". Isso cria uma qualidade editorial distintiva.

Desvie do triplo quando a direção estética exigir — mas sempre cubra os três papéis tipográficos.

---

## Anti-patterns (Nunca Faça)

- Valores hex hardcoded em regras de componentes CSS
- Inter, Roboto ou Arial como fonte display principal
- Purple-gradient-on-white como esquema de cor padrão
- Solid colors planos como único tratamento de background
- Layout genérico "card com sombra em página branca"
- Copiar o layout da referência pixel-a-pixel no Modo B
- Gerar a mesma estética duas vezes — cada página deve ser distinta
- Omitir motion completamente (até designs minimais se beneficiam de transições sutis)
- Hero clichê: texto centralizado + botão + imagem hero. Subverta.

---

## Recursos de Referência

- `references/design-tokens.md` — Arquitetura completa de CSS custom properties, dark theme baseline, padrões de componentes (steps, callouts, stat cards)
- `references/page-archetypes.md` — Templates estruturais e sequências de seções por tipo de página
- `references/extraction-protocol.md` — Workflow Firecrawl completo e schema de extração de tokens para builds baseados em referência

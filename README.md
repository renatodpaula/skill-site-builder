# skill-site-builder

Claude Code skill para construir páginas e sites completos com estética distintiva e código production-grade.

## O que faz

Gera HTML/CSS/JS (ou React/Vue) para páginas inteiras — landing pages, dashboards, docs pages, marketing sites. Não serve para componentes isolados (use `frontend-design` para isso).

Dois modos de operação:

- **From Scratch** — você dá um conceito ou brief, a skill define uma direção estética e constrói
- **From Reference** — você fornece uma URL, a skill usa Firecrawl para extrair o design system e constrói algo inspirado

## Instalação

```bash
npx skills add renatodpaula/skill-site-builder
```

## Triggers

Use em qualquer uma dessas situações:

- "build a landing page for..."
- "create a site for..."
- "make a dashboard with..."
- "design a docs page"
- "clone the aesthetic of [URL]"
- "redesign this site"
- "extract the design system from [URL] and build a new page"

## Modo From Scratch

A skill define e commita com uma direção estética antes de codar:

- **Tom**: brutally minimal, editorial, retro-futuristic, industrial, luxury, brutalist, art deco, sci-fi/terminal, etc.
- **Tipografia**: nunca Inter/Roboto/Arial como display. Baseline: Syne (display) + DM Mono (labels/metadata) + DM Sans (corpo)
- **Cor**: paleta dominante com acentos nítidos, tudo via CSS custom properties. Nunca purple-gradient-on-white
- **Background**: gradient meshes, noise textures, padrões geométricos — sem solid colors planas

## Modo From Reference

```
você: "constrói uma landing page com o estilo de [url]"
```

1. Firecrawl scrape da URL
2. Extração de tokens: cores, tipografia, espaçamento, motion, padrões de componente
3. Apresenta resumo do design system extraído para confirmação
4. Constrói — mesma estética, execução diferente (não copia layout)

## Output

Toda página gerada:

- HTML5 semântico
- Todos os valores visuais como CSS custom properties em `:root`
- Single file self-contained por padrão (CSS/JS inline) — sem build step
- Responsivo com `<meta name="viewport">`
- Fontes do Google Fonts com fallbacks

## Tipos de página suportados

| Tipo | Seções-chave |
|---|---|
| Landing Page | Hero, features, social proof, CTA, footer |
| Dashboard | Sidebar, stat cards, charts, tables |
| Docs Page | Sidebar TOC, content, callouts, steps |
| Marketing Site | Nav, hero, features, pricing, FAQ, footer |

## Arquivos de referência

A skill inclui três arquivos de referência usados internamente:

- `references/design-tokens.md` — arquitetura de CSS custom properties, dark theme baseline, padrões de componente (steps, callouts, stat cards)
- `references/page-archetypes.md` — templates estruturais e sequência de seções por tipo de página
- `references/extraction-protocol.md` — workflow Firecrawl e schema de extração de tokens para o Modo B

## Anti-patterns (o que a skill evita)

- Valores hex hardcoded em regras CSS
- Inter/Roboto/Arial como fonte display
- Purple-gradient-on-white
- Solid color como único tratamento de background
- Card com sombra em página branca
- Copiar layout da referência pixel-a-pixel
- Gerar a mesma estética duas vezes
- Hero clichê: texto centralizado + botão + imagem

## Dependências

Requer o CLI `firecrawl` instalado localmente para o Modo B (From Reference). O Modo A não tem dependências externas.

```bash
npm install -g @mendable/firecrawl-js
```

# Page Archetypes — Templates Estruturais

## 1. Landing Page

**Objetivo**: converter visitantes em leads/clientes. Hierarquia de informação: problema → solução → prova → ação.

### Sequência de Seções

```
<header>        Sticky nav: logo + links + CTA button
<section#hero>  Full-viewport hero — subverta o centered-text padrão
<section#logos> Logos/social proof strip (confiança imediata)
<section#features-grid>  3–4 features em grid (visão geral)
<section#feature-detail> Feature alternada com visual (profundidade)
<section#testimonials>   Depoimentos ou métricas de prova
<section#cta>            CTA block final, alta intensidade
<footer>        Links secundários, legal, social
```

### Padrões de Hero a Usar (nunca centered text + button + image)

**Split hero**: texto à esquerda, visual à direita. Linha diagonal separando as metades.
**Overlay hero**: texto sobre background de vídeo/imagem com overlay escuro.
**Terminal hero**: prompt de código animado como feature central.
**Stats hero**: grande número/métrica em destaque + contexto.
**Asymmetric hero**: texto em 2/3 da largura, accent element sangrando para fora do container.

### Layout CSS

```css
/* Hero — full viewport com padding generoso */
#hero {
  min-height: 100vh;
  display: grid;
  grid-template-columns: 1fr 1fr;
  align-items: center;
  gap: var(--space-16);
  padding: var(--section-padding-y) var(--section-padding-x);
  max-width: var(--max-width-wide);
  margin: 0 auto;
}

/* Features grid — responsive sem breakpoints */
.features-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: var(--space-6);
}

/* Feature alternada */
.feature-detail {
  display: grid;
  grid-template-columns: 1fr 1fr;
  align-items: center;
  gap: var(--space-16);
}
.feature-detail:nth-child(even) { direction: rtl; }
.feature-detail > * { direction: ltr; }
```

### Nav Sticky

```css
header {
  position: sticky;
  top: 0;
  z-index: 100;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: var(--space-4) var(--section-padding-x);
  background: var(--color-surface-overlay);
  backdrop-filter: blur(12px);
  border-bottom: 1px solid var(--color-border-subtle);
}
```

---

## 2. Dashboard

**Objetivo**: visualizar e monitorar dados. Densidade controlada. Tudo acessível sem scroll quando possível.

### Layout Principal

```css
.dashboard-layout {
  display: grid;
  grid-template-areas:
    "sidebar header"
    "sidebar main";
  grid-template-columns: 240px 1fr;
  grid-template-rows: 60px 1fr;
  height: 100vh;
  overflow: hidden;
}

.sidebar { grid-area: sidebar; overflow-y: auto; }
.dashboard-header { grid-area: header; }
.dashboard-main   { grid-area: main; overflow-y: auto; padding: var(--space-6); }
```

### Sidebar

```html
<aside class="sidebar">
  <div class="sidebar-logo">...</div>
  <nav class="sidebar-nav">
    <a class="nav-item active" href="#overview">
      <span class="nav-icon">◆</span>
      <span class="nav-label">Overview</span>
    </a>
    <!-- ... -->
  </nav>
  <div class="sidebar-footer">
    <!-- user avatar, settings -->
  </div>
</aside>
```

```css
.sidebar {
  background: var(--color-surface);
  border-right: 1px solid var(--color-border);
  display: flex;
  flex-direction: column;
  padding: var(--space-4) 0;
}

.nav-item {
  display: flex;
  align-items: center;
  gap: var(--space-3);
  padding: var(--space-3) var(--space-5);
  font-family: var(--font-mono);
  font-size: var(--text-sm);
  letter-spacing: var(--tracking-wide);
  color: var(--color-text-muted);
  text-decoration: none;
  border-left: 2px solid transparent;
  transition: all var(--transition);
}

.nav-item:hover { color: var(--color-text-primary); background: var(--color-surface-raised); }
.nav-item.active { color: var(--color-accent); border-left-color: var(--color-accent); }
```

### Dashboard Header

```css
.dashboard-header {
  background: var(--color-surface);
  border-bottom: 1px solid var(--color-border);
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0 var(--space-6);
}
```

### Stat Cards Row

```html
<div class="stats-row">
  <div class="stat-card">...</div>
  <div class="stat-card">...</div>
  <div class="stat-card">...</div>
  <div class="stat-card">...</div>
</div>
```

```css
.stats-row {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: var(--space-5);
  margin-bottom: var(--space-6);
}
```

### Área de Gráficos

```css
.charts-area {
  display: grid;
  grid-template-columns: 2fr 1fr;
  gap: var(--space-5);
}

.chart-card {
  background: var(--color-surface);
  border: 1px solid var(--color-border);
  border-radius: var(--radius-lg);
  padding: var(--space-5);
}

.chart-header {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  margin-bottom: var(--space-5);
}

.chart-title {
  font-family: var(--font-mono);
  font-size: var(--text-xs);
  letter-spacing: var(--tracking-wider);
  text-transform: uppercase;
  color: var(--color-text-muted);
}
```

### Tabela de Dados

```css
.data-table { width: 100%; border-collapse: collapse; }
.data-table th {
  font-family: var(--font-mono);
  font-size: var(--text-xs);
  letter-spacing: var(--tracking-wider);
  text-transform: uppercase;
  color: var(--color-text-muted);
  text-align: left;
  padding: var(--space-3) var(--space-4);
  border-bottom: 1px solid var(--color-border);
}
.data-table td {
  padding: var(--space-3) var(--space-4);
  border-bottom: 1px solid var(--color-border-subtle);
  font-size: var(--text-sm);
  color: var(--color-text-secondary);
}
.data-table tr:hover td { background: var(--color-surface-raised); }
```

---

## 3. Docs Page

**Objetivo**: documentação técnica legível. O ritmo tipográfico é o design.

### Layout

```css
.docs-layout {
  display: grid;
  grid-template-columns: 260px 1fr;
  grid-template-areas: "sidebar content";
  min-height: 100vh;
}

.docs-sidebar { grid-area: sidebar; position: sticky; top: 0; height: 100vh; overflow-y: auto; }
.docs-content { grid-area: content; max-width: var(--max-width-prose); padding: var(--space-16) var(--space-10); }
```

### Sidebar TOC

```css
.docs-sidebar {
  background: var(--color-surface);
  border-right: 1px solid var(--color-border);
  padding: var(--space-8) var(--space-5);
}

.toc-section-title {
  font-family: var(--font-mono);
  font-size: var(--text-xs);
  letter-spacing: var(--tracking-widest);
  text-transform: uppercase;
  color: var(--color-text-muted);
  margin-bottom: var(--space-3);
  padding-left: var(--space-3);
}

.toc-link {
  display: block;
  font-size: var(--text-sm);
  color: var(--color-text-secondary);
  padding: var(--space-2) var(--space-3);
  border-radius: var(--radius-sm);
  border-left: 2px solid transparent;
  text-decoration: none;
  transition: all var(--transition);
}
.toc-link:hover { color: var(--color-text-primary); background: var(--color-surface-raised); }
.toc-link.active { color: var(--color-accent); border-left-color: var(--color-accent); }
```

### Conteúdo Tipográfico

```css
.docs-content h1 { font-size: var(--text-3xl); margin-bottom: var(--space-3); }
.docs-content h2 { font-size: var(--text-2xl); margin-top: var(--space-16); margin-bottom: var(--space-4); padding-bottom: var(--space-3); border-bottom: 1px solid var(--color-border); }
.docs-content h3 { font-size: var(--text-xl); margin-top: var(--space-10); margin-bottom: var(--space-3); }
.docs-content p  { margin-bottom: var(--space-5); color: var(--color-text-secondary); }
.docs-content ul, .docs-content ol { margin-bottom: var(--space-5); padding-left: var(--space-6); color: var(--color-text-secondary); }
.docs-content li { margin-bottom: var(--space-2); }
```

### Navegação Prev/Next

```html
<nav class="docs-nav-footer">
  <a class="docs-nav-item prev" href="#">
    <span class="nav-dir">← Anterior</span>
    <span class="nav-title">Título da página anterior</span>
  </a>
  <a class="docs-nav-item next" href="#">
    <span class="nav-dir">Próximo →</span>
    <span class="nav-title">Título da próxima página</span>
  </a>
</nav>
```

```css
.docs-nav-footer {
  display: flex;
  justify-content: space-between;
  gap: var(--space-5);
  margin-top: var(--space-20);
  padding-top: var(--space-8);
  border-top: 1px solid var(--color-border);
}

.docs-nav-item {
  display: flex;
  flex-direction: column;
  gap: var(--space-1);
  text-decoration: none;
  padding: var(--space-4) var(--space-5);
  border: 1px solid var(--color-border);
  border-radius: var(--radius-md);
  flex: 1;
  transition: border-color var(--transition);
}
.docs-nav-item.next { text-align: right; }
.docs-nav-item:hover { border-color: var(--color-accent); }
.nav-dir { font-family: var(--font-mono); font-size: var(--text-xs); color: var(--color-text-muted); text-transform: uppercase; letter-spacing: var(--tracking-wider); }
.nav-title { font-size: var(--text-md); font-weight: 600; color: var(--color-text-primary); }
```

---

## 4. Marketing Site

**Objetivo**: site completo multi-seção com múltiplas CTAs. Nav com anchor scroll.

### Sequência de Seções

```
<header>         Nav sticky com logo + links âncora + CTA
<section#hero>   Hero poderoso, full-viewport
<section#features>  Features com ícones e descrições
<section#how>    How it works — steps numerados
<section#pricing>  Pricing tiers com tabela de features
<section#faq>    FAQ accordion
<section#cta-final> Newsletter ou trial CTA
<footer>         Links, legal, social
```

### Pricing Tiers

```html
<div class="pricing-grid">
  <div class="pricing-card">
    <div class="pricing-header">
      <span class="plan-name">Starter</span>
      <div class="plan-price"><span class="price-amount">$0</span><span class="price-period">/mês</span></div>
    </div>
    <ul class="features-list">...</ul>
    <a class="plan-cta" href="#">Começar grátis</a>
  </div>
  <div class="pricing-card featured">...</div>
</div>
```

```css
.pricing-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: var(--space-6);
  align-items: start;
}

.pricing-card {
  background: var(--color-surface);
  border: 1px solid var(--color-border);
  border-radius: var(--radius-xl);
  padding: var(--space-8);
  display: flex;
  flex-direction: column;
  gap: var(--space-6);
}

.pricing-card.featured {
  border-color: var(--color-accent);
  background: var(--color-surface-raised);
  transform: scale(1.02);
}

.plan-name {
  font-family: var(--font-mono);
  font-size: var(--text-xs);
  letter-spacing: var(--tracking-widest);
  text-transform: uppercase;
  color: var(--color-text-muted);
}

.price-amount {
  font-family: var(--font-display);
  font-size: var(--text-4xl);
  font-weight: 800;
  letter-spacing: var(--tracking-tight);
  color: var(--color-text-primary);
}

.price-period {
  font-size: var(--text-base);
  color: var(--color-text-muted);
}
```

### FAQ Accordion

```html
<div class="faq-list">
  <details class="faq-item">
    <summary class="faq-question">Pergunta frequente aqui?</summary>
    <div class="faq-answer"><p>Resposta aqui.</p></div>
  </details>
</div>
```

```css
.faq-item {
  border-bottom: 1px solid var(--color-border);
}

.faq-question {
  padding: var(--space-5) 0;
  font-size: var(--text-md);
  font-weight: 600;
  cursor: pointer;
  list-style: none;
  display: flex;
  justify-content: space-between;
  align-items: center;
  color: var(--color-text-primary);
}

.faq-question::after {
  content: '+';
  font-family: var(--font-mono);
  font-size: var(--text-xl);
  color: var(--color-accent);
  flex-shrink: 0;
  transition: transform var(--transition);
}

details[open] .faq-question::after { transform: rotate(45deg); }

.faq-answer {
  padding-bottom: var(--space-5);
  color: var(--color-text-secondary);
  line-height: var(--leading-relaxed);
}
```

### CTA Final / Newsletter

```css
.cta-final {
  text-align: center;
  padding: var(--space-32) var(--section-padding-x);
  background: radial-gradient(ellipse at center, rgba(232,255,71,0.08) 0%, transparent 70%);
  border-top: 1px solid var(--color-border);
}

.cta-form {
  display: flex;
  gap: var(--space-3);
  max-width: 480px;
  margin: var(--space-8) auto 0;
}

.cta-input {
  flex: 1;
  background: var(--color-surface);
  border: 1px solid var(--color-border);
  border-radius: var(--radius-md);
  padding: var(--space-3) var(--space-4);
  font-family: var(--font-body);
  font-size: var(--text-base);
  color: var(--color-text-primary);
  transition: border-color var(--transition);
}
.cta-input:focus { outline: none; border-color: var(--color-accent); }

.cta-btn {
  background: var(--color-accent);
  color: var(--color-accent-text);
  border: none;
  border-radius: var(--radius-md);
  padding: var(--space-3) var(--space-6);
  font-family: var(--font-mono);
  font-size: var(--text-sm);
  font-weight: 500;
  letter-spacing: var(--tracking-wide);
  text-transform: uppercase;
  cursor: pointer;
  transition: opacity var(--transition);
  white-space: nowrap;
}
.cta-btn:hover { opacity: 0.85; }
```

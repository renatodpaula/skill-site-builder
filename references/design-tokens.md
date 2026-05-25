# Design Tokens — Baseline Architecture

## CSS Custom Property Architecture

Dois layers: **primitivos** (valores brutos) → **semânticos** (mapeados para papéis de UI). Theming é feito mudando apenas a camada semântica.

### Layer 1: Primitivos de Cor

```css
:root {
  /* Neutros */
  --black:        #000000;
  --gray-950:     #0a0a0b;
  --gray-900:     #111113;
  --gray-850:     #18181b;
  --gray-800:     #1a1a1e;
  --gray-700:     #27272a;
  --gray-600:     #3f3f46;
  --gray-500:     #52525b;
  --gray-400:     #71717a;
  --gray-300:     #a1a1aa;
  --gray-200:     #d4d4d8;
  --gray-100:     #f4f4f5;
  --white:        #ffffff;

  /* Accent — lime yellow (padrão; substitua pelo contexto) */
  --accent-400:   #d9f52a;
  --accent-500:   #e8ff47;
  --accent-600:   #bdd614;

  /* Accent secundário — cyan */
  --cyan-400:     #22d3ee;
  --cyan-500:     #47b3ff;

  /* Semânticos de estado */
  --green-500:    #22c55e;
  --yellow-500:   #eab308;
  --orange-500:   #f97316;
  --red-500:      #ef4444;
}
```

### Layer 2: Dark Theme (padrão)

```css
:root {
  /* Surfaces */
  --color-bg:             var(--gray-950);   /* background da página */
  --color-surface:        var(--gray-900);   /* cards, panels */
  --color-surface-raised: var(--gray-800);   /* elementos elevados, dropdowns */
  --color-surface-overlay:rgba(17,17,19,0.8);/* overlays, modals */

  /* Bordas */
  --color-border:         var(--gray-700);   /* bordas padrão */
  --color-border-subtle:  var(--gray-800);   /* separadores sutis */

  /* Texto */
  --color-text-primary:   var(--gray-100);
  --color-text-secondary: var(--gray-300);
  --color-text-muted:     var(--gray-500);
  --color-text-disabled:  var(--gray-600);

  /* Accent */
  --color-accent:         var(--accent-500);
  --color-accent-dim:     var(--accent-600);
  --color-accent-text:    var(--gray-950);   /* texto sobre fundo accent */

  /* Estado */
  --color-success:        var(--green-500);
  --color-warning:        var(--orange-500);
  --color-error:          var(--red-500);
  --color-info:           var(--cyan-500);
}
```

### Layer 2: Light Theme Override

```css
.light {
  --color-bg:             var(--white);
  --color-surface:        var(--gray-100);
  --color-surface-raised: var(--white);
  --color-surface-overlay:rgba(255,255,255,0.9);

  --color-border:         var(--gray-200);
  --color-border-subtle:  var(--gray-100);

  --color-text-primary:   var(--gray-900);
  --color-text-secondary: var(--gray-600);
  --color-text-muted:     var(--gray-400);
  --color-text-disabled:  var(--gray-300);

  --color-accent:         var(--accent-600);
  --color-accent-dim:     var(--accent-500);
  --color-accent-text:    var(--white);
}
```

---

## Sistema Tipográfico

### Google Fonts Import

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Syne:wght@400;600;700;800&family=DM+Mono:ital,wght@0,300;0,400;0,500;1,300&family=DM+Sans:ital,opsz,wght@0,9..40,300;0,9..40,400;0,9..40,500;1,9..40,300&display=swap" rel="stylesheet">
```

### Tokens Tipográficos

```css
:root {
  --font-display: 'Syne', sans-serif;           /* headings, display, números grandes */
  --font-body:    'DM Sans', sans-serif;         /* prosa, UI text */
  --font-mono:    'DM Mono', monospace;          /* code, labels, metadata, timestamps */

  /* Escala de tamanhos */
  --text-xs:   11px;
  --text-sm:   13px;
  --text-base: 15px;
  --text-md:   17px;
  --text-lg:   20px;
  --text-xl:   24px;
  --text-2xl:  32px;
  --text-3xl:  clamp(36px, 5vw, 56px);
  --text-4xl:  clamp(48px, 7vw, 80px);

  /* Line heights */
  --leading-tight:  1.2;
  --leading-snug:   1.4;
  --leading-normal: 1.6;
  --leading-relaxed:1.75;

  /* Letter spacings */
  --tracking-tight: -0.03em;
  --tracking-normal: 0;
  --tracking-wide:  0.08em;
  --tracking-wider: 0.12em;
  --tracking-widest:0.18em;
}
```

### Reset Tipográfico Base

```css
*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

html { font-size: 16px; -webkit-font-smoothing: antialiased; }

body {
  font-family: var(--font-body);
  font-size: var(--text-base);
  font-weight: 300;
  line-height: var(--leading-relaxed);
  color: var(--color-text-primary);
  background-color: var(--color-bg);
}

h1, h2, h3, h4, h5, h6 {
  font-family: var(--font-display);
  line-height: var(--leading-tight);
  letter-spacing: var(--tracking-tight);
  color: var(--color-text-primary);
}

code, kbd, pre, samp {
  font-family: var(--font-mono);
}
```

---

## Escala de Espaçamento

```css
:root {
  --space-1:   4px;
  --space-2:   8px;
  --space-3:   12px;
  --space-4:   16px;
  --space-5:   20px;
  --space-6:   24px;
  --space-8:   32px;
  --space-10:  40px;
  --space-12:  48px;
  --space-16:  64px;
  --space-20:  80px;
  --space-24:  96px;
  --space-32:  128px;

  /* Layout */
  --max-width-content: 900px;
  --max-width-wide:    1200px;
  --max-width-prose:   72ch;
  --section-padding-x: clamp(var(--space-5), 5vw, var(--space-20));
  --section-padding-y: clamp(var(--space-16), 8vw, var(--space-32));
}
```

---

## Border Radius, Shadows, Transitions

```css
:root {
  /* Border radius */
  --radius-sm:   4px;
  --radius-md:   8px;
  --radius-lg:   12px;
  --radius-xl:   16px;
  --radius-full: 9999px;

  /* Shadows */
  --shadow-sm:  0 1px 2px rgba(0,0,0,0.4);
  --shadow-md:  0 4px 16px rgba(0,0,0,0.5);
  --shadow-lg:  0 8px 32px rgba(0,0,0,0.6);
  --shadow-accent: 0 0 24px rgba(232,255,71,0.15);

  /* Transitions */
  --ease-default: cubic-bezier(0.4, 0, 0.2, 1);
  --ease-spring:  cubic-bezier(0.34, 1.56, 0.64, 1);
  --duration-fast:   150ms;
  --duration-normal: 250ms;
  --duration-slow:   400ms;

  --transition: var(--duration-normal) var(--ease-default);
}
```

---

## Padrões de Componentes Baseline

### Step / Phase Component

Números de step com linha conectora vertical. DM Mono para o número.

```html
<div class="steps">
  <div class="step">
    <div class="step-indicator">
      <span class="step-number">01</span>
      <div class="step-line"></div>
    </div>
    <div class="step-content">
      <h3 class="step-title">Título do passo</h3>
      <p>Conteúdo do passo...</p>
    </div>
  </div>
</div>
```

```css
.steps { display: flex; flex-direction: column; gap: 0; }

.step { display: grid; grid-template-columns: 56px 1fr; gap: var(--space-6); }

.step-indicator {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0;
}

.step-number {
  width: 56px;
  height: 56px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-family: var(--font-mono);
  font-size: var(--text-sm);
  font-weight: 500;
  color: var(--color-accent);
  border: 1px solid var(--color-border);
  border-radius: var(--radius-md);
  background: var(--color-surface);
  flex-shrink: 0;
}

.step-line {
  width: 1px;
  flex: 1;
  min-height: var(--space-8);
  background: linear-gradient(to bottom, var(--color-border), transparent);
  margin: var(--space-2) 0;
}

.step-content { padding-bottom: var(--space-10); }
.step-title { font-size: var(--text-xl); margin-bottom: var(--space-3); }
.step:last-child .step-line { display: none; }
```

### Callout Box — 4 variantes

```html
<div class="callout callout-tip">
  <span class="callout-label">Dica</span>
  <p>Conteúdo da callout.</p>
</div>
```

```css
.callout {
  padding: var(--space-4) var(--space-5);
  border-left: 3px solid;
  border-radius: 0 var(--radius-md) var(--radius-md) 0;
  margin: var(--space-6) 0;
}

.callout-tip      { border-color: var(--color-accent);  background: rgba(232,255,71,0.06); }
.callout-info     { border-color: var(--color-info);    background: rgba(71,179,255,0.06); }
.callout-warning  { border-color: var(--color-warning); background: rgba(249,115,22,0.06); }
.callout-critical { border-color: var(--color-error);   background: rgba(239,68,68,0.06);  }

.callout-label {
  display: block;
  font-family: var(--font-mono);
  font-size: var(--text-xs);
  font-weight: 500;
  letter-spacing: var(--tracking-widest);
  text-transform: uppercase;
  color: var(--color-text-muted);
  margin-bottom: var(--space-2);
}

.callout-tip      .callout-label { color: var(--color-accent);  }
.callout-info     .callout-label { color: var(--color-info);    }
.callout-warning  .callout-label { color: var(--color-warning); }
.callout-critical .callout-label { color: var(--color-error);   }
```

### Stat Card

```html
<div class="stat-card">
  <span class="stat-label">Usuários Ativos</span>
  <span class="stat-value">24.8K</span>
  <span class="stat-delta stat-up">+12.4%</span>
</div>
```

```css
.stat-card {
  background: var(--color-surface);
  border: 1px solid var(--color-border);
  border-radius: var(--radius-lg);
  padding: var(--space-5) var(--space-6);
  display: flex;
  flex-direction: column;
  gap: var(--space-2);
  transition: border-color var(--transition);
}

.stat-card:hover { border-color: var(--color-accent); }

.stat-label {
  font-family: var(--font-mono);
  font-size: var(--text-xs);
  letter-spacing: var(--tracking-wider);
  text-transform: uppercase;
  color: var(--color-text-muted);
}

.stat-value {
  font-family: var(--font-display);
  font-size: var(--text-3xl);
  font-weight: 800;
  letter-spacing: var(--tracking-tight);
  color: var(--color-text-primary);
  line-height: 1;
}

.stat-delta {
  font-family: var(--font-mono);
  font-size: var(--text-sm);
  font-weight: 500;
}

.stat-up   { color: var(--color-success); }
.stat-down { color: var(--color-error);   }
```

### Code Block

```html
<div class="code-block">
  <div class="code-header">
    <span class="code-lang">bash</span>
    <button class="copy-btn" onclick="copyCode(this)">COPIAR</button>
  </div>
  <pre><code>firecrawl scrape "https://example.com"</code></pre>
</div>
```

```css
.code-block {
  background: var(--color-surface);
  border: 1px solid var(--color-border);
  border-radius: var(--radius-md);
  overflow: hidden;
  margin: var(--space-5) 0;
}

.code-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: var(--space-2) var(--space-4);
  border-bottom: 1px solid var(--color-border);
  background: var(--color-surface-raised);
}

.code-lang {
  font-family: var(--font-mono);
  font-size: var(--text-xs);
  letter-spacing: var(--tracking-wider);
  text-transform: uppercase;
  color: var(--color-text-muted);
}

.copy-btn {
  font-family: var(--font-mono);
  font-size: var(--text-xs);
  letter-spacing: var(--tracking-wider);
  color: var(--color-text-muted);
  background: none;
  border: none;
  cursor: pointer;
  transition: color var(--transition);
}
.copy-btn:hover { color: var(--color-accent); }

.code-block pre {
  padding: var(--space-4) var(--space-5);
  overflow-x: auto;
  font-family: var(--font-mono);
  font-size: var(--text-sm);
  line-height: var(--leading-relaxed);
  color: var(--color-text-secondary);
}
```

### Tag / Badge

```html
<span class="tag">design-system</span>
```

```css
.tag {
  display: inline-flex;
  align-items: center;
  font-family: var(--font-mono);
  font-size: var(--text-xs);
  font-weight: 500;
  letter-spacing: var(--tracking-wider);
  text-transform: uppercase;
  color: var(--color-accent);
  border: 1px solid currentColor;
  border-radius: var(--radius-sm);
  padding: 2px var(--space-2);
}
```

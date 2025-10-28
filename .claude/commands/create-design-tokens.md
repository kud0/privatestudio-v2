# Create Design Tokens

Create CSS custom properties for consistent styling across the site.

## Task
Create `public/styles/design-tokens.css` with all design variables extracted from AMIRO analysis.

## CSS Variables Structure

```css
:root {
  /* ============================================
     COLORS
     ============================================ */

  /* Backgrounds */
  --color-bg-black: #000000;
  --color-bg-dark: #2F2F2F;
  --color-bg-darker: #1a1a1a;

  /* Text */
  --color-text-primary: #E2E0DF;
  --color-text-secondary: #968C8C;
  --color-text-muted: rgba(226, 224, 223, 0.6);

  /* Accents */
  --color-accent-bronze: #968C8C;
  --color-accent-blue: #1863DC;

  /* Borders */
  --color-border: rgba(255, 255, 255, 0.1);
  --color-border-hover: rgba(255, 255, 255, 0.3);

  /* ============================================
     TYPOGRAPHY
     ============================================ */

  /* Font Families */
  --font-primary: -apple-system, BlinkMacSystemFont, 'Segoe UI', 'Roboto', 'Oxygen', 'Ubuntu', sans-serif;
  --font-display: 'Arial Black', sans-serif;

  /* Font Sizes - Mobile First */
  --font-size-xs: 13px;
  --font-size-sm: 16px;
  --font-size-base: 20px;
  --font-size-lg: 24px;
  --font-size-xl: 36px;
  --font-size-2xl: 48px;
  --font-size-3xl: 60px;
  --font-size-4xl: 80px;
  --font-size-5xl: 100px;
  --font-size-6xl: 120px;
  --font-size-display: 180px; /* HYPER BIG for BOOK NOW, TEAM */

  /* Font Weights */
  --font-weight-normal: 400;
  --font-weight-medium: 500;
  --font-weight-semibold: 600;
  --font-weight-bold: 700;
  --font-weight-black: 900;

  /* Line Heights */
  --line-height-tight: 0.9;
  --line-height-normal: 1.5;
  --line-height-relaxed: 1.8;

  /* Letter Spacing */
  --letter-spacing-tight: -0.02em;
  --letter-spacing-normal: 0;
  --letter-spacing-wide: 0.05em;

  /* ============================================
     SPACING
     ============================================ */

  --spacing-xs: 0.44rem;    /* ~7px */
  --spacing-sm: 1rem;       /* 16px */
  --spacing-md: 1.5rem;     /* 24px */
  --spacing-lg: 2rem;       /* 32px */
  --spacing-xl: 3rem;       /* 48px */
  --spacing-2xl: 4rem;      /* 64px */
  --spacing-3xl: 5.06rem;   /* ~81px */
  --spacing-4xl: 6rem;      /* 96px */
  --spacing-5xl: 8rem;      /* 128px */

  /* ============================================
     LAYOUT
     ============================================ */

  --container-max-width: 1400px;
  --container-padding: var(--spacing-lg);

  --grid-gap: var(--spacing-lg);
  --grid-gap-mobile: var(--spacing-md);

  /* ============================================
     SHADOWS
     ============================================ */

  --shadow-sm: 0 1px 2px rgba(0, 0, 0, 0.05);
  --shadow-md: 0 4px 6px rgba(0, 0, 0, 0.1);
  --shadow-lg: 6px 6px 9px rgba(0, 0, 0, 0.2);
  --shadow-xl: 0 20px 25px rgba(0, 0, 0, 0.3);
  --shadow-glow: 0 0 20px rgba(150, 140, 140, 0.3);

  /* ============================================
     TRANSITIONS
     ============================================ */

  --transition-fast: 0.15s ease;
  --transition-base: 0.3s ease;
  --transition-slow: 0.5s ease;

  --easing-default: cubic-bezier(0.4, 0, 0.2, 1);
  --easing-in: cubic-bezier(0.4, 0, 1, 1);
  --easing-out: cubic-bezier(0, 0, 0.2, 1);
  --easing-bounce: cubic-bezier(0.68, -0.55, 0.265, 1.55);

  /* ============================================
     BORDERS
     ============================================ */

  --border-radius-sm: 4px;
  --border-radius-md: 8px;
  --border-radius-lg: 12px;
  --border-radius-xl: 16px;
  --border-radius-full: 9999px;

  --border-width: 1px;

  /* ============================================
     Z-INDEX
     ============================================ */

  --z-base: 0;
  --z-dropdown: 10;
  --z-sticky: 20;
  --z-fixed: 30;
  --z-modal-backdrop: 40;
  --z-modal: 50;
  --z-popover: 60;
  --z-tooltip: 70;
}

/* ============================================
   RESPONSIVE BREAKPOINTS
   ============================================ */

@media (min-width: 768px) {
  :root {
    /* Tablet font sizes */
    --font-size-2xl: 60px;
    --font-size-3xl: 80px;
    --font-size-4xl: 100px;
    --font-size-5xl: 120px;
    --font-size-display: 120px;

    --container-padding: var(--spacing-xl);
  }
}

@media (min-width: 1024px) {
  :root {
    /* Desktop font sizes */
    --font-size-2xl: 72px;
    --font-size-3xl: 100px;
    --font-size-4xl: 120px;
    --font-size-5xl: 150px;
    --font-size-display: 180px;

    --container-padding: var(--spacing-2xl);
  }
}

@media (min-width: 1440px) {
  :root {
    /* Large desktop */
    --font-size-display: 200px;
  }
}
```

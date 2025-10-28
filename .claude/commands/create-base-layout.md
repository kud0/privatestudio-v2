# Create Base Layout

Create the foundational Astro layout component that all pages will use.

## Task
Create `src/layouts/BaseLayout.astro` with:
- HTML5 semantic structure
- Meta tags for SEO
- Dark theme by default
- Responsive viewport configuration
- Link to global styles
- Smooth scroll behavior
- Include Navigation and Footer components

## Structure
```astro
---
import Navigation from '../components/Navigation.astro';
import Footer from '../components/Footer.astro';

interface Props {
  title?: string;
  description?: string;
}

const {
  title = 'PrivateStudio - Barbería Barcelona',
  description = 'Barbería profesional en Barcelona. Cortes modernos, servicio premium.'
} = Astro.props;
---

<!DOCTYPE html>
<html lang="es" class="scroll-smooth">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta name="description" content={description}>
  <title>{title}</title>
  <link rel="stylesheet" href="/styles/design-tokens.css">
  <link rel="stylesheet" href="/styles/animations.css">
</head>
<body class="bg-black text-white antialiased">
  <Navigation />
  <main>
    <slot />
  </main>
  <Footer />
</body>
</html>
```

## Styling
- Background: Pure black (#000000)
- Text: Off-white (#E2E0DF)
- Smooth scroll enabled
- Font rendering optimized with antialiased

# Create About Section

Create about section with text superposition effect and parallax background.

## Task
Create `src/components/About.astro` with story content and sophisticated visual effects.

## Design Specifications
**Layout:**
- Two-column on desktop: Text (left) + Image (right)
- Single column on mobile: Text stacked above image
- Large section title: "PRIVATESTUDIO" (100px desktop)
- Subtitle: "Tu barbería en Barcelona"
- Body copy: 2-3 paragraphs about the shop

## Text Superposition Effect
Based on image reference:
- Large background text (very large, low opacity)
- Foreground content overlays
- Creates depth and visual interest
- Background text: "BARBERSHOP" at 200px, 5% opacity

## Content Structure
```
Section Title: PRIVATESTUDIO
Subtitle: Tu barbería en Barcelona

Body:
"En el corazón de Barcelona, PrivateStudio es más que una barbería.
Es un espacio donde la tradición se encuentra con la modernidad,
donde cada corte es una obra de arte.

Nuestro equipo de profesionales está dedicado a brindarte
la mejor experiencia en cuidado personal masculino."

[Image or visual element]
```

## Features
- Parallax effect on background image (subtle)
- Text animation on scroll (fade in + slide)
- High contrast for readability
- Responsive images with srcset
- Intersection Observer for scroll triggers

## Styling
- Section background: #2F2F2F or gradient
- Section padding: 8rem vertical
- Text container: Max-width 600px
- Title: Font-size 100px, line-height 0.9
- Body: Font-size 20px, line-height 1.6
- Text color: #E2E0DF

## Parallax Implementation
```javascript
// Use GSAP ScrollTrigger for parallax
gsap.to('.about-bg', {
  y: '20%',
  ease: 'none',
  scrollTrigger: {
    trigger: '.about-section',
    start: 'top bottom',
    end: 'bottom top',
    scrub: true
  }
});
```

## Responsive
- Desktop (1024px+): Two columns, 50/50 split
- Tablet (768-1023px): Two columns, 60/40 text/image
- Mobile (<768px): Single column, text first

# Create Team Section

Create team section showcasing 4 barbers with individual Booksy booking links.

## Task
Create `src/components/Team.astro` displaying 4 barber profiles with placeholder images and booking integration.

## Barber Data Structure
```javascript
const barbers = [
  {
    name: 'Barber Name 1',
    specialty: 'Classic Cuts & Fades',
    image: '/images/placeholders/barber-placeholder-1.jpg',
    booksy: 'https://booksy.com/privatestudio/barber1'
  },
  // ... 4 total barbers
];
```

## Features
- Section headline: "TEAM" (large typography, 120px desktop)
- Grid layout: 2x2 on desktop, 1 column on mobile
- Each card includes:
  - Square photo (400x400px placeholder)
  - Name (bold, 32px)
  - Specialty (italic, 18px)
  - "Reservar" button linking to Booksy
- Hover effects:
  - Image scale (1.05)
  - Card shadow increase
  - Button color change
- Staggered entrance animation

## Card Layout
```
[Photo - Square]
[Name]
[Specialty]
[Reservar Button]
```

## Styling
- Card background: #2F2F2F
- Card border: 1px solid rgba(255,255,255,0.1)
- Card padding: 2rem
- Card border-radius: 8px
- Gap between cards: 2rem
- Button: #968C8C background, white text
- Hover: Scale button 1.05, shadow glow

## Responsive
- Desktop (1024px+): 2x2 grid
- Tablet (768-1023px): 2 columns
- Mobile (<768px): 1 column, full width

## Animation
Use Intersection Observer for scroll-triggered entrance:
- Fade in + slide up
- Stagger delay: 0.15s between cards
- Trigger: 20% visible

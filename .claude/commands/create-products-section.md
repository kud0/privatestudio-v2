# Create Products Section

Create STMNT products showcase section inspired by Barber Barcelona.

## Task
Create `src/components/Products.astro` displaying STMNT Grooming Goods product lineup.

## Design Specifications
**Section Layout:**
- Section headline: "STMNT" (100px desktop, 48px mobile)
- Subheadline: "GROOMING GOODS" (36px)
- Intro text: "Lo mejor para tu cuidado"
- Product grid below

## Product Grid
**Layout:**
- Desktop: 3 columns
- Tablet: 2 columns
- Mobile: 1 column
- Gap: 2rem

**Product Card Structure:**
```
[Product Image - Square 300x300px]
[Product Name]
[Brief Description]
[Price (if applicable)]
```

## Sample Products (Placeholders)
```javascript
const products = [
  {
    name: 'Pomade',
    description: 'Hold firme con acabado mate',
    image: '/images/placeholders/product-placeholder-1.jpg',
    price: '€18.00'
  },
  {
    name: 'Hair Clay',
    description: 'Textura natural y volumen',
    image: '/images/placeholders/product-placeholder-2.jpg',
    price: '€16.00'
  },
  // ... 6 total products
];
```

## Features
- Hover effect: Image zoom 1.1, smooth transition
- Product card: Semi-transparent background
- Product info: Overlay or below image
- Typography: Clean, modern sans-serif
- CTA: "Ver Productos" button at bottom (optional)

## Styling
- Section background: #000000
- Section padding: 6rem vertical
- Card background: #2F2F2F
- Card border-radius: 4px
- Image: Object-fit cover, grayscale filter with color on hover
- Text color: #E2E0DF
- Price: Bronze accent #968C8C

## Animation
- Staggered fade in on scroll
- Hover: Scale image, lift card with shadow
- Smooth transitions: 0.3s ease

## Responsive
- Mobile: Stack vertically, full width
- Tablet: 2 columns
- Desktop: 3 columns, max-width 1400px container

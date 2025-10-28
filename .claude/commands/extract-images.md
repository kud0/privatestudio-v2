# Extract Images

Download and organize placeholder images for the website.

## Task
Create placeholder image system that can be easily replaced with real photos later.

## Required Placeholder Images

### Barber Photos (4 total)
- `barber-placeholder-1.jpg` - 400x400px, square
- `barber-placeholder-2.jpg` - 400x400px, square
- `barber-placeholder-3.jpg` - 400x400px, square
- `barber-placeholder-4.jpg` - 400x400px, square

### Product Photos (6 total)
- `product-placeholder-1.jpg` - 300x300px, square
- `product-placeholder-2.jpg` - 300x300px, square
- `product-placeholder-3.jpg` - 300x300px, square
- `product-placeholder-4.jpg` - 300x300px, square
- `product-placeholder-5.jpg` - 300x300px, square
- `product-placeholder-6.jpg` - 300x300px, square

### Background Images
- `hero-background.jpg` - 1920x1080px, landscape
- `about-background.jpg` - 1920x1080px, landscape

## Directory Structure
```
public/
└── images/
    └── placeholders/
        ├── barber-placeholder-1.jpg
        ├── barber-placeholder-2.jpg
        ├── barber-placeholder-3.jpg
        ├── barber-placeholder-4.jpg
        ├── product-placeholder-1.jpg
        ├── product-placeholder-2.jpg
        ├── product-placeholder-3.jpg
        ├── product-placeholder-4.jpg
        ├── product-placeholder-5.jpg
        ├── product-placeholder-6.jpg
        ├── hero-background.jpg
        ├── about-background.jpg
        └── README.md
```

## Implementation Options

**Option 1: Use Unsplash API**
Download high-quality barbershop-related images from Unsplash using their API or direct URLs.

**Option 2: Use Placeholder Services**
- https://picsum.photos/ for generic placeholders
- https://placehold.co/ for colored placeholders

**Option 3: Create Solid Color Placeholders**
Generate simple colored rectangles with text indicating what goes there.

## Create README for Replacements
Create `public/images/placeholders/README.md` with instructions:

```markdown
# Image Replacement Guide

Replace these placeholder images with real PrivateStudio photos.

## Barber Photos
- Size: 400x400px (square)
- Format: JPG or WebP
- File size: < 150KB each
- Name format: barber-[name].jpg

## Product Photos
- Size: 300x300px (square)
- Format: JPG or WebP
- File size: < 100KB each
- Clean background preferred

## Background Images
- Size: 1920x1080px minimum
- Format: JPG or WebP
- File size: < 500KB
- High quality, dark aesthetic

## Optimization
After replacing, run:
npm run optimize-images
```

## Image Optimization
Install sharp for optimization:
```bash
npm install sharp
```

Create optimization script if needed.

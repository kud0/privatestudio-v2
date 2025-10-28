# Optimize Images

Compress and optimize images for web performance.

## Task
Reduce image file sizes without visible quality loss.

## Target Specifications
- **Barber photos:** 400x400px, < 150KB each
- **Product photos:** 300x300px, < 100KB each
- **Background images:** 1920x1080px, < 500KB
- **Format:** WebP (preferred) or JPG
- **Quality:** 80-85%

## Instructions

### Manual Optimization
1. Identify large images (> target size)
2. Resize to exact dimensions needed
3. Convert to WebP format
4. Compress to 80-85% quality
5. Replace original files
6. Test image quality on site

### Automated Optimization
```bash
# Install sharp
npm install sharp

# Create optimization script
node scripts/optimize-images.js
```

### Using Online Tools
- https://squoosh.app/ (Google's image optimizer)
- https://tinypng.com/ (PNG/JPG compression)

## Before/After Metrics
Report:
- Original total size
- Optimized total size
- % reduction
- Lighthouse performance score improvement

## Preserve
- Image aspect ratios
- Visual quality (no visible artifacts)
- Alt text and accessibility

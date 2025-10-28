# Image Replacement Guide

This folder contains placeholder images that should be replaced with actual PrivateStudio photos before deployment.

## 📋 Image Inventory

### Barber Photos (4 images)
- `barber-placeholder-1.jpg` - Replace with Carlos Rodríguez photo
- `barber-placeholder-2.jpg` - Replace with Marc Fernández photo
- `barber-placeholder-3.jpg` - Replace with David López photo
- `barber-placeholder-4.jpg` - Replace with Alex Martínez photo

**Specifications:**
- Size: 400x400px (square, 1:1 aspect ratio)
- Format: JPG or WebP recommended
- File size: < 150KB each for optimal loading
- Style: Professional headshots, consistent lighting
- Background: Clean, preferably blurred or solid dark

### Product Photos (6 images)
- `product-placeholder-1.jpg` - Replace with STMNT Pomade
- `product-placeholder-2.jpg` - Replace with STMNT Hair Clay
- `product-placeholder-3.jpg` - Replace with STMNT Fiber Wax
- `product-placeholder-4.jpg` - Replace with STMNT Grooming Spray
- `product-placeholder-5.jpg` - Replace with STMNT Beard Oil
- `product-placeholder-6.jpg` - Replace with STMNT Styling Cream

**Specifications:**
- Size: 300x300px (square, 1:1 aspect ratio)
- Format: JPG or WebP recommended
- File size: < 100KB each
- Style: Clean product shots with white or dark background
- Lighting: Even, professional product photography

### Background Images (2 images)
- `hero-background.jpg` - Main hero section background
- `about-background.jpg` - About section image (interior/atmosphere shot)

**Specifications:**
- Size: 1920x1080px minimum (landscape, 16:9 aspect ratio)
- Format: JPG or WebP recommended
- File size: < 500KB each (optimize for web)
- Style: Dark, atmospheric, high contrast
- Content: Barbershop interior, tools, or ambiance

## 🔄 How to Replace Images

### Step 1: Prepare Your Images
1. Crop to correct aspect ratios (use tools like Photoshop, GIMP, or online editors)
2. Resize to specified dimensions
3. Optimize for web (compress without losing quality)

### Step 2: Name Your Files
Keep the same filenames or update references in components:
- Barber photos: Referenced in `src/components/Team.astro`
- Product photos: Referenced in `src/components/Products.astro`
- Backgrounds: Referenced in `src/components/Hero.astro` and `src/components/About.astro`

### Step 3: Replace Files
Simply drag and drop your new images into this folder, overwriting the placeholders.

### Step 4: Verify
1. Run `npm run dev` to start development server
2. Check each section to ensure images display correctly
3. Test on mobile and desktop viewports
4. Verify loading performance

## 🎨 Image Optimization Tips

### Manual Optimization
Use online tools:
- [TinyPNG](https://tinypng.com/) - PNG/JPG compression
- [Squoosh](https://squoosh.app/) - Advanced image optimization
- [Compressor.io](https://compressor.io/) - Multi-format compression

### Automated Optimization (Optional)
Install sharp for batch optimization:

```bash
npm install --save-dev sharp
```

Create an optimization script in `scripts/optimize-images.js`:

```javascript
const sharp = require('sharp');
const fs = require('fs');
const path = require('path');

const inputDir = './public/images/placeholders';
const outputDir = './public/images/optimized';

// Ensure output directory exists
if (!fs.existsSync(outputDir)){
    fs.mkdirSync(outputDir, { recursive: true });
}

// Process all images
fs.readdirSync(inputDir).forEach(file => {
  if (file.match(/\.(jpg|jpeg|png)$/i)) {
    sharp(path.join(inputDir, file))
      .resize(null, null, { withoutEnlargement: true })
      .jpeg({ quality: 85, progressive: true })
      .toFile(path.join(outputDir, file))
      .then(() => console.log(`Optimized: ${file}`))
      .catch(err => console.error(`Error: ${file}`, err));
  }
});
```

Run with: `node scripts/optimize-images.js`

## 📐 Aspect Ratio Reference

- **Square (1:1)**: Barber photos, product photos
- **Landscape (16:9)**: Background images
- **Portrait (4:5)**: Alternative for barber photos if needed

## ✅ Quality Checklist

Before replacing images, ensure:
- [ ] Images are properly cropped to aspect ratio
- [ ] Resolution meets minimum requirements
- [ ] File sizes are optimized (not too large)
- [ ] Images have consistent style and lighting
- [ ] Colors match the dark theme aesthetic (#000000, #2F2F2F)
- [ ] No watermarks or copyright issues
- [ ] Images are sharp and in focus

## 🎯 Brand Guidelines

For consistent visual identity:
- **Color Palette**: Black (#000000), Dark Gray (#2F2F2F), Bronze (#968C8C)
- **Photography Style**: Dark, moody, high contrast, professional
- **Barber Photos**: Confident poses, direct eye contact, dark backgrounds
- **Product Photos**: Clean, centered, well-lit, minimal shadows
- **Atmosphere Shots**: Authentic barbershop environment, tools visible

## 💡 Where to Get Images

### Professional Photography (Recommended)
- Hire a local photographer for authentic PrivateStudio photos
- Budget: €300-€500 for a 2-3 hour session

### Stock Photography (Temporary)
- [Unsplash](https://unsplash.com/s/photos/barbershop) - Free high-quality images
- [Pexels](https://www.pexels.com/search/barber/) - Free stock photos
- [Freepik](https://www.freepik.com/) - Free and premium options

### DIY Photography Tips
- Use natural lighting or professional lights
- Dark background for contrast
- High-resolution camera or modern smartphone
- Consistent angles and framing
- Edit for consistency (brightness, contrast, saturation)

## 🚀 After Replacing Images

1. **Test Thoroughly**
   - Check all pages: Hero, Team, Products, About
   - Test responsive layouts (mobile, tablet, desktop)
   - Verify image loading speed

2. **Optimize Performance**
   - Run Lighthouse audit
   - Ensure images are properly compressed
   - Consider lazy loading for below-fold images

3. **Update Alt Text**
   - Edit component files to update alt text with real names
   - Improve SEO and accessibility

4. **Deploy**
   - Commit changes to git
   - Deploy to production
   - Verify on live site

## 📞 Need Help?

If you encounter issues replacing images:
1. Check file paths in component files
2. Verify image dimensions and file sizes
3. Clear browser cache and rebuild: `npm run build`
4. Consult the implementation plan in `/docs/IMPLEMENTATION_PLAN.md`

---

**Last Updated:** October 28, 2024
**Status:** Placeholders Active - Ready for Replacement

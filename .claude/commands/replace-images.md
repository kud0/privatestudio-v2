# Replace Images

Swap placeholder images with real photos.

## Task
Replace placeholder images in `public/images/placeholders/` with actual photos.

## What to Replace
- Barber photos (4 images, 400x400px)
- Product photos (6 images, 300x300px)
- Background images (hero, about)

## Instructions
1. Verify new images exist and are properly sized
2. Check image format (WebP preferred, JPG acceptable)
3. Verify file sizes (< 150KB for portraits, < 500KB for backgrounds)
4. Update image paths in components if filenames changed
5. Test images load correctly
6. Verify alt text is descriptive

## Optimization
If images are too large:
- Resize to required dimensions
- Convert to WebP format
- Compress to 80-85% quality
- Use lazy loading for below-fold images

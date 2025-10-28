# Fix Responsive Issue

Diagnose and fix mobile/tablet layout problems.

## Task
Identify and fix responsive design issues at specific breakpoints.

## Common Issues
- Text overflow on mobile
- Images not scaling properly
- Broken grid layouts
- Horizontal scroll appearing
- Touch targets too small
- Overlapping elements

## Instructions
1. Ask user which component/section has the issue
2. Ask which breakpoint (mobile 320-767px, tablet 768-1023px)
3. Read the problematic component
4. Identify the CSS causing the issue
5. Apply fixes:
   - Add media queries if missing
   - Adjust font sizes for mobile
   - Fix grid columns (1 col mobile, 2 tablet, 3+ desktop)
   - Ensure images have max-width: 100%
   - Check padding/margins don't cause overflow
6. Test at all breakpoints (320px, 375px, 768px, 1024px, 1440px)

## Common Fixes
- Font too large: Scale down in media query
- Horizontal scroll: Add `overflow-x: hidden` to container
- Grid breaking: Change grid-template-columns in media query
- Images overflowing: Add `max-width: 100%; height: auto;`

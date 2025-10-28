# Create Horizontal Scroll Component

Create horizontal scrolling text effect on vertical scroll, inspired by AMIRO.

## Task
Create `src/components/HorizontalScroll.astro` that implements horizontal text scroll triggered by vertical page scroll using GSAP ScrollTrigger.

## Technical Implementation
Use GSAP ScrollTrigger to:
1. Detect vertical scroll position
2. Translate text horizontally based on scroll progress
3. Create infinite marquee effect
4. Performance optimized (60fps)

## Features
- Repeating text: "WE DON'T DO NORMAL • WE DON'T DO NORMAL • "
- Bidirectional: Scroll down = text moves left, scroll up = text moves right
- Large typography: 80px desktop, 48px mobile
- Infinite loop effect
- Smooth easing
- Performance: Use will-change, transform3d

## GSAP ScrollTrigger Setup
```javascript
gsap.to('.horizontal-text', {
  x: '-50%',
  ease: 'none',
  scrollTrigger: {
    trigger: '.horizontal-scroll-section',
    start: 'top bottom',
    end: 'bottom top',
    scrub: 1
  }
});
```

## Styling
- Section height: 200px
- Background: #000000
- Text color: #E2E0DF with 50% opacity
- Font weight: 700
- Overflow hidden
- White-space: nowrap

## Performance Optimization
- Use transform3d for GPU acceleration
- will-change: transform
- Limit to 60fps with throttling
- Disable on mobile if performance issues

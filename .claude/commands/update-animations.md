# Update Animations

Modify animation speeds, easings, or effects.

## Task
Update animation settings in `public/styles/animations.css` or component-level GSAP code.

## What Can Be Changed
- Animation duration (faster/slower)
- Easing functions (ease, ease-in-out, cubic-bezier)
- Stagger delays
- Scroll trigger settings
- Hover transition speeds

## Instructions
1. Identify which animation to modify
2. For CSS: Edit animations.css
3. For GSAP: Edit component script sections
4. Test animations run at 60fps
5. Ensure animations don't feel too fast or sluggish

## Performance
- Keep durations between 0.15s - 0.8s
- Use transform/opacity for GPU acceleration
- Avoid animating width/height/top/left

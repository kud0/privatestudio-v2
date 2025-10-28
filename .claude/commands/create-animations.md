# Create Animations

Create reusable CSS animations and transition utilities.

## Task
Create `public/styles/animations.css` with all animation keyframes and utility classes.

## CSS Animations

```css
/* ============================================
   KEYFRAME ANIMATIONS
   ============================================ */

@keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}

@keyframes fadeInUp {
  from {
    opacity: 0;
    transform: translateY(30px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@keyframes fadeInDown {
  from {
    opacity: 0;
    transform: translateY(-30px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@keyframes slideInLeft {
  from {
    opacity: 0;
    transform: translateX(-100px);
  }
  to {
    opacity: 1;
    transform: translateX(0);
  }
}

@keyframes slideInRight {
  from {
    opacity: 0;
    transform: translateX(100px);
  }
  to {
    opacity: 1;
    transform: translateX(0);
  }
}

@keyframes scaleIn {
  from {
    opacity: 0;
    transform: scale(0.9);
  }
  to {
    opacity: 1;
    transform: scale(1);
  }
}

@keyframes pulse {
  0%, 100% {
    opacity: 1;
  }
  50% {
    opacity: 0.5;
  }
}

@keyframes bounce {
  0%, 100% {
    transform: translateY(0);
  }
  50% {
    transform: translateY(-10px);
  }
}

@keyframes rotate {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(360deg);
  }
}

/* ============================================
   ANIMATION UTILITY CLASSES
   ============================================ */

.animate-fade-in {
  animation: fadeIn 0.8s ease forwards;
}

.animate-fade-in-up {
  animation: fadeInUp 0.8s ease forwards;
}

.animate-fade-in-down {
  animation: fadeInDown 0.8s ease forwards;
}

.animate-slide-in-left {
  animation: slideInLeft 0.8s ease forwards;
}

.animate-slide-in-right {
  animation: slideInRight 0.8s ease forwards;
}

.animate-scale-in {
  animation: scaleIn 0.5s ease forwards;
}

.animate-pulse {
  animation: pulse 2s ease infinite;
}

.animate-bounce {
  animation: bounce 1s ease infinite;
}

/* ============================================
   STAGGER DELAYS
   ============================================ */

.stagger-delay-1 { animation-delay: 0.1s; }
.stagger-delay-2 { animation-delay: 0.2s; }
.stagger-delay-3 { animation-delay: 0.3s; }
.stagger-delay-4 { animation-delay: 0.4s; }
.stagger-delay-5 { animation-delay: 0.5s; }
.stagger-delay-6 { animation-delay: 0.6s; }

/* ============================================
   HOVER TRANSITIONS
   ============================================ */

.hover-scale {
  transition: transform var(--transition-base);
}

.hover-scale:hover {
  transform: scale(1.05);
}

.hover-scale-sm {
  transition: transform var(--transition-base);
}

.hover-scale-sm:hover {
  transform: scale(1.02);
}

.hover-lift {
  transition: transform var(--transition-base), box-shadow var(--transition-base);
}

.hover-lift:hover {
  transform: translateY(-4px);
  box-shadow: var(--shadow-xl);
}

.hover-glow {
  transition: box-shadow var(--transition-base);
}

.hover-glow:hover {
  box-shadow: var(--shadow-glow);
}

.hover-brightness {
  transition: filter var(--transition-base);
}

.hover-brightness:hover {
  filter: brightness(1.1);
}

.hover-grayscale {
  filter: grayscale(100%);
  transition: filter var(--transition-base);
}

.hover-grayscale:hover {
  filter: grayscale(0%);
}

/* ============================================
   SCROLL ANIMATIONS (WITH INTERSECTION OBSERVER)
   ============================================ */

.scroll-reveal {
  opacity: 0;
  transform: translateY(30px);
  transition: opacity 0.8s ease, transform 0.8s ease;
}

.scroll-reveal.is-visible {
  opacity: 1;
  transform: translateY(0);
}

.scroll-reveal-left {
  opacity: 0;
  transform: translateX(-50px);
  transition: opacity 0.8s ease, transform 0.8s ease;
}

.scroll-reveal-left.is-visible {
  opacity: 1;
  transform: translateX(0);
}

.scroll-reveal-right {
  opacity: 0;
  transform: translateX(50px);
  transition: opacity 0.8s ease, transform 0.8s ease;
}

.scroll-reveal-right.is-visible {
  opacity: 1;
  transform: translateX(0);
}

.scroll-reveal-scale {
  opacity: 0;
  transform: scale(0.9);
  transition: opacity 0.8s ease, transform 0.8s ease;
}

.scroll-reveal-scale.is-visible {
  opacity: 1;
  transform: scale(1);
}

/* ============================================
   PERFORMANCE OPTIMIZATIONS
   ============================================ */

.will-animate {
  will-change: transform, opacity;
}

.gpu-accelerated {
  transform: translateZ(0);
  backface-visibility: hidden;
}

/* ============================================
   SMOOTH SCROLL
   ============================================ */

html {
  scroll-behavior: smooth;
}

@media (prefers-reduced-motion: reduce) {
  *,
  *::before,
  *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
    scroll-behavior: auto !important;
  }
}

/* ============================================
   UTILITY: LOADING STATES
   ============================================ */

.loading {
  position: relative;
  pointer-events: none;
}

.loading::after {
  content: '';
  position: absolute;
  top: 50%;
  left: 50%;
  width: 20px;
  height: 20px;
  margin: -10px 0 0 -10px;
  border: 2px solid var(--color-accent-bronze);
  border-radius: 50%;
  border-top-color: transparent;
  animation: rotate 0.6s linear infinite;
}
```

## Intersection Observer Script
Add this to components that need scroll-triggered animations:

```javascript
// Add to component script
const observerOptions = {
  threshold: 0.2,
  rootMargin: '0px 0px -100px 0px'
};

const observer = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      entry.target.classList.add('is-visible');
    }
  });
}, observerOptions);

document.querySelectorAll('.scroll-reveal, .scroll-reveal-left, .scroll-reveal-right, .scroll-reveal-scale')
  .forEach(el => observer.observe(el));
```

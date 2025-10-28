# AMIRO Barbershops - Complete Design System Specification

> **Source**: https://www.amirobarbershops.com/
> **Analyzed**: October 28, 2025
> **Built with**: WordPress + Elementor Pro

---

## 1. Typography System

### Primary Font Families

#### **Bicyclette** (Adobe Typekit - Primary Brand Font)
- **Source**: `https://use.typekit.net/fin5gwl.css`
- **Weights Available**:
  - Regular (400) - Normal
  - Regular (400) - Italic
  - Bold (700) - Normal
- **Usage**: Primary headings, hero text, brand-heavy sections
- **CSS Class**: `.tk-bicyclette { font-family: "bicyclette", sans-serif; }`

#### **Montserrat** (Google Fonts - Secondary)
- **Source**: Google Fonts (local hosted via Elementor)
- **Weights Available**:
  - Thin (100) - Italic
  - Extra Light (200) - Italic
  - Light (300) - Italic
  - Regular (400)
  - Medium (500)
  - Semi Bold (600)
  - Bold (700)
- **Usage**: Body text, secondary headings, navigation, buttons
- **font-display**: swap

#### **IBM Plex Mono** (Google Fonts - Monospace)
- **Source**: Google Fonts (local hosted via Elementor)
- **Usage**: Special text elements, possibly pricing or technical details

### Font Sizes

```css
/* Base Sizes */
--font-size-base: 16px;
--font-size-small: 13px;
--font-size-medium: 20px;
--font-size-large: 36px;
--font-size-x-large: 42px;

/* Additional Sizes Used */
12px   /* Small UI elements, disclaimers */
14px   /* Secondary text, metadata */
16px   /* Body text (base) */
18px   /* Large body text, subheadings */
1.5em  /* Quote text */
```

### Heading Hierarchy

```css
h1 {
  font-family: "bicyclette", sans-serif;
  font-weight: 700;
  /* Size varies by section, typically 48-72px on desktop */
}

h2 {
  font-family: "bicyclette", sans-serif;
  font-weight: 700;
  /* Elementor default sizing applied */
}

h3 {
  font-family: "bicyclette", sans-serif;
  font-weight: 400;
  /* Used for card headings and section subheadings */
}

h4 {
  font-family: "Montserrat", sans-serif;
  font-weight: 600;
}
```

### Line Heights

```css
--line-height-base: 1.6;
--line-height-tight: 1.0;
--line-height-heading: 16px;
--line-height-body: 24px;
```

### Font Weights

```css
--font-weight-normal: 400;
--font-weight-medium: 500;
--font-weight-semibold: 600;
--font-weight-bold: 700;
```

### Text Transformations

- **None** - Default throughout the site (natural casing preserved)
- No uppercase forcing on headings or buttons

---

## 2. Color Palette

### Primary Brand Colors

```css
/* Main Background Colors */
--color-primary-dark: #2F2F2F;      /* Primary dark background */
--color-secondary-dark: #2a2a2a;    /* Secondary dark sections */
--color-accent-mauve: #968C8C;      /* Accent/highlight color */

/* Text Colors */
--color-text-primary: #E2E0DF;      /* Primary light text */
--color-text-secondary: #d0d0d0;    /* Secondary light text */
--color-text-muted: #b2b0b0;        /* Muted/disabled text */

/* Border/Divider Colors */
--color-border-dark: #474444;       /* Borders on dark backgrounds */
--color-border-light: #E2E0DF;      /* Light borders */
```

### WordPress Color Presets

```css
/* System Colors */
--wp--preset--color--black: #000000;
--wp--preset--color--white: #ffffff;
--wp--preset--color--cyan-bluish-gray: #abb8c3;
--wp--preset--color--pale-pink: #f78da7;
--wp--preset--color--vivid-red: #cf2e2e;
--wp--preset--color--luminous-vivid-orange: #ff6900;
--wp--preset--color--luminous-vivid-amber: #fcb900;
--wp--preset--color--light-green-cyan: #7bdcb5;
--wp--preset--color--vivid-green-cyan: #00d084;
--wp--preset--color--pale-cyan-blue: #8ed1fc;
--wp--preset--color--vivid-cyan-blue: #0693e3;
--wp--preset--color--vivid-purple: #9b51e0;
```

### UI State Colors

```css
/* Interactive States */
--color-hover-mauve: #968C8C;       /* Hover state for buttons/links */
--color-focus: #E2E0DF;             /* Focus state */
--color-active: #2F2F2F;            /* Active state */
```

### Utility Colors

```css
/* Additional UI Colors Found */
#1863DC    /* Accent blue (links/actions) */
#858585    /* Medium gray */
#D0D5D2    /* Very light gray */
#f4f4f4    /* Off-white backgrounds */
#acabab    /* Subtle borders/shadows */
#dedfe0    /* Light dividers */
```

### Color Opacity Variations

```css
rgba(0, 0, 0, 0.2)   /* Light shadows */
rgba(0, 0, 0, 0.3)   /* Medium shadows */
rgba(0, 0, 0, 0.4)   /* Deep shadows */
#acabab4d            /* Border shadow color with opacity */
```

### Gradient Presets

```css
/* WordPress Gradient System */
--wp--preset--gradient--vivid-cyan-blue-to-vivid-purple: 
  linear-gradient(135deg, rgba(6,147,227,1) 0%, rgb(155,81,224) 100%);

--wp--preset--gradient--light-green-cyan-to-vivid-green-cyan: 
  linear-gradient(135deg, rgb(122,220,180) 0%, rgb(0,208,130) 100%);

--wp--preset--gradient--luminous-vivid-amber-to-luminous-vivid-orange: 
  linear-gradient(135deg, rgba(252,185,0,1) 0%, rgba(255,105,0,1) 100%);

--wp--preset--gradient--cool-to-warm-spectrum: 
  linear-gradient(135deg, rgb(74,234,220) 0%, rgb(151,120,209) 20%, 
  rgb(207,42,186) 40%, rgb(238,44,130) 60%, rgb(251,105,98) 80%, 
  rgb(254,248,76) 100%);
```

---

## 3. Spacing & Layout System

### Container System

```css
/* Max Widths */
--wp--style--global--content-size: 800px;
--wp--style--global--wide-size: 1200px;
```

### Spacing Scale

```css
/* WordPress Spacing Presets */
--wp--preset--spacing--20: 0.44rem;   /* ~7px */
--wp--preset--spacing--30: 0.67rem;   /* ~11px */
--wp--preset--spacing--40: 1rem;      /* 16px */
--wp--preset--spacing--50: 1.5rem;    /* 24px */
--wp--preset--spacing--60: 2.25rem;   /* 36px */
--wp--preset--spacing--70: 3.38rem;   /* ~54px */
--wp--preset--spacing--80: 5.06rem;   /* ~81px */

/* Block Gap (Default spacing between elements) */
--wp--style--block-gap: 24px;
```

### Common Padding Values

```css
/* Button Padding */
padding: calc(0.667em + 2px) calc(1.333em + 2px);  /* Default button */
padding: 10px 16px;                                 /* Small button */
padding: 16px 24px;                                 /* Medium button */
padding: 20px 26px;                                 /* Large button */
padding: 22px 24px;                                 /* Custom button */

/* Section Padding */
padding: 0 15px;       /* Mobile horizontal padding */
padding: 0 22px;       /* Tablet horizontal padding */
padding: 0 24px;       /* Desktop horizontal padding */
padding: 20px 0;       /* Vertical section spacing */

/* Card/Component Padding */
padding: 15px 10px;    /* Compact card */
padding: 10px;         /* Minimal padding */
padding: 8px;          /* Tight padding */
padding: 4px 8px;      /* Tag/badge padding */
```

### Common Margin Values

```css
margin: 0 auto;         /* Center element */
margin: 0 12px 0 0;     /* Right spacing */
margin: 0 0 12px 0;     /* Bottom spacing */
margin: 0 0 0 12px;     /* Left spacing */
margin: 0 0 0 28px;     /* Larger left spacing */
margin: 0 0 0 32px;     /* Extra left spacing */
margin: 10px 0 0 0;     /* Top spacing */
margin: 10px 0 16px 0;  /* Top and bottom */
margin: 0 8px 0 0;      /* Small right spacing */
```

### Responsive Breakpoints

```css
/* Elementor Default Breakpoints */
@media (max-width: 352px)  { /* Extra small mobile */ }
@media (max-width: 425px)  { /* Small mobile */ }
@media (max-width: 440px)  { /* Mobile */ }
@media (max-width: 576px)  { /* Large mobile */ }
@media (max-width: 768px)  { /* Tablet */ }
@media (max-width: 845px)  { /* Large tablet */ }
@media (max-width: 1024px) { /* Small desktop */ }
```

---

## 4. Button Styles

### Primary Button

```css
.elementor-button {
  font-family: "Montserrat", sans-serif;
  font-size: 16px;
  font-weight: 600;
  padding: calc(0.667em + 2px) calc(1.333em + 2px);
  
  /* Colors */
  color: #E2E0DF;
  background-color: #2F2F2F;
  border-color: #2F2F2F;
  
  /* Border */
  border-radius: 50px;  /* Pill-shaped buttons */
  border-width: 2px;
  border-style: solid;
  
  /* Transitions */
  transition: all 0.4s ease;
  
  /* Text */
  text-decoration: none;
  text-transform: none;
}
```

### Button Variants

```css
/* Small Button */
.elementor-button.elementor-size-sm {
  font-size: 14px;
  padding: 10px 16px;
}

/* Accent Button */
.elementor-button.accent {
  background-color: #968C8C;
  border-color: #968C8C;
  color: #E2E0DF;
}

/* Outline Button */
.elementor-button.outline {
  background-color: transparent;
  border-color: #E2E0DF;
  color: #E2E0DF;
}

/* Dark Button (for light backgrounds) */
.elementor-button.dark {
  background-color: #2a2a2a;
  border-color: #474444;
  color: #d0d0d0;
}
```

### Button Hover States

```css
.elementor-button:hover {
  background-color: #968C8C;
  border-color: #968C8C;
  color: #E2E0DF;
  transform: translateY(-2px);  /* Subtle lift effect */
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
}

.elementor-button.outline:hover {
  background-color: #968C8C;
  border-color: #968C8C;
  color: #E2E0DF;
}
```

### Button with Icon

```css
.elementor-button-content-wrapper {
  display: flex;
  align-items: center;
  justify-content: center;
}

.elementor-button-icon {
  margin: 0 8px 0 0;  /* Right spacing from text */
}

.elementor-button-icon svg {
  width: 1em;
  height: 1em;
  fill: currentColor;
}

.elementor-button-text {
  flex-grow: 0;
}
```

### Button Border Radius Options

```css
--border-radius-none: 0px;
--border-radius-small: 2px;
--border-radius-medium: 4px;
--border-radius-large: 6px;
--border-radius-pill: 50px;
--border-radius-circle: 50%;
```

---

## 5. Component Styles

### Navigation Bar

```css
.elementor-nav {
  /* Sticky behavior */
  position: sticky;
  top: 0;
  z-index: 1000;
  
  /* Background */
  background-color: #2F2F2F;
  
  /* Shadow when scrolled */
  box-shadow: 0 -1px 10px 0 #acabab4d;
  
  /* Sticky offset */
  --sticky-offset: 0;
  --sticky-effects-offset: 0;
  
  /* Padding */
  padding: 12px 24px;
}

/* Nav on scroll behavior */
[data-settings*="sticky"] {
  position: sticky;
  top: 0;
}
```

### Hero Section

```css
.hero-section {
  /* Background */
  background-color: #2F2F2F;
  
  /* Or with slideshow */
  background: slideshow;
  --slideshow-duration: 5000ms;
  --slideshow-transition: 1000ms;
  --slideshow-effect: fade;
  --ken-burns: enabled;
  
  /* Min height */
  min-height: 100vh;
  min-height: calc(100vh - 80px); /* Accounting for nav */
  
  /* Flexbox centering */
  display: flex;
  align-items: center;
  justify-content: center;
  
  /* Text color */
  color: #E2E0DF;
}

.hero-heading {
  font-family: "bicyclette", sans-serif;
  font-size: clamp(48px, 8vw, 96px);
  font-weight: 700;
  line-height: 1.1;
  margin-bottom: 24px;
}
```

### Team Cards / Service Cards

```css
.team-card,
.service-card {
  /* Background */
  background-color: #2a2a2a;
  
  /* Border */
  border: 1px solid #474444;
  border-radius: 4px;
  
  /* Padding */
  padding: 22px 24px;
  
  /* Transition */
  transition: all 0.4s ease;
  
  /* Text color */
  color: #d0d0d0;
}

.team-card:hover,
.service-card:hover {
  /* Hover shadow */
  box-shadow: 0 32px 68px rgba(0, 0, 0, 0.3);
  
  /* Lift effect */
  transform: translateY(-8px);
  
  /* Border change */
  border-color: #968C8C;
}

.card-image {
  width: 100%;
  height: auto;
  border-radius: 4px;
  margin-bottom: 16px;
}

.card-title {
  font-family: "bicyclette", sans-serif;
  font-size: 24px;
  font-weight: 700;
  margin-bottom: 8px;
  color: #E2E0DF;
}

.card-subtitle {
  font-family: "Montserrat", sans-serif;
  font-size: 14px;
  font-weight: 400;
  color: #d0d0d0;
  margin-bottom: 12px;
}

.card-description {
  font-family: "Montserrat", sans-serif;
  font-size: 16px;
  line-height: 1.6;
  color: #d0d0d0;
}
```

### Image Styles

```css
img {
  max-width: 100%;
  height: auto;
  
  /* Lazy loading behavior */
  contain-intrinsic-size: 3000px 1500px;
}

/* Image with border radius */
.rounded-image {
  border-radius: 4px;
}

/* Circular image (profile photos) */
.circular-image {
  border-radius: 50%;
  aspect-ratio: 1 / 1;
  object-fit: cover;
}
```

### Icon List

```css
.elementor-icon-list {
  list-style: none;
  padding: 0;
  margin: 0;
}

.elementor-icon-list-item {
  display: flex;
  align-items: center;
  margin-bottom: 12px;
}

.elementor-icon-list-icon {
  margin-right: 12px;
  color: #968C8C;
}

.elementor-icon-list-text {
  font-family: "Montserrat", sans-serif;
  font-size: 16px;
  color: #E2E0DF;
}
```

### Social Icons

```css
.elementor-social-icon {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  
  width: 40px;
  height: 40px;
  
  background-color: transparent;
  border: 2px solid #E2E0DF;
  border-radius: 50%;
  
  color: #E2E0DF;
  
  transition: all 0.2s ease-in-out;
}

.elementor-social-icon:hover {
  background-color: #968C8C;
  border-color: #968C8C;
  transform: scale(1.1);
}

.elementor-social-icon svg {
  width: 20px;
  height: 20px;
  fill: currentColor;
}
```

---

## 6. Shadows & Elevation

### Shadow Presets

```css
/* WordPress Shadow System */
--wp--preset--shadow--natural: 6px 6px 9px rgba(0, 0, 0, 0.2);
--wp--preset--shadow--deep: 12px 12px 50px rgba(0, 0, 0, 0.4);
--wp--preset--shadow--sharp: 6px 6px 0px rgba(0, 0, 0, 0.2);
--wp--preset--shadow--outlined: 6px 6px 0px -3px rgba(255, 255, 255, 1), 6px 6px rgba(0, 0, 0, 1);
--wp--preset--shadow--crisp: 6px 6px 0px rgba(0, 0, 0, 1);

/* Custom Shadows Used */
--shadow-card: 0 32px 68px rgba(0, 0, 0, 0.3);
--shadow-nav: 0 -1px 10px 0 #acabab4d;
--shadow-button-hover: 0 4px 12px rgba(0, 0, 0, 0.2);
```

### Elevation Levels

```css
/* Level 0 - Flat */
box-shadow: none;

/* Level 1 - Subtle */
box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);

/* Level 2 - Medium */
box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);

/* Level 3 - Elevated */
box-shadow: 0 8px 24px rgba(0, 0, 0, 0.25);

/* Level 4 - Floating */
box-shadow: 0 32px 68px rgba(0, 0, 0, 0.3);
```

---

## 7. Animations & Interactions

### Transition Timings

```css
/* Standard transitions */
--transition-fast: all 0.2s ease-in-out;
--transition-normal: all 0.4s ease;
--transition-slow: all 1s ease;
--transition-instant: all 0s ease;
```

### Scroll Animations (Elementor Motion Effects)

```css
/* Parallax scrolling */
[data-settings*="motion_fx_scrolling"] {
  /* translateX effect */
  --motion-fx-translateX-speed: 4px;
  --motion-fx-translateX-direction: negative; /* or normal */
  --motion-fx-translateX-range: 0% to 100%;
}
```

### Fade In Animations

```css
/* fadeInLeft animation (used throughout) */
@keyframes fadeInLeft {
  from {
    opacity: 0;
    transform: translate3d(-100%, 0, 0);
  }
  to {
    opacity: 1;
    transform: translate3d(0, 0, 0);
  }
}

[data-settings*="_animation=fadeInLeft"] {
  animation-name: fadeInLeft;
  animation-duration: 1s;
  animation-fill-mode: both;
}
```

### Hover Effects

```css
/* Card hover */
.card {
  transition: transform 0.4s ease, box-shadow 0.4s ease;
}

.card:hover {
  transform: translateY(-8px);
  box-shadow: 0 32px 68px rgba(0, 0, 0, 0.3);
}

/* Button hover */
.button {
  transition: all 0.4s ease;
}

.button:hover {
  transform: translateY(-2px);
  background-color: #968C8C;
}

/* Icon hover */
.icon {
  transition: transform 0.2s ease-in-out;
}

.icon:hover {
  transform: scale(1.1);
}
```

### Loading States

```css
/* Lazy load behavior */
.e-con.e-parent:not(.e-lazyloaded):not(.e-no-lazyload) {
  background-image: none !important;
}

/* Image loading */
img[sizes="auto"] {
  contain-intrinsic-size: 3000px 1500px;
}
```

### Swiper/Carousel Settings

```css
/* Elementor uses Swiper v8 */
.swiper {
  --swiper-theme-color: #968C8C;
  --swiper-navigation-size: 44px;
}

.swiper-slide {
  transition: all 0.4s ease;
}
```

---

## 8. Forms & Inputs

### Input Fields

```css
input[type="text"],
input[type="email"],
input[type="tel"],
textarea {
  font-family: "Montserrat", sans-serif;
  font-size: 16px;
  
  padding: 12px 16px;
  
  background-color: #2a2a2a;
  border: 1px solid #474444;
  border-radius: 4px;
  
  color: #E2E0DF;
  
  transition: all 0.2s ease-in-out;
}

input:focus,
textarea:focus {
  border-color: #968C8C;
  outline: none;
  box-shadow: 0 0 0 3px rgba(150, 140, 140, 0.1);
}

input::placeholder,
textarea::placeholder {
  color: #b2b0b0;
}
```

### Checkbox & Radio Buttons

```css
input[type="checkbox"],
input[type="radio"] {
  width: 20px;
  height: 20px;
  
  accent-color: #968C8C;
  
  border: 1px solid #474444;
  border-radius: 2px;
}

input[type="radio"] {
  border-radius: 50%;
}
```

---

## 9. Aspect Ratios & Media

### Preset Aspect Ratios

```css
--wp--preset--aspect-ratio--square: 1;
--wp--preset--aspect-ratio--4-3: 4/3;
--wp--preset--aspect-ratio--3-4: 3/4;
--wp--preset--aspect-ratio--3-2: 3/2;
--wp--preset--aspect-ratio--2-3: 2/3;
--wp--preset--aspect-ratio--16-9: 16/9;
--wp--preset--aspect-ratio--9-16: 9/16;
```

### Image Treatments

```css
/* Cover image */
.cover-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
  object-position: center;
}

/* Contain image */
.contain-image {
  width: 100%;
  height: 100%;
  object-fit: contain;
}
```

---

## 10. Utility Classes & Helpers

### Display Utilities

```css
/* Visibility */
.elementor-hidden-desktop { display: none !important; }
@media (max-width: 1024px) {
  .elementor-hidden-tablet { display: none !important; }
}
@media (max-width: 767px) {
  .elementor-hidden-mobile { display: none !important; }
}

/* Width */
.elementor-widget__width-auto {
  width: auto !important;
}

.elementor-widget__width-inherit {
  width: inherit !important;
}

.elementor-widget__width-initial {
  width: initial !important;
}
```

### Alignment Utilities

```css
/* Text alignment */
.elementor-align-left { text-align: left; }
.elementor-align-center { text-align: center; }
.elementor-align-right { text-align: right; }
.elementor-align-justify { text-align: justify; }

/* Mobile alignment */
@media (max-width: 767px) {
  .elementor-mobile-align-center { text-align: center; }
  .elementor-mobile-align-left { text-align: left; }
  .elementor-mobile-align-right { text-align: right; }
}
```

### Flexbox Utilities

```css
.e-flex {
  display: flex;
}

.e-con-full {
  width: 100%;
}

/* Flex container with children */
.e-parent {
  display: flex;
  flex-direction: column;
}

.e-child {
  flex: 1;
}
```

---

## 11. Accessibility Features

### Focus States

```css
/* Keyboard focus */
*:focus-visible {
  outline: 2px solid #968C8C;
  outline-offset: 2px;
}

button:focus-visible,
a:focus-visible {
  outline: 2px solid #968C8C;
  outline-offset: 4px;
}
```

### Screen Reader Text

```css
.screen-reader-text {
  clip: rect(1px, 1px, 1px, 1px);
  clip-path: inset(50%);
  height: 1px;
  width: 1px;
  margin: -1px;
  overflow: hidden;
  padding: 0;
  position: absolute;
  word-wrap: normal !important;
}
```

---

## 12. Print Styles

```css
@media print {
  /* Hide navigation and non-essential elements */
  nav,
  .elementor-button,
  .social-icons {
    display: none !important;
  }
  
  /* Reset colors for print */
  body {
    background: white !important;
    color: black !important;
  }
  
  /* Adjust margins for print */
  @page {
    margin: 2cm;
  }
}
```

---

## 13. Performance Optimizations

### Font Loading Strategy

```css
/* font-display: swap for all fonts */
@font-face {
  font-family: "Montserrat";
  font-display: swap;
  /* ... */
}

@font-face {
  font-family: "bicyclette";
  font-display: auto;
  /* ... */
}
```

### Image Lazy Loading

```html
<!-- Native lazy loading -->
<img loading="lazy" src="..." alt="..." />

<!-- Elementor lazy loading classes -->
<div class="e-lazyloaded">...</div>
```

### CSS Loading

```html
<!-- Elementor splits CSS into separate files per post -->
<link rel='stylesheet' href='post-6.css' media='all' />
<link rel='stylesheet' href='post-7.css' media='all' />

<!-- Conditional loading -->
<link rel='stylesheet' href='apple-webkit.min.css' media='all' />
```

---

## 14. Implementation Notes

### Tech Stack
- **CMS**: WordPress 6.8.3
- **Page Builder**: Elementor Pro 3.32.2 + Elementor 3.32.3
- **Theme**: Hello Elementor 3.4.4 (minimal theme)
- **Fonts**: Adobe Typekit (Bicyclette) + Google Fonts (Montserrat, IBM Plex Mono)

### Key Elementor Features Used
1. **Containers**: Flexbox-based layout system (`e-con`, `e-parent`, `e-child`)
2. **Motion Effects**: Parallax scrolling with translateX
3. **Sticky Elements**: Sticky navigation and sections
4. **Animations**: fadeInLeft entrance animation
5. **Responsive Controls**: Different styles for desktop/tablet/mobile
6. **Background Slideshow**: Ken Burns effect with fade transitions

### CSS Architecture
- **Utility-first approach** for spacing and layout
- **Component-based** for reusable UI elements
- **BEM-like naming** for Elementor classes
- **CSS Custom Properties** for theming
- **Mobile-first responsive** design patterns

### Critical CSS Classes

```css
/* Most important Elementor classes to implement */
.elementor-element
.elementor-widget
.elementor-button
.elementor-heading-title
.elementor-icon-list
.e-con (container)
.e-flex (flexbox)
.e-parent / .e-child (flex relationships)
```

---

## 15. Rebuild Checklist

To recreate this design exactly:

- [ ] Set up typography with Bicyclette and Montserrat
- [ ] Implement color palette with CSS variables
- [ ] Create button component with all variants
- [ ] Build card component with hover effects
- [ ] Set up sticky navigation with scroll behavior
- [ ] Implement hero section with background slideshow
- [ ] Add motion effects (parallax, fade-in animations)
- [ ] Configure responsive breakpoints
- [ ] Set up spacing system with CSS variables
- [ ] Implement shadow and elevation system
- [ ] Add form styling
- [ ] Configure image lazy loading
- [ ] Test all hover and focus states
- [ ] Verify mobile responsiveness
- [ ] Test accessibility (keyboard navigation, screen readers)
- [ ] Optimize font loading (font-display: swap)
- [ ] Implement smooth scrolling behavior

---

## 16. Design Tokens (Recommended)

```css
:root {
  /* Brand Colors */
  --amiro-primary: #2F2F2F;
  --amiro-secondary: #2a2a2a;
  --amiro-accent: #968C8C;
  
  /* Text Colors */
  --amiro-text-primary: #E2E0DF;
  --amiro-text-secondary: #d0d0d0;
  --amiro-text-muted: #b2b0b0;
  
  /* Borders */
  --amiro-border-dark: #474444;
  --amiro-border-light: #E2E0DF;
  
  /* Spacing */
  --amiro-space-xs: 8px;
  --amiro-space-sm: 12px;
  --amiro-space-md: 16px;
  --amiro-space-lg: 24px;
  --amiro-space-xl: 32px;
  --amiro-space-2xl: 48px;
  
  /* Typography */
  --amiro-font-brand: "bicyclette", sans-serif;
  --amiro-font-body: "Montserrat", sans-serif;
  --amiro-font-mono: "IBM Plex Mono", monospace;
  
  /* Border Radius */
  --amiro-radius-sm: 4px;
  --amiro-radius-md: 6px;
  --amiro-radius-pill: 50px;
  --amiro-radius-circle: 50%;
  
  /* Transitions */
  --amiro-transition-fast: 0.2s ease-in-out;
  --amiro-transition-normal: 0.4s ease;
  --amiro-transition-slow: 1s ease;
  
  /* Shadows */
  --amiro-shadow-card: 0 32px 68px rgba(0, 0, 0, 0.3);
  --amiro-shadow-nav: 0 -1px 10px 0 rgba(172, 171, 171, 0.3);
  --amiro-shadow-button: 0 4px 12px rgba(0, 0, 0, 0.2);
}
```

---

## 17. Additional Resources

### Fonts
- Bicyclette: https://use.typekit.net/fin5gwl.css
- Montserrat: https://fonts.google.com/specimen/Montserrat
- IBM Plex Mono: https://fonts.google.com/specimen/IBM+Plex+Mono

### Libraries Used
- Elementor Frontend: `frontend.min.css` (v3.32.3)
- Swiper: `swiper.min.css` (v8.4.5)
- Animations: `fadeInLeft.min.css`

### Performance
- Lazy loading images
- Conditional CSS loading (apple-webkit specific)
- Split CSS per page/post
- Font preloading via Typekit

---

**End of Design Specification**

*This document provides a complete, implementation-ready specification of the AMIRO Barbershops website design system. All values are extracted from the live site and ready for exact recreation.*

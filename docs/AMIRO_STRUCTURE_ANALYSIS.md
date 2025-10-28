# AMIRO BARBERSHOP - COMPLETE STRUCTURAL ANALYSIS
**Website:** https://www.amirobarbershops.com/  
**Analysis Date:** 2025-10-28  
**Platform:** WordPress + Elementor Pro

---

## TABLE OF CONTENTS
1. [Header/Navigation](#1-header--navigation)
2. [Hero Section](#2-hero-section)
3. [Scrolling Marquee](#3-scrolling-marquee-section)
4. [Services Introduction](#4-services-introduction-section)
5. [Service Items Grid](#5-service-items-grid)
6. [Team Section](#6-team-section)
7. [About Section](#7-about-section)
8. [Contact/Footer](#8-contact--footer-section)
9. [Grid Structure](#9-grid-structure--layout-system)
10. [Animations](#10-scrolling--animation-elements)
11. [Image Strategy](#11-image-placement-strategy)
12. [CTA Positioning](#12-call-to-action-cta-positioning)
13. [Complete Section List](#13-complete-section-list-top-to-bottom)

---

## 1. HEADER / NAVIGATION

### Structure
- **Position:** Sticky (stays on scroll)
- **Layout:** 3-column flex container
- **Background:** Solid color

### Desktop Layout (Left to Right)

#### Left Section (Hidden on Mobile)
- **Social Media Icons:**
  - Instagram: https://www.instagram.com/amirobarbershops/
  - TikTok: https://www.tiktok.com/@amiro.barbershops

#### Center Section
- **Logo:** Amiro icon (visible on all devices)

#### Right Section
- **Desktop:** Two CTA buttons
  - "call us" button
  - "Book now" button
- **Mobile:** Hamburger menu icon

### Responsive Behavior
- Social icons hidden on mobile
- CTA buttons hidden on mobile/tablet
- Logo remains centered on all devices

---

## 2. HERO SECTION

### Overall Structure
- **Height:** Full viewport (100vh)
- **Background:** Large background image
- **Container:** Full-width flex container

### Mobile Layout (Top to Bottom)
```
┌─────────────────────────────────┐
│  [Book now]     [call us]       │ ← Two CTA buttons (top)
│                                 │
│                                 │
│         book                    │ ← Main heading (centered)
│         now                     │
│                                 │
└─────────────────────────────────┘
```

**Elements:**
1. **Top CTAs (Mobile only):**
   - "Book now" button (left)
   - "call us" button (right)
   
2. **Main Heading (Centered):**
   - Text: "book<br>now"
   - Large, bold typography
   - Line break creates stacked effect

### Desktop Layout
```
┌───────────────────────────────────────┐
│                                       │
│                                       │
│             book now                  │ ← Centered heading
│                                       │
│                                       │
│              [Schedule your    ]      │ ← CTA bottom right
│              [ haircut today > ]      │
└───────────────────────────────────────┘
```

**Elements:**
1. **Main Heading:** Centered, massive scale
2. **CTA Button (Bottom Right):**
   - Text: "Schedule your haircut today >"
   - Class: `bottone-scritta` (brand button style)
   - Hidden on tablet/mobile

### Design Notes
- Hero uses line breaks (`<br>`) in heading for visual impact
- Mobile prioritizes immediate CTAs at top
- Desktop creates more breathing room with bottom-right CTA

---

## 3. SCROLLING MARQUEE SECTION

### Structure
- **Width:** Full viewport
- **Background:** Colored/patterned background
- **Animation:** Infinite horizontal scroll

### Animation Details
```javascript
// Elementor Motion Effects Settings:
{
  motion_fx_scrolling: true,
  motion_fx_translateX: true,
  motion_fx_translateX_direction: "negative",  // Scrolls left
  motion_fx_translateX_speed: 4,               // 4px per unit
  motion_fx_translateX_affectedRange: {
    start: 0,
    end: 100
  }
}
```

### Content Pattern
```
| barbershop | amiro | barbershop | amiro | barbershop | amiro | ...
```

**Details:**
- Repeating pattern: "| barbershop | amiro"
- 18 total instances in HTML (creates seamless loop)
- Scrolls continuously to the left
- Icon-list widget with inline layout

### Implementation
- Widget: `elementor-widget-icon-list`
- Layout: Inline items
- Uses Elementor's built-in motion effects

---

## 4. SERVICES INTRODUCTION SECTION

### Desktop Layout (Two-Column Grid)

```
┌─────────────────┬──────────────────────────┐
│                 │  "humans in mirror are   │
│   [Image 1]     │   braver than they       │ ← Sticky heading
│   Portrait      │   appear"                │   (offset: 150px)
│                 │                          │
│                 │                          │
│                 │   [Image 2]              │
│                 │   Portrait               │
│                 │                          │
└─────────────────┴──────────────────────────┘
```

### Elements

#### Left Column
- **Large Portrait Image**
  - File: `image-for-export-777x1024.jpg`
  - Aspect ratio: Portrait (777x1024)
  - Full height of column

#### Right Column
1. **Sticky Heading** (Desktop only)
   - Text: "humans in mirror are braver than they appear"
   - Behavior: Scrolls normally, then sticks at 150px from top
   - Settings: `sticky: "top"`, `sticky_offset: 150`
   - Hidden on mobile: `elementor-hidden-mobile`

2. **Second Portrait Image**
   - File: `image-for-exports-777x1024.jpg`
   - Below the sticky heading

### Mobile Layout
```
┌─────────────────┐
│   [Image 1]     │
├─────────────────┤
│   [Image 2]     │
└─────────────────┘
```
- Single column stack
- Sticky heading hidden
- Images flow vertically

### Technical Details
- Container ID: `717cf47f`
- Child containers: `2813113d`, `6041680c`
- Sticky heading ID: `73363332`

---

## 5. SERVICE ITEMS GRID

### Overall Structure
- **Layout:** Vertical stack of full-width sections
- **Pattern:** Repeating service item modules
- **Grid:** Each service gets full row

### Individual Service Item Pattern

```
┌────────────────────────────────────────────┐
│  Service Heading (e.g., "luca", "adrian")  │
├────────────────────────────────────────────┤
│                                            │
│         [Service Image]                    │
│         (Boxed Container)                  │
│                                            │
├────────────────────────────────────────────┤
│  Service Description/Details               │
├────────────────────────────────────────────┤
│  [Book >]  ← CTA Button                   │
└────────────────────────────────────────────┘
```

### Service List
The page features multiple service items following this pattern. Each service has:
1. **Heading** (h3 tag)
2. **Image** (in `e-flex e-con-boxed` container)
3. **Description text**
4. **CTA Button**

### Button Styling
- **Text:** "Book >"
- **Class:** `bottone-scritta` (brand button style)
- **Animation:** `fadeInLeft` (animated-slow)
  - Appears from left when scrolled into view
- **Alignment:** Left
- **Trigger:** On scroll into viewport

### Container Structure
```
Parent: e-con-full e-flex (ID: 4689713)
  ├─ Service Container (e-con-full e-flex)
  │   ├─ Heading Container
  │   ├─ Image Container (e-flex e-con-boxed)
  │   ├─ Text Container
  │   └─ Button Container
  ├─ Service Container (repeats)
  └─ ...
```

### Responsive Behavior
- Desktop: Maintain pattern with optimal spacing
- Tablet: Adjust image sizes
- Mobile: Stack all elements vertically

---

## 6. TEAM SECTION

### Structure
- **Section Heading:** "team" (h3)
- **Layout:** Grid/carousel of team member cards

### Team Members (5 Total)
1. **luca** (h2)
2. **adrian** (h2)
3. **jacopo** (h2)
4. **nicholas** (h2)
5. **DELIA** (h2)

### Team Member Card Pattern
Each card likely contains:
- **Photo:** Team member portrait
- **Name:** Large heading (h2)
- **Role/Bio:** (if included)
- **Book Button:** Optional CTA

### Layout Notes
- Member names use h2 tags (large emphasis)
- Could be grid layout or carousel/slider
- Likely includes hover effects on photos

---

## 7. ABOUT SECTION

### Structure
- **Main Heading:** "about" (h3)
- **Layout:** Story-driven content sections

### Subsections & Headlines

1. **"we don't do normal"**
   - Likely hero-style statement section

2. **"A Dream Born from Friendship"**
   - Origin story subsection

3. **"If We don't Look Good, You don't Look Good!"**
   - Philosophy/values statement

4. **"BUILT ON FRIENDSHIP, CRAFTED WITH CARE."**
   - Core values statement

5. **"A place where Friendship, Craftsmanship, and Style meet"**
   - Mission statement

### Content Type
- Story-driven narrative
- Multiple subsections with h3 headings
- Mix of text and imagery
- Emphasis on friendship, craftsmanship, care

### Tone
- Personal and authentic
- Bold, confident statements
- Community-focused

---

## 8. CONTACT / FOOTER SECTION

### Structure
- **Main Heading:** "contact" (h2)
- **Layout:** Multi-column footer grid

### Footer Subsections

#### 1. "follow us" (h2)
**Social Media Links:**
- Instagram: https://www.instagram.com/amirobarbershops/
- TikTok: https://www.tiktok.com/@amiro.barbershops

#### 2. "OPENING HOURS" (h4)
Hours of operation (likely formatted as list)

#### 3. "contact Us" (h4)
Contact information including:
- Phone number (tel: link)
- Email address (mailto: link)
- Physical address (if present)

### Footer Bottom
- **Copyright:** "Copyright © 2025 amiro barbershop | All rights reserved."
- **Credits:** "Powered by [service]"

### Additional Footer Elements
- Privacy Policy link
- Terms & Conditions (if present)
- Additional CTAs: "call us", "Book now"

---

## 9. GRID STRUCTURE & LAYOUT SYSTEM

### Framework
- **Builder:** Elementor Pro (WordPress)
- **System:** Container-based (Flexbox)
- **Responsive:** Mobile-first approach

### Container Types

#### `e-con-full`
- **Width:** 100% viewport width
- **Use case:** Full-width sections
- **Padding:** Usually none

#### `e-con-boxed`
- **Width:** Constrained (max-width)
- **Use case:** Contained content
- **Padding:** Inner padding applied

#### `e-flex`
- **Display:** Flexbox
- **Use case:** Flexible layouts
- **Alignment:** Various justify/align options

#### `e-parent` / `e-child`
- **Hierarchy:** Nested container structure
- **Behavior:** Parent controls child layout

### Responsive Breakpoints

#### Desktop
- Full multi-column layouts
- Side-by-side content
- All features visible

#### Tablet
- Reduced columns (2-column → 1-column)
- Adjusted spacing
- Some elements hidden

#### Mobile
- Single column stack
- Hamburger navigation
- Mobile-specific CTAs

### Width Modifiers
- `elementor-widget__width-auto`: Auto width
- `elementor-widget__width-inherit`: Inherit from parent
- `elementor-widget__width-initial`: Initial/default width

---

## 10. SCROLLING & ANIMATION ELEMENTS

### Marquee Animation
**Location:** After hero section

**Settings:**
```javascript
{
  type: "motion_fx_scrolling",
  effect: "translateX",
  direction: "negative",  // Left scroll
  speed: 4,               // 4px per unit
  range: "0-100%"         // Full range
}
```

**Behavior:**
- Continuous infinite scroll
- No pause on hover
- Smooth animation

### Sticky Elements

#### 1. Header Navigation
- **Behavior:** Sticks to top on scroll
- **Offset:** 0px
- **Devices:** Desktop, tablet, mobile

#### 2. Service Section Heading
- **Text:** "humans in mirror are braver than they appear"
- **Behavior:** Scrolls normally, then sticks
- **Offset:** 150px from top
- **Parent:** Sticky within parent container
- **Devices:** Desktop only (hidden on mobile)

### Fade Animations

#### Service CTAs
- **Animation:** `fadeInLeft`
- **Speed:** `animated-slow`
- **Trigger:** On scroll into viewport
- **Elements:** All "Book >" buttons in service grid

#### Implementation
```html
<div class="elementor-invisible" data-settings='{"_animation":"fadeInLeft"}'>
  <button>Book ></button>
</div>
```

---

## 11. IMAGE PLACEMENT STRATEGY

### Hero Section
- **Type:** Background image
- **Size:** Full viewport coverage
- **Position:** Center center
- **Effect:** Fixed or parallax (possibly)

### Services Introduction
- **Quantity:** 2 large portrait images
- **Aspect Ratio:** 777x1024 (portrait)
- **Layout:** 
  - Desktop: Two-column (one per column)
  - Mobile: Stacked vertically
- **Container:** One full, one in boxed container

### Service Items Grid
- **Quantity:** One image per service
- **Container:** `e-flex e-con-boxed`
- **Width:** `elementor-widget__width-initial` (auto-sizing)
- **Layout:** Alternating or consistent positioning

### Team Section
- **Quantity:** 5 images (one per team member)
- **Type:** Portrait photos
- **Aspect Ratio:** Likely square or portrait
- **Layout:** Grid or carousel

### About Section
- **Quantity:** Multiple supporting images
- **Purpose:** Storytelling and atmosphere
- **Container:** Mixed (boxed and full)

### Image Loading Strategy
- Lazy loading (likely enabled)
- Responsive srcset (different sizes per device)
- WebP format (modern browsers)

---

## 12. CALL-TO-ACTION (CTA) POSITIONING

### Header (Sticky - Always Visible)
**Desktop:**
- "call us" button (right side)
- "Book now" button (right side)

**Mobile:**
- Hamburger menu (right side)

### Hero Section

**Mobile:**
```
┌─────────────────────┐
│ [Book now] [call us]│ ← Top of hero
│                     │
│    Main Heading     │
└─────────────────────┘
```

**Desktop:**
```
┌─────────────────────────┐
│                         │
│     Main Heading        │
│                         │
│    [Schedule your  ]    │ ← Bottom right
│    [ haircut today>]    │
└─────────────────────────┘
```

### Service Items Grid
**Every Service Has:**
- "Book >" button
- Position: Left aligned
- Animation: fadeInLeft
- Class: `bottone-scritta`

### Footer/Contact
- "call us" button (repeated)
- "Book now" button (repeated)
- Social media CTAs ("follow us")

### CTA Hierarchy (Priority Order)
1. **Primary:** "Book now" (booking emphasis)
2. **Secondary:** "call us" (immediate contact)
3. **Tertiary:** Service-specific "Book >" buttons
4. **Supporting:** Social media follows

### Button Styling: `bottone-scritta`
This appears to be the brand's signature button class:
- Used for primary CTAs
- Special visual treatment
- Consistent across all major actions

---

## 13. COMPLETE SECTION LIST (TOP TO BOTTOM)

### Page Structure Flow

```
┌─────────────────────────────────────────────┐
│  1. STICKY HEADER / NAVIGATION              │
│     - Logo center                           │
│     - Social links (left, desktop)          │
│     - CTA buttons (right, desktop)          │
├─────────────────────────────────────────────┤
│  2. HERO SECTION (Full Viewport)            │
│     - Background image                      │
│     - Large heading                         │
│     - Primary CTA                           │
├─────────────────────────────────────────────┤
│  3. HORIZONTAL SCROLLING MARQUEE            │
│     - "| barbershop | amiro" (repeating)    │
│     - Infinite scroll animation             │
├─────────────────────────────────────────────┤
│  4. SERVICES INTRODUCTION                   │
│     - Two-column layout (desktop)           │
│     - Portrait images                       │
│     - Sticky heading                        │
├─────────────────────────────────────────────┤
│  5. SERVICE ITEMS GRID                      │
│     - Multiple service items                │
│     - Each with image + heading + CTA       │
│     - Repeating pattern                     │
├─────────────────────────────────────────────┤
│  6. "WE DON'T DO NORMAL" SECTION           │
│     - Brand statement                       │
│     - Likely full-width impact section      │
├─────────────────────────────────────────────┤
│  7. TEAM SECTION                            │
│     - Heading: "team"                       │
│     - 5 team members:                       │
│       * luca                                │
│       * adrian                              │
│       * jacopo                              │
│       * nicholas                            │
│       * DELIA                               │
│     - Grid/carousel layout                  │
├─────────────────────────────────────────────┤
│  8. ABOUT SECTION                           │
│     - Heading: "about"                      │
│     - Subsections:                          │
│       * A Dream Born from Friendship        │
│       * If We don't Look Good, You don't... │
│       * BUILT ON FRIENDSHIP, CRAFTED...     │
│       * A place where Friendship...         │
│     - Story-driven content                  │
├─────────────────────────────────────────────┤
│  9. CONTACT SECTION                         │
│     - Heading: "contact"                    │
├─────────────────────────────────────────────┤
│  10. FOOTER                                 │
│      - "follow us" (social links)           │
│      - "OPENING HOURS"                      │
│      - "contact Us"                         │
│      - Copyright notice                     │
│      - Privacy policy link                  │
└─────────────────────────────────────────────┘
```

---

## DESIGN PATTERNS & KEY TAKEAWAYS

### Typography Hierarchy
1. **h1:** Hero heading (largest)
2. **h2:** Major section headings, team names
3. **h3:** Section labels, subsection headings
4. **h4:** Footer subsections

### Layout Patterns
- **Full-width sections** for impact (hero, marquee)
- **Two-column layouts** for content depth (services intro)
- **Vertical stacks** for services/team (mobile-friendly)
- **Boxed containers** for constrained content areas

### Animation Strategy
- **Continuous:** Marquee scrolling
- **Sticky:** Header and key headings
- **On-scroll:** Service CTAs (fadeInLeft)
- **Hover:** Likely on buttons and images

### Responsive Strategy
- **Mobile-first:** Single column stacks
- **Progressive enhancement:** Add columns on larger screens
- **Smart hiding:** Desktop social icons, mobile hamburger
- **Device-specific CTAs:** Different placement per device

### Brand Consistency
- **Button class:** `bottone-scritta` used throughout
- **Lowercase preference:** Most headings use lowercase
- **Bold statements:** Strong, confident copy
- **Friendship theme:** Repeated throughout copy

---

## TECHNICAL IMPLEMENTATION NOTES

### Platform Stack
- **CMS:** WordPress
- **Builder:** Elementor Pro
- **Animations:** Elementor Motion Effects
- **Layout:** Flexbox containers

### Custom Classes
- `bottone-scritta` - Brand button style
- `animated-slow` - Slower animation timing
- Various `elementor-hidden-*` for responsive hiding

### Performance Considerations
- Sticky positioning (CSS)
- Transform animations (GPU accelerated)
- Lazy loading images
- Efficient HTML structure

### Accessibility Notes
- Semantic heading hierarchy
- ARIA labels on social icons
- Button labels
- Responsive touch targets

---

## REBUILD CHECKLIST

When rebuilding this site, ensure:

- [ ] Sticky header with proper z-index
- [ ] Full viewport hero section
- [ ] Horizontal marquee with infinite loop
- [ ] Sticky heading with 150px offset in services section
- [ ] Service grid with fadeInLeft animation on CTAs
- [ ] Team member cards (5 members)
- [ ] About section with 4-5 subsections
- [ ] Footer with 3 columns
- [ ] Mobile: Different CTA placement in hero
- [ ] Desktop: Social icons in header
- [ ] All buttons use `bottone-scritta` class
- [ ] Images: Portrait aspect ratio in services
- [ ] Responsive breakpoints properly configured
- [ ] Animations trigger on scroll
- [ ] Social media links functional

---

**End of Analysis**

---

## VISUAL LAYOUT DIAGRAMS

### Desktop Layout Overview

```
┌────────────────────────────────────────────────────────────────┐
│ HEADER (Sticky)                                                │
│ [Social Icons]    [AMIRO LOGO]    [call us] [Book now]        │
├────────────────────────────────────────────────────────────────┤
│                                                                │
│                     HERO SECTION                               │
│                                                                │
│                      book now                                  │
│                                                                │
│                                   [Schedule your haircut >]    │
│                                                                │
├────────────────────────────────────────────────────────────────┤
│ | barbershop | amiro | barbershop | amiro | barbershop |...   │ ← Scrolling
├────────────────────────────────────────────────────────────────┤
│                           │                                    │
│   [Portrait Image 1]      │  "humans in mirror are braver     │ ← Sticky
│   777 x 1024             │   than they appear"                │   heading
│                           │                                    │
│                           │  [Portrait Image 2]                │
│                           │  777 x 1024                        │
├────────────────────────────────────────────────────────────────┤
│                                                                │
│                   SERVICE ITEMS GRID                           │
│                                                                │
│   Heading: "luca"                                              │
│   [Service Image]                                              │
│   [Book >] ← fadeInLeft animation                             │
│                                                                │
│   Heading: "adrian"                                            │
│   [Service Image]                                              │
│   [Book >]                                                     │
│                                                                │
│   (repeating pattern...)                                       │
│                                                                │
├────────────────────────────────────────────────────────────────┤
│                    WE DON'T DO NORMAL                          │
├────────────────────────────────────────────────────────────────┤
│                      TEAM SECTION                              │
│                                                                │
│   [luca]  [adrian]  [jacopo]  [nicholas]  [DELIA]            │
│                                                                │
├────────────────────────────────────────────────────────────────┤
│                     ABOUT SECTION                              │
│                                                                │
│   A Dream Born from Friendship                                 │
│   If We don't Look Good, You don't Look Good!                 │
│   BUILT ON FRIENDSHIP, CRAFTED WITH CARE.                      │
│                                                                │
├────────────────────────────────────────────────────────────────┤
│                    CONTACT SECTION                             │
│                                                                │
│   follow us    │   OPENING HOURS   │   contact Us             │
│   [Social]     │   Hours listing   │   Phone/Email            │
│                                                                │
├────────────────────────────────────────────────────────────────┤
│ FOOTER                                                         │
│ Copyright © 2025 amiro barbershop | Privacy Policy            │
└────────────────────────────────────────────────────────────────┘
```

### Mobile Layout Overview

```
┌──────────────────────────┐
│ [≡]  [LOGO]              │ ← Header
├──────────────────────────┤
│                          │
│  [Book now] [call us]    │ ← Mobile CTAs
│                          │
│        book              │
│        now               │ ← Hero
│                          │
├──────────────────────────┤
│ | barbershop | amiro |   │ ← Marquee
├──────────────────────────┤
│   [Image 1]              │
│                          │
│   [Image 2]              │ ← Services
│                          │   (stacked)
├──────────────────────────┤
│   Heading                │
│   [Service Image]        │
│   [Book >]               │
│                          │
│   Heading                │ ← Service
│   [Service Image]        │   Items
│   [Book >]               │   (stacked)
│                          │
│   (repeating...)         │
├──────────────────────────┤
│   WE DON'T DO NORMAL     │
├──────────────────────────┤
│   [Team Member 1]        │
│   [Team Member 2]        │ ← Team
│   [Team Member 3]        │   (stacked)
│   [Team Member 4]        │
│   [Team Member 5]        │
├──────────────────────────┤
│   About Content          │
│   (stacked sections)     │ ← About
├──────────────────────────┤
│   Follow Us              │
│   [Social Links]         │
│                          │
│   OPENING HOURS          │
│   [Hours]                │ ← Contact
│                          │   (stacked)
│   Contact Us             │
│   [Phone/Email]          │
├──────────────────────────┤
│   Footer                 │
└──────────────────────────┘
```

---

## EXACT HERO POSITIONING

### Desktop Hero Layout (EXACT)

```
┌─────────────────────────────────────────────────────────────────────┐
│                                                                     │
│                                                                     │
│                                                                     │
│                          book                                       │
│                          now                                        │  ← Centered
│                                                                     │     vertically
│                                                                     │     and
│                                                                     │     horizontally
│                                                                     │
│                                                                     │
│                                          ┌───────────────────────┐ │
│                                          │  Schedule your        │ │  ← Bottom
│                                          │  haircut today >      │ │     right
│                                          └───────────────────────┘ │     corner
└─────────────────────────────────────────────────────────────────────┘
```

**Positioning Details:**
- Main heading: `position: absolute; top: 50%; left: 50%; transform: translate(-50%, -50%)`
- CTA button: `position: absolute; bottom: [value]px; right: [value]px`
- Button class: `bottone-scritta`
- Hidden on tablet/mobile: `elementor-hidden-tablet elementor-hidden-mobile`

### Mobile Hero Layout (EXACT)

```
┌─────────────────────────────────────┐
│  ┌─────────────┐  ┌─────────────┐  │  ← Top: 20-40px from top
│  │  Book now   │  │  call us    │  │     Full width buttons
│  └─────────────┘  └─────────────┘  │     Side by side
│                                     │
│                                     │
│                                     │
│              book                   │  ← Centered vertically
│              now                    │     and horizontally
│                                     │
│                                     │
│                                     │
│                                     │
└─────────────────────────────────────┘
```

**Positioning Details:**
- Mobile buttons: `position: absolute; top: [value]px; width: auto`
- Button containers: Two separate containers, side by side
- Only visible: `elementor-hidden-desktop`
- Main heading: Same center position as desktop

---

## SCROLLING MARQUEE - EXACT SPECS

### Marquee Implementation

```css
/* Elementor Motion Effects */
.elementor-element-16c307b9 {
  /* Container settings */
  background: [color/pattern];
  width: 100vw;
  overflow: hidden;
  
  /* Inner list settings */
  .elementor-icon-list {
    display: inline-flex;
    white-space: nowrap;
    
    /* Animation */
    animation: marquee-scroll [duration]s linear infinite;
  }
}

@keyframes marquee-scroll {
  from {
    transform: translateX(0);
  }
  to {
    transform: translateX(-50%); /* Moves exactly half to create seamless loop */
  }
}
```

**Content Structure:**
```html
<div class="marquee-container">
  <ul class="icon-list">
    <li>| barbershop</li>
    <li>| amiro</li>
    <li>| barbershop</li>
    <li>| amiro</li>
    <!-- Repeated 9 times (18 total items) -->
  </ul>
</div>
```

**Motion Settings:**
- Speed: 4px per scroll unit
- Direction: Negative X (left)
- Range: 0-100%
- Devices: All (desktop, tablet, mobile)

---

## SERVICE GRID - EXACT PATTERN

### Pattern A: Service Item

```
Container (e-con-full e-flex)
├─ Heading Container
│  └─ <h3>Service Name</h3>
├─ Image Container (e-flex e-con-boxed)
│  └─ <img src="[service-image].jpg" />
├─ Text Container
│  └─ <p>Service description...</p>
└─ Button Container (fadeInLeft animation)
   └─ <button class="bottone-scritta">Book ></button>
```

### Animation Trigger

```javascript
// Intersection Observer for fadeInLeft
observer.observe(element, {
  rootMargin: "0px",
  threshold: 0.1  // Triggers when 10% visible
});

// On intersection
element.classList.add('elementor-animation-fadeInLeft');
element.classList.remove('elementor-invisible');
```

---

## REBUILD PRIORITY ORDER

### Phase 1: Structure (Must Have)
1. Header with sticky positioning
2. Hero section with responsive CTA placement
3. Marquee with infinite scroll
4. Service grid with repeating pattern
5. Team section with 5 members
6. About section with subsections
7. Contact/Footer

### Phase 2: Styling (Should Have)
1. `bottone-scritta` button class
2. Typography hierarchy
3. Color scheme
4. Spacing system
5. Image aspect ratios

### Phase 3: Interactivity (Nice to Have)
1. Sticky heading in services section
2. fadeInLeft animations on scroll
3. Hover effects on buttons/images
4. Smooth scroll
5. Mobile menu functionality

### Phase 4: Optimization (Enhancement)
1. Image lazy loading
2. WebP format images
3. CSS optimization
4. Animation performance
5. Accessibility improvements

---

**Document Complete** - Use this as your single source of truth for rebuilding the Amiro Barbershop website structure.

# AMIRO BARBERSHOP - DOCUMENTATION INDEX

Complete structural analysis and rebuild guides for https://www.amirobarbershops.com/

**Analysis Date:** 2025-10-28  
**Total Documentation:** 2,903 lines across 4 files

---

## QUICK START

**First time?** Read documents in this order:
1. [Quick Reference](#quick-reference) - 5 min overview
2. [Structure Analysis](#structure-analysis) - 15 min deep dive
3. [HTML Patterns](#html-patterns) - Copy-paste code patterns
4. [Design Spec](#design-spec) - Visual design details (if exists)

---

## DOCUMENTATION FILES

### 1. AMIRO_STRUCTURE_ANALYSIS.md
**File:** `AMIRO_STRUCTURE_ANALYSIS.md`  
**Size:** 35KB | 1,012 lines  
**Purpose:** Complete structural breakdown

**Contains:**
- 13 detailed sections covering every part of the site
- Exact positioning for hero elements (left/right/center)
- Navigation structure and behavior
- Scrolling marquee implementation details
- Grid structure and layout system
- Animation specifications
- Image placement strategy
- CTA positioning throughout the site
- Visual layout diagrams (ASCII art)
- Desktop vs mobile comparisons
- Rebuild priority checklist

**Best for:** Deep understanding, reference during build, troubleshooting

**Key Sections:**
- Header/Navigation (3-column sticky)
- Hero Section (exact positioning details)
- Scrolling Marquee (animation specs)
- Services Introduction (two-column + sticky heading)
- Service Items Grid (repeating pattern)
- Team Section (5 members)
- About Section (4 subsections)
- Contact/Footer (3-column layout)

---

### 2. AMIRO_QUICK_REFERENCE.md
**File:** `AMIRO_QUICK_REFERENCE.md`  
**Size:** 3.8KB | 149 lines  
**Purpose:** Fast lookup reference

**Contains:**
- 10-section layout summary
- Key measurements table
- Critical CSS classes
- Animation quick reference
- Responsive breakpoints guide
- CTA placement map
- Content checklist (team members, about sections, social links)
- Build order steps
- Common mistakes to avoid

**Best for:** Quick lookups while building, checklist validation

**Tables:**
- Key Measurements (desktop vs mobile)
- Animation Reference
- Responsive Breakpoints

---

### 3. AMIRO_HTML_PATTERNS.md
**File:** `AMIRO_HTML_PATTERNS.md`  
**Size:** 11KB | 582 lines  
**Purpose:** Copy-paste HTML/CSS patterns

**Contains:**
- Complete HTML structure for each section
- CSS positioning code
- Animation JavaScript (Intersection Observer)
- Responsive utility classes
- Button styling (bottone-scritta)
- Grid layouts
- Marquee implementation with CSS animation
- Complete page structure template

**Best for:** Implementation, copying code patterns, understanding structure

**Patterns Included:**
- Header Structure
- Hero Section (with exact CSS positioning)
- Scrolling Marquee (HTML + CSS animation)
- Services Introduction (two-column grid)
- Service Item (repeating pattern)
- Team Member Card
- About Section
- Footer/Contact Section
- Button class: bottone-scritta
- Responsive utility classes

---

### 4. AMIRO_DESIGN_SPEC.md
**File:** `AMIRO_DESIGN_SPEC.md`  
**Size:** 23KB | 1,160 lines  
**Purpose:** Visual design specifications (if created previously)

**Contains:** Typography, colors, spacing, brand assets

---

## SECTION-BY-SECTION GUIDE

### Header / Navigation
- **Docs:** Structure Analysis p.25-30
- **Pattern:** HTML Patterns p.1-20
- **Quick Ref:** Quick Reference "CTA Placement Map"

**What to build:**
- 3-column sticky header
- Social icons (left, desktop only)
- Logo (center, always visible)
- CTA buttons (right, desktop only)
- Hamburger menu (mobile)

---

### Hero Section
- **Docs:** Structure Analysis p.31-62
- **Pattern:** HTML Patterns p.21-65
- **Quick Ref:** Quick Reference "Key Measurements"

**What to build:**
- Full viewport height (100vh)
- Background image
- Centered heading with line break
- Mobile: Two CTAs at top
- Desktop: One CTA bottom-right

**Critical:** Different CTA positioning for mobile vs desktop!

---

### Scrolling Marquee
- **Docs:** Structure Analysis p.63-86
- **Pattern:** HTML Patterns p.66-105
- **Quick Ref:** Quick Reference "Animation Quick Ref"

**What to build:**
- Horizontal infinite scroll
- "| barbershop | amiro" repeating pattern
- CSS translateX animation
- Speed: 4px per unit
- Direction: Left (negative)

---

### Services Introduction
- **Docs:** Structure Analysis p.87-129
- **Pattern:** HTML Patterns p.106-140
- **Quick Ref:** Quick Reference "Key Measurements"

**What to build:**
- Two-column grid (desktop)
- Left: Large portrait image
- Right: Sticky heading + image
- Sticky offset: 150px
- Single column stack (mobile)

**Critical:** Sticky heading hidden on mobile!

---

### Service Items Grid
- **Docs:** Structure Analysis p.130-178
- **Pattern:** HTML Patterns p.141-210
- **Quick Ref:** Quick Reference "Animation Quick Ref"

**What to build:**
- Repeating pattern per service
- Heading → Image → Description → CTA
- CTA button: "Book >"
- Animation: fadeInLeft on scroll
- Class: bottone-scritta

---

### Team Section
- **Docs:** Structure Analysis p.179-199
- **Pattern:** HTML Patterns p.211-245
- **Quick Ref:** Quick Reference "Content Checklist"

**What to build:**
- 5 team members: luca, adrian, jacopo, nicholas, DELIA
- Grid layout (responsive)
- Photo + name per member
- Photos: Square or portrait

---

### About Section
- **Docs:** Structure Analysis p.200-227
- **Pattern:** HTML Patterns p.246-275
- **Quick Ref:** Quick Reference "Content Checklist"

**What to build:**
- 4 subsections with h3 headings
- Story-driven content
- Mix of text and imagery
- Emphasis on friendship/craftsmanship

---

### Footer / Contact
- **Docs:** Structure Analysis p.228-252
- **Pattern:** HTML Patterns p.276-330
- **Quick Ref:** Quick Reference "Content Checklist"

**What to build:**
- 3-column grid layout
- Follow Us (social links)
- Opening Hours
- Contact Us (phone/email)
- Footer bottom (copyright)

---

## KEY FEATURES TO IMPLEMENT

### Animations
1. **Marquee:** Infinite horizontal scroll (always on)
2. **Service CTAs:** fadeInLeft on scroll (10% visible threshold)
3. **Sticky Elements:** Header (0px), Services heading (150px)

**Implementation:** See HTML Patterns p.160-210 for JavaScript code

---

### Responsive Behavior
- **Desktop:** Multi-column, all features visible
- **Tablet:** Reduced columns, some hiding
- **Mobile:** Single column stack, hamburger menu

**Classes:** See HTML Patterns p.331-350 for utility classes

---

### Brand Styling
- **Button Class:** `bottone-scritta` (use everywhere!)
- **Typography:** Mostly lowercase, bold headings
- **Spacing:** Generous whitespace
- **Animations:** Slow, elegant

**Pattern:** See HTML Patterns p.301-325 for button CSS

---

## REBUILD WORKFLOW

### Phase 1: Structure (Day 1)
1. Set up HTML skeleton (see HTML Patterns p.351-385)
2. Create header with sticky behavior
3. Build hero section with responsive CTAs
4. Add marquee section
5. Create services intro grid
6. Build service items pattern
7. Add team section
8. Create about section
9. Add footer/contact

### Phase 2: Styling (Day 2)
1. Implement bottone-scritta button class
2. Set up typography hierarchy
3. Add color scheme
4. Configure spacing system
5. Style images (aspect ratios)

### Phase 3: Interactivity (Day 3)
1. Add sticky positioning
2. Implement scroll animations (fadeInLeft)
3. Add hover effects
4. Test mobile menu
5. Smooth scroll

### Phase 4: Testing & Optimization (Day 4)
1. Responsive testing (mobile, tablet, desktop)
2. Animation performance
3. Image optimization (lazy loading, WebP)
4. Accessibility audit
5. Cross-browser testing

---

## COMMON PITFALLS

1. Forgetting `bottone-scritta` class on CTAs
2. Wrong hero CTA placement (mobile vs desktop)
3. Marquee not looping seamlessly
4. Missing 150px sticky offset on services heading
5. Service CTAs not animating (fadeInLeft)
6. Social icons showing on mobile (should be hidden)
7. Wrong image aspect ratios (should be portrait 777x1024)

---

## QUICK LOOKUPS

### Need to know...
- **Hero CTA positioning?** → Structure Analysis p.40-55
- **Marquee animation code?** → HTML Patterns p.66-105
- **Team member names?** → Quick Reference "Content Checklist"
- **Button styling?** → HTML Patterns p.301-325
- **Responsive breakpoints?** → Quick Reference "Responsive Breakpoints"
- **All animations?** → Quick Reference "Animation Quick Ref"
- **Social links?** → Quick Reference "Content Checklist"

---

## FILE SIZES

| File | Size | Lines | Purpose |
|------|------|-------|---------|
| AMIRO_STRUCTURE_ANALYSIS.md | 35KB | 1,012 | Deep reference |
| AMIRO_QUICK_REFERENCE.md | 3.8KB | 149 | Fast lookup |
| AMIRO_HTML_PATTERNS.md | 11KB | 582 | Code patterns |
| AMIRO_DESIGN_SPEC.md | 23KB | 1,160 | Design details |
| **TOTAL** | **73KB** | **2,903** | Complete docs |

---

## GETTING HELP

### Can't find something?
1. Check Quick Reference first (fastest)
2. Search Structure Analysis (most detailed)
3. Look in HTML Patterns (code examples)

### Need code?
- HTML Patterns has copy-paste ready code

### Need exact positioning?
- Structure Analysis has ASCII diagrams

### Need a checklist?
- Quick Reference has content checklists

---

## VERSION HISTORY

- **v1.0** (2025-10-28): Initial complete analysis
  - 10-section structure documented
  - All animations specified
  - Responsive behavior mapped
  - HTML patterns created
  - Quick reference guide added

---

**Ready to build?** Start with [AMIRO_QUICK_REFERENCE.md](./AMIRO_QUICK_REFERENCE.md) for the 5-minute overview!

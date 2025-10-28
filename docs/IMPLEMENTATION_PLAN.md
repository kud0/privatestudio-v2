# PrivateStudio Website - Implementation Plan
## Following IndyDevDan's Claude Code Best Practices

**Project:** PrivateStudio Barbershop Website Redesign
**Framework:** Astro 4.13
**Approach:** Option A - Full Dan Hierarchy (Primitives → Composition → Skills)
**Timeline:** ~4 hours for complete implementation
**Date:** October 28, 2025

---

## 🎯 Project Overview

### Goals
1. Create modern barbershop website inspired by AMIRO Barbershops
2. Implement STMNT product showcase (Barber Barcelona style)
3. Feature 4 barbers with individual Booksy booking links
4. Full mobile responsiveness
5. Extract placeholder images for future owner photo replacement

### Design Inspiration
- **Primary:** https://www.amirobarbershops.com/
  - Horizontal scroll effect on vertical scroll
  - HYPER BIG typography (BOOK NOW, TEAM)
  - Dark theme with sophisticated animations
  - Text superposition effects

- **Secondary:** https://www.barberbarcelona.es/
  - STMNT product section layout
  - Footer information structure
  - Service card patterns

### Technical Requirements
- Astro 4.13 (already initialized)
- Tailwind CSS for styling
- GSAP for advanced animations
- Intersection Observer for scroll effects
- Mobile-first responsive design
- Placeholder image extraction system

---

## 📚 IndyDevDan's Hierarchy Applied

### The Core Four Principles
Every implementation is built on:
1. **Context** - Design specs, inspiration sites, requirements
2. **Model** - Claude Sonnet 4.5 for code generation
3. **Prompt** - Slash commands (our primitives)
4. **Tools** - Astro, Tailwind, GSAP, etc.

### Composition Hierarchy for This Project
```
┌─────────────────────────────────────────────┐
│  privatestudio-manager SKILL (Top Level)   │
│  Manages all website sections & updates    │
└─────────────────────────────────────────────┘
                    │
        ┌───────────┼───────────┐
        │           │           │
┌───────▼──────┐ ┌──▼────────┐ ┌▼──────────┐
│  SLASH       │ │  SLASH    │ │  SLASH    │
│  COMMANDS    │ │  COMMANDS │ │  COMMANDS │
│  (Setup)     │ │  (Layout) │ │  (Sections)│
└──────────────┘ └───────────┘ └───────────┘
        │           │           │
        └───────────┼───────────┘
                    │
        ┌───────────▼───────────┐
        │  Context + Model +    │
        │  Prompt + Tools       │
        └───────────────────────┘
```

**Why No Sub-Agents?** No parallel workflows needed
**Why No MCP Servers?** No external data sources, just static links

---

## 🏗️ Phase 1: Create Slash Commands (Primitives)

### Directory Structure
```
privatestudio-v2/.claude/commands/
├── setup/
│   ├── astro-init.md
│   └── install-dependencies.md
├── layout/
│   ├── create-base-layout.md
│   ├── create-nav.md
│   └── create-footer.md
├── sections/
│   ├── create-hero.md
│   ├── create-horizontal-scroll.md
│   ├── create-team-section.md
│   ├── create-products-section.md
│   └── create-about-section.md
├── assets/
│   └── extract-images.md
└── styling/
    ├── create-design-tokens.md
    └── create-animations.md
```

### Detailed Command Specifications

#### Setup Commands

**1. `/astro-init`** ✅ (Already done)
- Initialize Astro 4.13 project
- Configure TypeScript
- Set up basic project structure

**2. `/install-dependencies`**
- Install Tailwind CSS
- Install GSAP (GreenSock Animation Platform)
- Install @astrojs/tailwind integration
- Install any image optimization packages

#### Layout Commands

**3. `/create-base-layout`**
- Create `src/layouts/BaseLayout.astro`
- Set up HTML structure
- Include global styles
- Meta tags for SEO
- Responsive viewport configuration

**4. `/create-nav`**
- Create `src/components/Navigation.astro`
- Responsive hamburger menu for mobile
- Sticky navigation behavior
- Language switcher (ES/EN optional)
- "Book Now" CTA button
- Social links integration

**5. `/create-footer`**
- Create `src/components/Footer.astro`
- Extract content from https://www.barberbarcelona.es/
- Location: Carrer de Muntaner, 172, BAJO 01, 08036 Barcelona
- Hours: Mon-Fri 10:00-20:00, Sat 10:00-15:00, Closed Sun
- Contact: +34 624 367 153, privatestudio2@gmail.com
- Social: Instagram (@private.studiobcn), Facebook (PrivateStudioBcn)
- Google Maps embed

#### Section Commands

**6. `/create-hero`**
- Create `src/components/Hero.astro`
- HYPER BIG typography (150px+ on desktop)
- "BOOK NOW" massive headline
- Smooth scroll indicator
- Dark background with subtle texture
- Animated entrance effect
- Mobile scaling (60-80px on mobile)

**7. `/create-horizontal-scroll`**
- Create `src/components/HorizontalScroll.astro`
- Implement horizontal text scroll on vertical scroll
- Use GSAP ScrollTrigger
- Repeating marquee effect: "WE DON'T DO NORMAL"
- Bidirectional scroll (left/right on scroll up/down)
- Performance optimized with requestAnimationFrame

**8. `/create-team-section`**
- Create `src/components/Team.astro`
- 4 barber cards (placeholders)
- Each card includes:
  - Photo placeholder
  - Name
  - Title/Specialty
  - Individual Booksy link
  - Hover effects
- Grid layout: 2x2 on desktop, 1 column on mobile
- Staggered entrance animations

**9. `/create-products-section`**
- Create `src/components/Products.astro`
- STMNT product showcase
- Inspired by Barber Barcelona's STMNT section
- Product grid (3 columns desktop, 1 mobile)
- Product cards:
  - Image placeholder
  - Product name
  - Brief description
  - Price (if applicable)
  - Hover zoom effect
- Section headline: "STMNT GROOMING GOODS"

**10. `/create-about-section`**
- Create `src/components/About.astro`
- "PrivateStudio Story" or similar
- Large typography for section title
- Two-column layout (text + image)
- Parallax effect on background image
- Text superposition effect (image #1 reference)

#### Asset Commands

**11. `/extract-images`**
- Create `src/scripts/extractImages.js`
- Download hero backgrounds from inspiration sites
- Create placeholder system:
  - `barber-placeholder-1.jpg` through `barber-placeholder-4.jpg`
  - `product-placeholder-1.jpg` through `product-placeholder-6.jpg`
  - `about-background.jpg`
  - `hero-background.jpg`
- Store in `public/images/placeholders/`
- Document image dimensions and aspect ratios
- Create README.md for image replacement instructions

#### Styling Commands

**12. `/create-design-tokens`**
- Create `src/styles/design-tokens.css`
- Extract from AMIRO analysis:
  ```css
  /* Colors */
  --color-bg-dark: #2F2F2F;
  --color-bg-black: #000000;
  --color-text-light: #E2E0DF;
  --color-accent: #968C8C;
  --color-link: #1863DC;

  /* Typography Scale (HYPER BIG) */
  --font-size-xs: 13px;
  --font-size-sm: 20px;
  --font-size-md: 36px;
  --font-size-lg: 42px;
  --font-size-xl: 80px;
  --font-size-2xl: 120px;
  --font-size-3xl: 180px;  /* For BOOK NOW, TEAM */

  /* Spacing */
  --spacing-xs: 0.44rem;
  --spacing-sm: 1rem;
  --spacing-md: 1.5rem;
  --spacing-lg: 3rem;
  --spacing-xl: 5.06rem;

  /* Shadows */
  --shadow-natural: 6px 6px 9px rgba(0, 0, 0, 0.2);

  /* Breakpoints */
  --breakpoint-mobile: 767px;
  --breakpoint-tablet: 1024px;
  --breakpoint-desktop: 1440px;
  ```

**13. `/create-animations`**
- Create `src/styles/animations.css`
- Fade in entrance animations
- Stagger animations for grids
- Hover effects (scale, glow, shadow)
- Smooth scroll behavior
- Text reveal animations
- Parallax scroll classes

---

## 🔧 Phase 2: Create Management Skill

### Skill Structure
```
privatestudio-v2/.claude/skills/
└── privatestudio-manager/
    ├── SKILL.md
    └── allowed-tools
```

### SKILL.md Specification
```markdown
# PrivateStudio Website Manager

Manages the PrivateStudio barbershop website sections, content updates, and maintenance.

## Purpose
This skill coordinates all website operations for PrivateStudio, from initial build
to ongoing content updates. It uses slash commands as primitives and composes them
into coherent workflows.

## When to use this skill
The agent automatically invokes this skill when:
- User mentions "update website"
- User wants to modify barber information
- User needs to add/remove products
- User requests design changes
- User mentions "PrivateStudio website"

## Available Operations

### Content Management
- **update-hero**: Modify hero section text and CTAs
- **add-barber**: Add new barber profile with Booksy link
- **remove-barber**: Remove a barber from team section
- **update-barber**: Modify existing barber info
- **update-products**: Add/remove/modify STMNT products

### Design Updates
- **update-colors**: Modify color scheme
- **update-typography**: Adjust font sizes and styles
- **update-animations**: Modify or add new effects

### Footer & Contact
- **update-footer**: Change hours, contact info, social links
- **update-location**: Modify address or map embed

### Asset Management
- **replace-images**: Swap placeholder images with real photos
- **optimize-images**: Run image optimization
- **extract-new-images**: Download new inspiration images

## Internal Slash Commands Used
- /create-hero
- /create-team-section
- /create-products-section
- /create-footer
- /create-about-section
- /create-design-tokens
- /create-animations
- /extract-images

## Workflow Patterns

### Pattern 1: Add New Barber
1. Use /create-team-section to regenerate with 5 barbers
2. Add new barber data structure
3. Ensure Booksy link is properly formatted
4. Update responsive grid (consider 3-2 layout)

### Pattern 2: Update Design Tokens
1. Use /create-design-tokens with new values
2. Verify all components use CSS variables
3. Test responsive breakpoints
4. Regenerate any hard-coded styles

### Pattern 3: Replace Placeholder Images
1. Identify which images to replace
2. Ensure new images match aspect ratios
3. Optimize images (WebP format, proper sizing)
4. Update image paths in components
5. Test lazy loading

## Best Practices
- Always test mobile responsiveness after changes
- Maintain design token consistency
- Keep Booksy links updated and tested
- Optimize images before deployment
- Use semantic HTML for accessibility
- Follow Astro component patterns

## Resources
- Design inspiration: AMIRO Barbershops
- Product reference: Barber Barcelona STMNT section
- Current site: https://www.barberbarcelona.es/
```

### allowed-tools Specification
```
# Tools allowed for privatestudio-manager skill
Read
Write
Edit
Bash
Glob
Grep
```

---

## 🚀 Implementation Workflow - DO THIS

Just say to Claude Code:

```
"Build the complete PrivateStudio website"
```

That's it. The skill handles everything.

---

## 📋 What Happens When You Run That Command

The skill will automatically:

1. Install Tailwind, GSAP, and dependencies
2. Create design system (colors, typography, spacing)
3. Create animations (keyframes, transitions)
4. Build base layout, navigation, and footer
5. Build hero section with HYPER BIG typography
6. Build horizontal scroll effect
7. Build team section (4 barbers with Booksy links)
8. Build products section (STMNT showcase)
9. Build about section
10. Extract placeholder images
11. Create main index page with all sections
12. Test responsive design

Total time: ~2-3 hours (automated)

---

## 🔄 After Initial Build - Making Changes

**Add a barber:**
```
"Add a new barber named Juan Pérez, specialty 'Fade Expert', Booksy link [URL]"
```

**Update hero text:**
```
"Change the hero section to say 'TRANSFORM YOUR STYLE'"
```

**Change colors:**
```
"Update the accent color to gold"
```

**Replace images:**
```
"Replace barber photos with real images from /public/images/team/"
```

---

## 📱 Responsive Breakpoints

### Mobile First Approach
```css
/* Mobile: Default (320px - 767px) */
- Single column layouts
- Font sizes: 60-80px for headlines
- Hamburger menu
- Stacked team grid (1 column)
- Stacked products (1 column)

/* Tablet: 768px - 1023px */
- Two column layouts where appropriate
- Font sizes: 100-120px for headlines
- Team grid (2x2)
- Products grid (2 columns)

/* Desktop: 1024px+ */
- Full multi-column layouts
- Font sizes: 150-180px for headlines
- Team grid (2x2 or 4 columns)
- Products grid (3 columns)
- Horizontal scroll effects active
```

---

## 🎨 Design Specifications

### Color Palette
```css
Primary Background: #000000 (Pure Black)
Secondary Background: #2F2F2F (Dark Gray)
Text Primary: #E2E0DF (Off White)
Accent: #968C8C (Taupe/Bronze)
Hover/Active: #1863DC (Blue)
```

### Typography Scale
```
Display (BOOK NOW): 180px desktop, 60px mobile
Section Headers (TEAM): 120px desktop, 48px mobile
Subsection: 80px desktop, 36px mobile
Body Large: 42px desktop, 24px mobile
Body: 20px desktop, 16px mobile
Small: 13px
```

### Spacing System
```
xs: 0.44rem (7px)
sm: 1rem (16px)
md: 1.5rem (24px)
lg: 3rem (48px)
xl: 5.06rem (81px)
```

---

## 🔗 External Links & Integrations

### Booksy Links (Placeholder - Update with real links)
```
Barber 1: https://booksy.com/privatestudio/barber1
Barber 2: https://booksy.com/privatestudio/barber2
Barber 3: https://booksy.com/privatestudio/barber3
Barber 4: https://booksy.com/privatestudio/barber4
```

### Social Media
```
Instagram: @private.studiobcn
Facebook: /PrivateStudioBcn
```

### Contact
```
Phone: +34 624 367 153
Email: privatestudio2@gmail.com
Address: Carrer de Muntaner, 172, BAJO 01, 08036 Barcelona
```

---

## 📦 Deliverables Checklist

### Code Deliverables
- [ ] 13 Slash Commands created in `.claude/commands/`
- [ ] 1 Management Skill created in `.claude/skills/`
- [ ] Base Astro layout with dark theme
- [ ] Responsive navigation with hamburger menu
- [ ] Footer with contact information
- [ ] Hero section with HYPER BIG typography
- [ ] Horizontal scroll effect component
- [ ] Team section with 4 barber cards
- [ ] STMNT products showcase section
- [ ] About section with text superposition
- [ ] Design tokens CSS file
- [ ] Animations CSS file
- [ ] Image extraction script

### Documentation Deliverables
- [ ] This implementation plan
- [ ] Image replacement guide (`public/images/README.md`)
- [ ] Slash command usage documentation
- [ ] Skill usage guide
- [ ] Deployment instructions

### Testing Deliverables
- [ ] Mobile responsive test (320px, 375px, 768px, 1024px, 1440px)
- [ ] Cross-browser test (Chrome, Firefox, Safari, Edge)
- [ ] Performance audit (Lighthouse score 90+)
- [ ] Accessibility audit (WCAG AA compliance)
- [ ] Animation performance test (60fps)

---

## 🧪 Testing Strategy

### Manual Testing Checklist
```
Navigation:
- [ ] Hamburger menu opens/closes on mobile
- [ ] All navigation links work
- [ ] Smooth scroll to sections
- [ ] Book Now CTA is prominent
- [ ] Social links open in new tabs

Hero Section:
- [ ] HYPER BIG typography renders correctly
- [ ] Responsive font sizes work
- [ ] Background image loads
- [ ] CTA button is clickable

Horizontal Scroll:
- [ ] Text scrolls horizontally on vertical scroll
- [ ] Smooth 60fps performance
- [ ] Direction changes work
- [ ] Mobile performance acceptable

Team Section:
- [ ] All 4 barbers display
- [ ] Booksy links work
- [ ] Hover effects animate smoothly
- [ ] Grid layout responsive
- [ ] Images load correctly

Products Section:
- [ ] Product grid displays correctly
- [ ] Hover zoom effects work
- [ ] Layout responsive on all devices
- [ ] Images optimized

Footer:
- [ ] Contact info correct
- [ ] Hours displayed properly
- [ ] Google Maps embed works
- [ ] Social links functional

General:
- [ ] All images have alt text
- [ ] Page loads in < 3 seconds
- [ ] No console errors
- [ ] Smooth animations (no jank)
```

### Performance Targets
```
Lighthouse Scores:
- Performance: 90+
- Accessibility: 95+
- Best Practices: 100
- SEO: 100

Load Times:
- First Contentful Paint: < 1.5s
- Time to Interactive: < 3s
- Total Page Size: < 2MB
```

---

## 🚢 Deployment Plan

### Pre-Deployment
1. Run `npm run build`
2. Test build locally with `npm run preview`
3. Verify all assets are included
4. Check all links work
5. Run final performance audit

### Deployment Options
1. **Vercel** (Recommended for Astro)
   - Connect GitHub repo
   - Auto-deploy on push
   - Custom domain setup

2. **Netlify**
   - Drag & drop dist folder
   - Configure redirects
   - Custom domain

3. **GitHub Pages**
   - Build to docs folder
   - Enable in repo settings

### Post-Deployment
- Verify site on production URL
- Test on real mobile devices
- Submit sitemap to Google
- Set up analytics (optional)

---

## 🔄 Maintenance & Updates

### Regular Updates
**Monthly:**
- Update barber information if needed
- Check and update Booksy links
- Refresh product showcase

**Quarterly:**
- Replace placeholder images with new photos
- Review and update content
- Performance audit
- Security updates for dependencies

**Yearly:**
- Design refresh if needed
- Major content overhaul
- Technology stack updates

### Using the Skill for Updates
```bash
# Example: Add a new barber
"Use privatestudio-manager to add a new barber named Juan
with specialty 'Fade Expert' and Booksy link [url]"

# Example: Update products
"Update the STMNT products section to include 3 new items"

# Example: Change hero text
"Modify the hero section to say 'TRANSFORM YOUR LOOK'"
```

---

## 📖 Learning Resources

### Dan's Principles Applied
1. **Prompts are Primitives**: Each slash command is a focused, reusable prompt
2. **Skills Manage, Don't Replace**: The skill coordinates slash commands
3. **Composition Over Creation**: Build up from small pieces
4. **Agent-First Thinking**: Let the skill decide which commands to use

### Key Insights for This Project
- We don't need sub-agents (no parallelization needed)
- We don't need MCP servers (no external data)
- Slash commands give us maximum flexibility
- The skill makes future updates trivial

### Reference Materials
- IndyDevDan's video analysis: `knowledge/skills-indydevdan-summary.md`
- AMIRO Barbershop: https://www.amirobarbershops.com/
- Barber Barcelona: https://www.barberbarcelona.es/
- Astro Docs: https://docs.astro.build
- GSAP Docs: https://greensock.com/docs/

---

## 🎯 Success Metrics

### Technical Metrics
- ✅ All 13 slash commands functional
- ✅ 1 management skill operational
- ✅ Lighthouse performance score 90+
- ✅ Mobile responsive 320px - 1440px+
- ✅ Cross-browser compatible
- ✅ 60fps animations

### Design Metrics
- ✅ Matches AMIRO design inspiration
- ✅ HYPER BIG typography implemented
- ✅ Horizontal scroll effect working
- ✅ Dark theme consistent
- ✅ Text superposition effects

### Business Metrics
- ✅ 4 barbers with Booksy integration
- ✅ STMNT products showcased
- ✅ Contact information accurate
- ✅ Mobile-friendly booking flow
- ✅ Professional appearance

---

## 🎬 Next Steps

### Immediate (Today)
1. Review this plan
2. Approve approach
3. Begin Phase 1: Create slash commands

### This Week
1. Complete all 13 slash commands
2. Build all website sections
3. Test responsive design

### Next Week
1. Create management skill
2. Replace placeholder images with real photos
3. Final polish and deployment

---

## 📝 Notes & Considerations

### Design Decisions
- Using GSAP for horizontal scroll (more performant than pure CSS)
- Astro for SSG (better performance than client-side React)
- Tailwind for utility classes (faster development)
- Mobile-first approach (majority of barbershop traffic)

### Future Enhancements
- [ ] Online booking form (instead of Booksy redirect)
- [ ] Gallery section with before/after photos
- [ ] Blog section for hair care tips
- [ ] Multi-language support (ES/EN/CA)
- [ ] Dark/Light theme toggle
- [ ] Appointment availability calendar
- [ ] Customer reviews section

### Potential Challenges
1. **Horizontal scroll performance on mobile**: May need to disable or simplify
2. **HYPER BIG typography**: Test loading times with web fonts
3. **Image optimization**: Ensure placeholders are properly sized
4. **Booksy links**: Verify they work before deployment

---

## ✅ Approval & Sign-Off

**Plan Status:** ⏳ Awaiting Approval

**Prepared By:** Claude Code (Sonnet 4.5)
**Date:** October 28, 2025
**Estimated Time:** 4 hours
**Approach:** Full Dan Hierarchy (Option A)

**Ready to Proceed:** Yes ✅

---

*This plan follows IndyDevDan's best practices for Claude Code development, emphasizing slash commands as primitives, skills for management, and proper composition hierarchy.*

# AMIRO BARBERSHOP - QUICK REFERENCE GUIDE

Use this as a quick lookup while building. See [AMIRO_STRUCTURE_ANALYSIS.md](./AMIRO_STRUCTURE_ANALYSIS.md) for complete details.

---

## 10-SECTION LAYOUT (Top to Bottom)

1. **Sticky Header** - Logo center, social left (desktop), CTAs right
2. **Hero** - Full viewport, heading centered, CTA bottom-right (desktop) or top (mobile)
3. **Marquee** - "| barbershop | amiro" infinite scroll left
4. **Services Intro** - Two columns (image left, sticky heading + image right)
5. **Service Grid** - Repeating: Heading → Image → CTA "Book >"
6. **"We Don't Do Normal"** - Brand statement section
7. **Team** - 5 members: luca, adrian, jacopo, nicholas, DELIA
8. **About** - 4 subsections about friendship/craftsmanship
9. **Contact** - Multi-section contact info
10. **Footer** - Social, hours, contact, copyright

---

## KEY MEASUREMENTS

| Element | Desktop | Mobile |
|---------|---------|--------|
| Hero CTAs | Bottom right | Top center (two buttons) |
| Sticky Header | 0px offset | 0px offset |
| Sticky Heading | 150px offset | Hidden |
| Service Images | Portrait (777x1024) | Responsive |
| Marquee Speed | 4px/unit | 4px/unit |

---

## CRITICAL CSS CLASSES

```css
.bottone-scritta         /* Brand button style - USE EVERYWHERE */
.animated-slow           /* Slower animation timing */
.elementor-invisible     /* Hidden until animation triggers */
.elementor-hidden-mobile /* Hide on mobile only */
.elementor-hidden-tablet /* Hide on tablet only */
.elementor-hidden-desktop /* Hide on desktop only */
```

---

## ANIMATION QUICK REF

| Element | Animation | Trigger |
|---------|-----------|---------|
| Marquee | translateX(-50%) | Always on |
| Service CTAs | fadeInLeft | On scroll (10% visible) |
| Sticky Header | position: sticky | On scroll |
| Services Heading | position: sticky | On scroll (150px) |

---

## RESPONSIVE BREAKPOINTS

### Desktop
- Social icons visible in header
- Two CTAs in header right
- Hero CTA bottom-right
- Two-column services intro
- Sticky heading visible

### Mobile
- Hamburger menu replaces CTAs
- Hero CTAs at top (two buttons)
- Single column everywhere
- Sticky heading hidden
- Stacked service items

---

## CTA PLACEMENT MAP

```
Header:     [Social] [Logo] [call us] [Book now]

Hero:       Mobile: [Book now] [call us] (top)
            Desktop: [Schedule your haircut today >] (bottom-right)

Services:   Every item: [Book >] (left, fadeInLeft)

Footer:     [Social links] (Instagram, TikTok)
```

---

## COLOR & TYPOGRAPHY NOTES

- **Heading Style:** Mostly lowercase, bold
- **Button Style:** `bottone-scritta` class (brand-specific)
- **Typography:** Clean, modern, readable
- **Whitespace:** Generous spacing throughout

---

## CONTENT CHECKLIST

### Team Members (5)
- [ ] luca
- [ ] adrian
- [ ] jacopo
- [ ] nicholas
- [ ] DELIA

### About Subsections (4)
- [ ] A Dream Born from Friendship
- [ ] If We don't Look Good, You don't Look Good!
- [ ] BUILT ON FRIENDSHIP, CRAFTED WITH CARE.
- [ ] A place where Friendship, Craftsmanship, and Style meet

### Social Links (2)
- [ ] Instagram: https://www.instagram.com/amirobarbershops/
- [ ] TikTok: https://www.tiktok.com/@amiro.barbershops

---

## BUILD ORDER

1. Set up sticky header structure
2. Create hero with responsive CTA positioning
3. Implement marquee animation
4. Build services intro (two-column)
5. Create service grid pattern
6. Add team section grid
7. Build about section
8. Add contact/footer
9. Apply `bottone-scritta` styling
10. Add scroll animations
11. Test responsive breakpoints

---

## COMMON MISTAKES TO AVOID

- Forgetting `bottone-scritta` class on CTAs
- Wrong CTA placement (mobile vs desktop)
- Marquee not infinite loop
- Missing sticky offset (150px) on services heading
- Wrong image aspect ratios
- Not hiding social icons on mobile
- Service CTAs not using fadeInLeft

---

**Quick Ref Complete** - Bookmark this page!

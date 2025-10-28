# Test Cross-Browser Compatibility

Test the site in multiple browsers and report issues.

## Task
Verify the site works correctly in all major browsers.

## Browsers to Test
- [ ] **Chrome** (latest)
- [ ] **Firefox** (latest)
- [ ] **Safari** (latest, macOS/iOS)
- [ ] **Edge** (latest)
- [ ] **Mobile Safari** (iOS)
- [ ] **Chrome Mobile** (Android)

## What to Check in Each Browser

### Layout & Design
- [ ] Fonts render correctly
- [ ] Colors display accurately
- [ ] Spacing/alignment consistent
- [ ] Grid layouts work
- [ ] Flexbox layouts work

### Functionality
- [ ] Navigation works (hamburger menu)
- [ ] Smooth scroll works
- [ ] Hover effects work
- [ ] Click/tap targets work
- [ ] Links work (especially Booksy)

### Animations
- [ ] GSAP animations work
- [ ] Horizontal scroll effect works
- [ ] CSS animations play smoothly
- [ ] No animation jank (60fps)

### Performance
- [ ] Page loads quickly
- [ ] Images load properly
- [ ] No console errors
- [ ] No layout shifts (CLS)

## Common Browser-Specific Issues
- **Safari:** CSS Grid gaps, backdrop-filter, smooth scroll
- **Firefox:** Flexbox bugs, font rendering differences
- **Edge:** Legacy compatibility issues
- **Mobile Safari:** Viewport height (vh units), input zoom

## Report Format
For each issue:
- Browser name and version
- Issue description
- Steps to reproduce
- Screenshot if applicable
- Suggested fix

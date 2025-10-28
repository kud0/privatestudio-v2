# Test Accessibility

Check WCAG compliance and accessibility issues.

## Task
Test the site for accessibility compliance (WCAG AA standard).

## What to Check

### Keyboard Navigation
- [ ] Tab through all interactive elements
- [ ] Tab order is logical
- [ ] Focus indicators are visible
- [ ] No keyboard traps
- [ ] Skip to main content link works

### Screen Reader
- [ ] All images have descriptive alt text
- [ ] Headings are hierarchical (h1 → h2 → h3)
- [ ] Landmark regions defined (nav, main, footer)
- [ ] Form inputs have labels
- [ ] Links have descriptive text (not "click here")

### Color Contrast
- [ ] Text contrast ≥ 4.5:1 (normal text)
- [ ] Text contrast ≥ 3:1 (large text 24px+)
- [ ] Test with black background and white text

### Interactive Elements
- [ ] Buttons clearly labeled
- [ ] Touch targets ≥ 44x44px on mobile
- [ ] Error messages are clear and helpful
- [ ] Success states are announced

### Media
- [ ] Videos have captions
- [ ] Audio has transcripts
- [ ] Animations respect prefers-reduced-motion

## Tools
```bash
# Use axe DevTools browser extension
# Or run automated tests:
npm install -D @axe-core/cli
npx axe http://localhost:4321
```

## Report Format
List all issues found with:
- Severity (critical, serious, moderate, minor)
- Element affected
- WCAG criterion violated
- Suggested fix

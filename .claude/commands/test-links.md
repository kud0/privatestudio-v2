# Test Links

Validate all internal and external links work.

## Task
Check that all links on the site are functional and go to the correct destinations.

## Links to Test

### Navigation Links
- [ ] All nav menu items work
- [ ] Logo links to homepage
- [ ] Mobile menu links work
- [ ] Smooth scroll to sections works

### Booksy Links (Critical)
- [ ] All 4 barber Booksy links work
- [ ] Links open in new tab (target="_blank")
- [ ] Links have rel="noopener noreferrer"

### Social Media Links
- [ ] Instagram link works
- [ ] Facebook link works
- [ ] Links open in new tab

### Contact Links
- [ ] Phone link opens dialer (tel:)
- [ ] Email link opens mail client (mailto:)
- [ ] Google Maps link/embed works

### Footer Links
- [ ] All footer links functional
- [ ] Copyright year is current

## Testing Methods
```bash
# Manual: Click every link
# Automated: Use link checker
npm install -g broken-link-checker
blc http://localhost:4321 -ro
```

## Report Format
For broken links:
- Link text
- Expected destination
- Actual destination (or error)
- Location on page

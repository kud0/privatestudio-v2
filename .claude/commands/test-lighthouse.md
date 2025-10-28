# Test Lighthouse Performance

Run Lighthouse audit and report scores.

## Task
Run Google Lighthouse audit and analyze performance, accessibility, best practices, and SEO.

## How to Run
```bash
# In Chrome DevTools:
# 1. Open Chrome DevTools (F12)
# 2. Go to "Lighthouse" tab
# 3. Select: Mobile or Desktop
# 4. Check all categories
# 5. Click "Analyze page load"
```

Or use CLI:
```bash
npx lighthouse http://localhost:4321 --view
```

## Target Scores
- **Performance:** 90+ (green)
- **Accessibility:** 95+ (green)
- **Best Practices:** 100 (green)
- **SEO:** 100 (green)

## What to Report
1. **Scores:** All four category scores
2. **Opportunities:** Top 3 improvements that would increase performance
3. **Diagnostics:** Any warnings or errors
4. **Failed Audits:** Accessibility or best practice issues

## Common Issues & Fixes
- **Low Performance:** Images too large (optimize/compress)
- **Accessibility:** Missing alt text, low contrast, missing ARIA labels
- **Best Practices:** HTTPS, console errors, deprecated APIs
- **SEO:** Missing meta descriptions, unreadable font sizes

## Report Only
This command reports findings. Use other commands to fix issues.

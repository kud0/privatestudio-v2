# PrivateStudio Website Manager

Manages the PrivateStudio barbershop website sections, content updates, and maintenance.

## Purpose
This skill coordinates all website operations for PrivateStudio, from initial build to ongoing content updates. It uses slash commands as primitives and composes them into coherent workflows following IndyDevDan's hierarchy principles.

## When to use this skill
The agent automatically invokes this skill when:
- User mentions "PrivateStudio website"
- User wants to build or modify the website
- User requests "update website" operations
- User mentions barbers, products, or site content
- User asks to implement design features from AMIRO or Barber Barcelona

## Available Operations

### Initial Setup
- **setup-project**: Install dependencies and configure Astro
- **build-foundation**: Create base layout, navigation, and footer
- **setup-design-system**: Create design tokens and animations

### Build Sections
- **build-hero**: Create hero section with HYPER BIG typography
- **build-horizontal-scroll**: Implement horizontal scroll effect
- **build-team**: Create team section with 4 barbers
- **build-products**: Create STMNT products showcase
- **build-about**: Create about section with text effects

### Content Management
- **update-hero**: Modify hero section text and CTAs
- **add-barber**: Add new barber profile with Booksy link
- **remove-barber**: Remove a barber from team section
- **update-barber**: Modify existing barber information
- **update-products**: Add/remove/modify STMNT products
- **update-footer**: Change hours, contact info, social links

### Design Updates
- **update-colors**: Modify color scheme in design tokens
- **update-typography**: Adjust font sizes and styles
- **update-animations**: Modify or add new effects
- **update-spacing**: Adjust spacing scale

### Asset Management
- **extract-images**: Download placeholder images
- **replace-images**: Swap placeholder images with real photos
- **optimize-images**: Run image optimization

### Testing & QA
- **test-site**: Run comprehensive QA tests (spawns qa-tester sub-agent)
- **test-responsive**: Check mobile/tablet/desktop layouts
- **test-performance**: Run Lighthouse audit
- **test-accessibility**: Check WCAG compliance
- **test-links**: Validate all links work
- **fix-issues**: Address problems found in testing

## Internal Slash Commands Used

This skill composes these primitive slash commands:

**Building Commands:**
- `/install-dependencies` - Install Tailwind, GSAP, dependencies
- `/create-base-layout` - Base Astro layout structure
- `/create-nav` - Responsive navigation component
- `/create-footer` - Footer with contact information
- `/create-hero` - Hero with HYPER BIG typography
- `/create-horizontal-scroll` - Horizontal scroll effect
- `/create-team-section` - Team showcase with Booksy links
- `/create-products-section` - STMNT products grid
- `/create-about-section` - About with text superposition
- `/create-design-tokens` - CSS custom properties
- `/create-animations` - Animation keyframes and utilities
- `/extract-images` - Image placeholder system

**Modification Commands:**
- `/update-hero-text` - Change hero headline and CTA
- `/update-colors` - Modify color scheme
- `/update-typography` - Change font sizes/weights
- `/update-spacing` - Adjust padding/margins
- `/update-animations` - Modify animation settings
- `/replace-images` - Swap placeholder images
- `/update-barber-info` - Modify team member details
- `/update-products` - Change product information
- `/update-footer-info` - Update contact details
- `/fix-responsive-issue` - Fix mobile/tablet problems

**Testing Commands:**
- `/test-responsive` - Test all breakpoints
- `/test-lighthouse` - Performance audit
- `/test-accessibility` - WCAG compliance check
- `/test-links` - Validate links
- `/optimize-images` - Image compression
- `/test-cross-browser` - Browser compatibility

**Sub-Agents:**
- `qa-tester` - Runs all tests in parallel, provides comprehensive report

## Workflow Patterns

### Pattern 1: Initial Build
When user says "build the PrivateStudio website":

1. Run `/install-dependencies` to set up tools
2. Run `/create-design-tokens` for design system
3. Run `/create-animations` for animation utilities
4. Run `/create-base-layout` for HTML structure
5. Run `/create-nav` for navigation
6. Run `/create-footer` for footer
7. Run `/create-hero` for hero section
8. Run `/create-horizontal-scroll` for scroll effect
9. Run `/create-team-section` for barbers
10. Run `/create-products-section` for STMNT
11. Run `/create-about-section` for about
12. Run `/extract-images` for placeholders
13. Create main index.astro page importing all components
14. Test responsive design and animations

### Pattern 2: Add New Barber
When user says "add a new barber named [Name]":

1. Read current team section
2. Add new barber object with:
   - Name
   - Specialty
   - Placeholder image path
   - Booksy link (ask user if not provided)
3. Update team grid layout if needed (4→5 barbers = adjust grid)
4. Regenerate team section component
5. Test responsive layout

### Pattern 3: Update Design Tokens
When user says "change the accent color to [color]":

1. Read `public/styles/design-tokens.css`
2. Update `--color-accent-bronze` variable
3. Verify color contrast for accessibility
4. Test all components using accent color
5. Report changes made

### Pattern 4: Replace Placeholder Images
When user says "replace barber photos with real images":

1. Identify which images to replace
2. Verify new images exist in correct location
3. Check image dimensions match requirements (400x400px for barbers)
4. Update image paths in team component
5. Test image loading and performance
6. Recommend WebP format if not already used

### Pattern 5: Build Complete Site
When user says "build the complete PrivateStudio website":

Execute full workflow:
```
1. Setup (install dependencies, create design system)
2. Layout (base layout, nav, footer)
3. Sections (hero, scroll, team, products, about)
4. Assets (extract placeholder images)
5. Integration (create index page, test everything)
```

### Pattern 6: Test Website
When user says "test the PrivateStudio website" or "run QA":

1. Spawn `qa-tester` sub-agent
2. Sub-agent runs all testing commands in parallel:
   - `/test-responsive` (all breakpoints)
   - `/test-lighthouse` (performance)
   - `/test-accessibility` (WCAG)
   - `/test-links` (all links)
   - `/optimize-images` (check sizes)
   - `/test-cross-browser` (compatibility)
3. Collect comprehensive report
4. Categorize issues by severity (critical, warning, suggestion)
5. Provide actionable recommendations with commands to fix

### Pattern 7: Fix Responsive Issues
When user says "the [component] is broken on mobile":

1. Run `/test-responsive` to identify exact breakpoint
2. Run `/fix-responsive-issue` for the specific component
3. Test again to verify fix
4. Report changes made

### Pattern 8: Update Content
When user says "change the [element] to [new content]":

1. Identify which update command to use:
   - Hero text → `/update-hero-text`
   - Colors → `/update-colors`
   - Barber info → `/update-barber-info`
   - Products → `/update-products`
   - Footer → `/update-footer-info`
2. Run the appropriate command
3. Verify changes
4. Test responsive if layout affected

## Best Practices

### Always Follow These Rules:
1. **Mobile First**: Test responsive design at 320px, 768px, 1024px, 1440px
2. **Performance**: Optimize images, use WebP, lazy loading
3. **Accessibility**: Alt text for images, ARIA labels, keyboard navigation
4. **Design Tokens**: Never hardcode colors/spacing, always use CSS variables
5. **Component Isolation**: Each component should be self-contained
6. **Booksy Links**: Always verify links are formatted correctly
7. **GSAP Performance**: Use will-change and transform3d for animations
8. **Dark Theme**: Maintain consistent dark aesthetic throughout

### Code Quality:
- Use semantic HTML
- Follow Astro component patterns
- Keep components under 200 lines
- Document complex logic
- Use TypeScript for type safety

### Testing Checklist:
- [ ] Mobile responsive (all breakpoints)
- [ ] Cross-browser (Chrome, Firefox, Safari, Edge)
- [ ] Lighthouse score 90+ performance
- [ ] All links work (Booksy, social, internal)
- [ ] Images load correctly
- [ ] Animations run at 60fps
- [ ] Accessible (keyboard navigation, screen readers)

## Design Specifications Reference

### Inspiration Sources:
- **Primary**: AMIRO Barbershops (https://www.amirobarbershops.com/)
  - Horizontal scroll on vertical scroll
  - HYPER BIG typography (180px+)
  - Dark theme with sophisticated animations

- **Secondary**: Barber Barcelona (https://www.barberbarcelona.es/)
  - STMNT product section layout
  - Footer structure and content

### Key Design Elements:
- **Typography**: Display headlines at 180px (desktop), 60px (mobile)
- **Colors**: Black (#000000) background, off-white (#E2E0DF) text, bronze (#968C8C) accent
- **Effects**: Horizontal scroll, parallax, text superposition, hover animations
- **Layout**: Mobile-first, responsive grid, max-width 1400px

### Technical Stack:
- Astro 4.13 (SSG)
- Tailwind CSS (utility styling)
- GSAP (advanced animations)
- CSS custom properties (design tokens)

## Resources & Documentation

### Internal Documentation:
- Implementation Plan: `docs/IMPLEMENTATION_PLAN.md`
- Image Replacement Guide: `public/images/placeholders/README.md`
- Design Tokens: `public/styles/design-tokens.css`
- Animations: `public/styles/animations.css`

### External Resources:
- Astro Docs: https://docs.astro.build
- GSAP Docs: https://greensock.com/docs/
- Tailwind Docs: https://tailwindcss.com/docs

### Contact Information (for footer):
**PrivateStudio**
- Location: Carrer de Muntaner, 172, BAJO 01, 08036 Barcelona
- Hours: Mon-Fri 10:00-20:00, Sat 10:00-15:00, Closed Sunday
- Phone: +34 624 367 153
- Email: privatestudio2@gmail.com
- Instagram: @private.studiobcn
- Facebook: PrivateStudioBcn

## Booksy Integration

### Placeholder Links (update with real links):
```
Barber 1: https://booksy.com/privatestudio/barber1
Barber 2: https://booksy.com/privatestudio/barber2
Barber 3: https://booksy.com/privatestudio/barber3
Barber 4: https://booksy.com/privatestudio/barber4
```

When adding barbers, always ask for their Booksy link if not provided.

## Error Handling

If a command fails:
1. Report the error clearly
2. Suggest fixes
3. Don't proceed to dependent steps
4. Ask user if they want to retry or skip

## Success Criteria

A successful operation means:
- ✅ All files created/updated without errors
- ✅ Code passes linting and type checking
- ✅ Site builds successfully (`npm run build`)
- ✅ Visual output matches design inspiration
- ✅ Responsive across all breakpoints
- ✅ Animations perform at 60fps
- ✅ Lighthouse performance score 90+

## Communication Style

When responding:
- Be concise and clear
- Report what you're doing (which commands you're running)
- Show progress (e.g., "Creating hero section... ✓")
- Highlight any decisions made
- Ask for clarification only when truly needed
- Provide next steps or recommendations

---

**Remember**: This skill follows IndyDevDan's principle: "Skills manage, don't replace." Each slash command is a primitive that does one thing well. This skill coordinates them into complete workflows.

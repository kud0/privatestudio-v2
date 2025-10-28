# Install Dependencies

Install all required packages for the PrivateStudio website: Tailwind CSS, GSAP, and image optimization tools.

## Task
Install the following dependencies for Astro 4.13:
1. @astrojs/tailwind - Tailwind CSS integration
2. tailwindcss - Utility-first CSS framework
3. gsap - GreenSock Animation Platform for advanced animations
4. @astrojs/image - Image optimization (if not already included)

## Commands to run
```bash
npm install @astrojs/tailwind tailwindcss gsap
npx astro add tailwind
```

## Verification
- Check that tailwind.config.cjs is created
- Verify package.json includes all dependencies
- Test dev server starts without errors

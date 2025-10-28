# AMIRO BARBERSHOP - HTML STRUCTURE PATTERNS

Exact HTML/component patterns extracted from the live site.

---

## HEADER STRUCTURE

```html
<header class="sticky-header">
  <div class="header-container flex">
    <!-- Left: Social Icons (Desktop only) -->
    <div class="social-icons hidden-mobile">
      <a href="https://www.instagram.com/amirobarbershops/" aria-label="Instagram">
        <i class="instagram-icon"></i>
      </a>
      <a href="https://www.tiktok.com/@amiro.barbershops" aria-label="TikTok">
        <i class="tiktok-icon"></i>
      </a>
    </div>
    
    <!-- Center: Logo -->
    <div class="logo-container">
      <a href="/">
        <img src="logo.svg" alt="Amiro Barbershop" />
      </a>
    </div>
    
    <!-- Right: CTAs (Desktop) / Menu (Mobile) -->
    <div class="header-right">
      <button class="bottone-scritta hidden-mobile">call us</button>
      <button class="bottone-scritta hidden-mobile">Book now</button>
      <button class="hamburger-menu hidden-desktop">
        <span></span>
        <span></span>
        <span></span>
      </button>
    </div>
  </div>
</header>
```

---

## HERO SECTION

```html
<section class="hero-section">
  <!-- Mobile CTAs (Top) -->
  <div class="hero-ctas-mobile hidden-desktop">
    <button class="bottone-scritta">Book now</button>
    <button class="bottone-scritta">call us</button>
  </div>
  
  <!-- Main Heading -->
  <div class="hero-content">
    <h1>
      book<br>
      now
    </h1>
  </div>
  
  <!-- Desktop CTA (Bottom Right) -->
  <div class="hero-cta-desktop hidden-tablet hidden-mobile">
    <button class="bottone-scritta">
      Schedule your <br>haircut today &gt;
    </button>
  </div>
</section>
```

**CSS Positioning:**
```css
.hero-section {
  position: relative;
  height: 100vh;
  background-image: url('hero-bg.jpg');
  background-size: cover;
  background-position: center;
}

.hero-ctas-mobile {
  position: absolute;
  top: 20px;
  left: 50%;
  transform: translateX(-50%);
  display: flex;
  gap: 1rem;
}

.hero-content {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  text-align: center;
}

.hero-cta-desktop {
  position: absolute;
  bottom: 40px;
  right: 40px;
}
```

---

## SCROLLING MARQUEE

```html
<section class="marquee-section">
  <div class="marquee-container">
    <ul class="marquee-list">
      <li>| barbershop</li>
      <li>| amiro</li>
      <li>| barbershop</li>
      <li>| amiro</li>
      <li>| barbershop</li>
      <li>| amiro</li>
      <li>| barbershop</li>
      <li>| amiro</li>
      <li>| barbershop</li>
      <li>| amiro</li>
      <!-- Duplicate for seamless loop -->
      <li>| barbershop</li>
      <li>| amiro</li>
      <li>| barbershop</li>
      <li>| amiro</li>
      <li>| barbershop</li>
      <li>| amiro</li>
      <li>| barbershop</li>
      <li>| amiro</li>
      <li>| barbershop</li>
      <li>| amiro</li>
    </ul>
  </div>
</section>
```

**CSS Animation:**
```css
.marquee-section {
  width: 100vw;
  overflow: hidden;
  background: [your-color];
  padding: 2rem 0;
}

.marquee-list {
  display: inline-flex;
  list-style: none;
  white-space: nowrap;
  animation: marquee-scroll 30s linear infinite;
}

.marquee-list li {
  padding: 0 2rem;
  font-size: 2rem;
  font-weight: bold;
}

@keyframes marquee-scroll {
  0% {
    transform: translateX(0);
  }
  100% {
    transform: translateX(-50%);
  }
}
```

---

## SERVICES INTRODUCTION (Two-Column)

```html
<section class="services-intro">
  <div class="services-grid">
    <!-- Left Column: Image -->
    <div class="services-left">
      <img src="image-for-export-777x1024.jpg" alt="Barber at work" />
    </div>
    
    <!-- Right Column: Sticky Heading + Image -->
    <div class="services-right">
      <h2 class="sticky-heading hidden-mobile">
        humans in mirror are braver than they appear
      </h2>
      <img src="image-for-exports-777x1024.jpg" alt="Barbershop interior" />
    </div>
  </div>
</section>
```

**CSS Positioning:**
```css
.services-intro {
  padding: 4rem 0;
}

.services-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 2rem;
  max-width: 1200px;
  margin: 0 auto;
}

.sticky-heading {
  position: sticky;
  top: 150px;
  font-size: 3rem;
  margin-bottom: 2rem;
}

/* Mobile */
@media (max-width: 768px) {
  .services-grid {
    grid-template-columns: 1fr;
  }
}
```

---

## SERVICE ITEM (Repeating Pattern)

```html
<div class="service-item">
  <!-- Heading -->
  <div class="service-heading">
    <h3>luca</h3>
  </div>
  
  <!-- Image (Boxed) -->
  <div class="service-image-container">
    <img src="service-image.jpg" alt="Service preview" />
  </div>
  
  <!-- Description -->
  <div class="service-description">
    <p>Service details and description...</p>
  </div>
  
  <!-- CTA Button -->
  <div class="service-cta elementor-invisible" data-animation="fadeInLeft">
    <button class="bottone-scritta">Book &gt;</button>
  </div>
</div>
```

**Animation JS (Intersection Observer):**
```javascript
const serviceButtons = document.querySelectorAll('.service-cta');

const observer = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      entry.target.classList.add('animate-fadeInLeft');
      entry.target.classList.remove('elementor-invisible');
    }
  });
}, {
  threshold: 0.1
});

serviceButtons.forEach(button => observer.observe(button));
```

**CSS Animation:**
```css
.elementor-invisible {
  opacity: 0;
}

@keyframes fadeInLeft {
  from {
    opacity: 0;
    transform: translateX(-20px);
  }
  to {
    opacity: 1;
    transform: translateX(0);
  }
}

.animate-fadeInLeft {
  animation: fadeInLeft 0.8s ease-out forwards;
}

.animated-slow {
  animation-duration: 1.2s;
}
```

---

## TEAM MEMBER CARD

```html
<div class="team-grid">
  <div class="team-member">
    <img src="luca.jpg" alt="Luca" />
    <h2>luca</h2>
  </div>
  
  <div class="team-member">
    <img src="adrian.jpg" alt="Adrian" />
    <h2>adrian</h2>
  </div>
  
  <div class="team-member">
    <img src="jacopo.jpg" alt="Jacopo" />
    <h2>jacopo</h2>
  </div>
  
  <div class="team-member">
    <img src="nicholas.jpg" alt="Nicholas" />
    <h2>nicholas</h2>
  </div>
  
  <div class="team-member">
    <img src="delia.jpg" alt="Delia" />
    <h2>DELIA</h2>
  </div>
</div>
```

**CSS Grid:**
```css
.team-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 2rem;
  padding: 4rem 2rem;
}

.team-member {
  text-align: center;
}

.team-member img {
  width: 100%;
  aspect-ratio: 1 / 1;
  object-fit: cover;
  border-radius: 8px;
  margin-bottom: 1rem;
}

.team-member h2 {
  font-size: 1.5rem;
  text-transform: lowercase;
}
```

---

## ABOUT SECTION

```html
<section class="about-section">
  <h3 class="section-title">about</h3>
  
  <div class="about-content">
    <div class="about-subsection">
      <h3>A Dream Born from Friendship</h3>
      <p>Content about the origin story...</p>
    </div>
    
    <div class="about-subsection">
      <h3>If We don't Look Good, You don't Look Good!</h3>
      <p>Philosophy content...</p>
    </div>
    
    <div class="about-subsection">
      <h3>BUILT ON FRIENDSHIP, CRAFTED WITH CARE.</h3>
      <p>Values content...</p>
    </div>
    
    <div class="about-subsection">
      <h3>A place where Friendship, Craftsmanship, and Style meet</h3>
      <p>Mission content...</p>
    </div>
  </div>
</section>
```

---

## FOOTER / CONTACT SECTION

```html
<footer class="footer-section">
  <div class="footer-grid">
    <!-- Follow Us -->
    <div class="footer-column">
      <h2>follow us</h2>
      <div class="social-links">
        <a href="https://www.instagram.com/amirobarbershops/">Instagram</a>
        <a href="https://www.tiktok.com/@amiro.barbershops">TikTok</a>
      </div>
    </div>
    
    <!-- Opening Hours -->
    <div class="footer-column">
      <h4>OPENING HOURS</h4>
      <ul class="hours-list">
        <li>Monday - Friday: 9am - 8pm</li>
        <li>Saturday: 9am - 6pm</li>
        <li>Sunday: Closed</li>
      </ul>
    </div>
    
    <!-- Contact Us -->
    <div class="footer-column">
      <h4>contact Us</h4>
      <ul class="contact-list">
        <li><a href="tel:+1234567890">Phone</a></li>
        <li><a href="mailto:info@amiro.com">Email</a></li>
        <li>Address line</li>
      </ul>
    </div>
  </div>
  
  <!-- Footer Bottom -->
  <div class="footer-bottom">
    <p>Copyright © 2025 amiro barbershop | All rights reserved.</p>
    <a href="/privacy-policy">Privacy Policy</a>
  </div>
</footer>
```

**CSS Grid:**
```css
.footer-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 3rem;
  padding: 4rem 2rem;
  max-width: 1200px;
  margin: 0 auto;
}

.footer-bottom {
  text-align: center;
  padding: 2rem;
  border-top: 1px solid #ccc;
}

/* Mobile */
@media (max-width: 768px) {
  .footer-grid {
    grid-template-columns: 1fr;
  }
}
```

---

## BUTTON CLASS: bottone-scritta

This is the brand's signature button style used throughout.

```html
<button class="bottone-scritta">Button Text</button>
```

**Example CSS (customize to match brand):**
```css
.bottone-scritta {
  padding: 1rem 2rem;
  font-size: 1rem;
  font-weight: 600;
  text-transform: lowercase;
  background: transparent;
  border: 2px solid currentColor;
  color: inherit;
  cursor: pointer;
  transition: all 0.3s ease;
}

.bottone-scritta:hover {
  background: currentColor;
  color: white;
  transform: translateY(-2px);
}
```

---

## RESPONSIVE UTILITY CLASSES

```css
/* Hide on specific devices */
.hidden-mobile {
  display: block;
}

.hidden-tablet {
  display: block;
}

.hidden-desktop {
  display: none;
}

@media (max-width: 1024px) {
  .hidden-tablet {
    display: none !important;
  }
}

@media (max-width: 768px) {
  .hidden-mobile {
    display: none !important;
  }
  
  .hidden-desktop {
    display: block;
  }
}
```

---

## COMPLETE PAGE STRUCTURE

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Amiro Barbershop</title>
  <link rel="stylesheet" href="styles.css">
</head>
<body>
  <!-- 1. Header -->
  <header class="sticky-header">...</header>
  
  <!-- 2. Hero -->
  <section class="hero-section">...</section>
  
  <!-- 3. Marquee -->
  <section class="marquee-section">...</section>
  
  <!-- 4. Services Intro -->
  <section class="services-intro">...</section>
  
  <!-- 5. Service Items -->
  <section class="service-items">
    <div class="service-item">...</div>
    <div class="service-item">...</div>
    <!-- Repeat -->
  </section>
  
  <!-- 6. We Don't Do Normal -->
  <section class="wddn-section">...</section>
  
  <!-- 7. Team -->
  <section class="team-section">
    <h3>team</h3>
    <div class="team-grid">...</div>
  </section>
  
  <!-- 8. About -->
  <section class="about-section">...</section>
  
  <!-- 9. Contact -->
  <section class="contact-section">...</section>
  
  <!-- 10. Footer -->
  <footer class="footer-section">...</footer>
  
  <script src="animations.js"></script>
</body>
</html>
```

---

**HTML Patterns Complete** - Copy and adapt these patterns for your rebuild!

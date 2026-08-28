# 🎯 Final Project — "Portfolio Landing Page"
### Course: CSS Fundamentals to Advanced (Sessions 1–8)

---

## 📌 Project Brief

You will build a **fully responsive one-page personal portfolio website** using only **HTML + CSS** (no frameworks like Bootstrap/Tailwind, no JavaScript required). This project brings together everything covered in Sessions 1–8: selectors, box model, typography, positioning, pseudo-classes, variables, Flexbox, Grid, transitions/animations, and responsive design.

**Deadline:** Set by instructor
**Submission format:** A folder containing `index.html`, `style.css`, and any images used.

---

## 🎨 Required Color Palette (CSS Variables)

Students **must** define these as CSS custom properties in `:root` and use them throughout the project (no hard-coded hex values elsewhere in the stylesheet):

```css
:root {
  --color-primary: #2563EB;      /* Royal Blue - buttons, links, highlights */
  --color-primary-dark: #1E40AF; /* Hover states */
  --color-secondary: #F59E0B;    /* Amber - accents, call-to-action */
  --color-bg: #F9FAFB;           /* Light background */
  --color-bg-dark: #111827;      /* Dark section background (footer/hero) */
  --color-text: #1F2937;         /* Main body text */
  --color-text-light: #6B7280;   /* Secondary/muted text */
  --color-white: #FFFFFF;
  --color-border: #E5E7EB;

  --font-heading: 'Poppins', sans-serif;
  --font-body: 'Inter', sans-serif;

  --radius-sm: 4px;
  --radius-md: 12px;
  --shadow-card: 0 4px 12px rgba(0, 0, 0, 0.08);
  --transition-default: all 0.3s ease-in-out;
}
```

---

## 🧩 Required Page Sections

| # | Section | Purpose |
|---|---------|---------|
| 1 | **Navbar** | Fixed/sticky top nav with logo + links (Home, About, Projects, Skills, Contact) |
| 2 | **Hero** | Full-height intro with name, tagline, CTA button, background using `--color-bg-dark` |
| 3 | **About Me** | Short bio + profile photo, two-column layout |
| 4 | **Skills** | Grid of skill cards (e.g., HTML, CSS, JS icons/labels) |
| 5 | **Projects** | Grid/Flexbox gallery of 3–6 project cards with image, title, description, hover effect |
| 6 | **Contact** | Simple styled form (name, email, message, submit button) |
| 7 | **Footer** | Dark background, social icons/links, copyright text |

---

## ✅ Technical Requirements Checklist

Students must demonstrate **at least one** implementation of each concept below inside the project:

### From Session 1–2
- [ ] External stylesheet linked properly (`<link>` in `<head>`)
- [ ] At least 3 different selector types used (class, id, descendant/combinator)
- [ ] Use of `overflow`, `text-align`, `text-transform`, and `background` properties
- [ ] Comments explaining major CSS sections

### From Session 3
- [ ] Use of at least 3 different CSS units (`px`, `%`, `rem`/`em`, `vh`/`vw`)
- [ ] `position: relative/absolute/sticky` used at least once (e.g., navbar or badge)
- [ ] `z-index` used to layer at least two overlapping elements
- [ ] `calc()` used at least once

### From Session 4
- [ ] Styled `:hover`, `:focus`, and `:nth-child()` pseudo-classes
- [ ] `::before` or `::after` pseudo-element with `content` property (e.g., decorative icon/quote mark)
- [ ] `border-radius` and `box-shadow` used on cards
- [ ] Form inputs styled (padding, border, focus state)

### From Session 5
- [ ] All colors/fonts/spacing pulled from CSS **variables** (`:root`)
- [ ] Skills or Navbar section built with **Flexbox** (`justify-content`, `align-items`, `gap`)

### From Session 6
- [ ] Projects section built with **CSS Grid** (`grid-template-columns`, `gap`)
- [ ] Use of `repeat()` and `minmax()` for responsive grid tracks

### From Session 7
- [ ] Smooth `transition` on buttons/cards (color, transform, or shadow change on hover)
- [ ] At least one `transform` (`scale`, `translateY`, or `rotate`) on hover
- [ ] A subtle `@keyframes` animation somewhere (e.g., fade-in hero text, bouncing scroll arrow)
- [ ] A gradient background used at least once

### From Session 8
- [ ] Fully **responsive**: works cleanly at 320px (mobile), 768px (tablet), and 1200px+ (desktop) using media queries
- [ ] Mobile-first approach: base styles for small screens, `min-width` media queries scale up
- [ ] At least one **attribute selector** (e.g., `input[type="email"]`) and one **relationship selector** (`>`, `+`, or `~`)
- [ ] Basic accessibility: semantic HTML tags (`<nav>`, `<section>`, `<footer>`), `alt` text on images, sufficient color contrast

---

## 📐 Suggested Breakpoints

```css
/* Mobile first - base styles apply below */

@media (min-width: 768px) {
  /* Tablet styles */
}

@media (min-width: 1200px) {
  /* Desktop styles */
}
```

---

## 🏆 Grading Rubric (100 pts)

| Criteria | Points |
|---|---|
| Correct use of CSS variables & palette | 15 |
| Layout accuracy (Flexbox + Grid usage) | 20 |
| Responsiveness across breakpoints | 20 |
| Pseudo-classes/elements & selectors | 10 |
| Transitions/Transforms/Animations | 15 |
| Code cleanliness & comments | 10 |
| Accessibility basics | 10 |

---

## 💡 Tips for Students
- Start with mobile layout first, then scale up.
- Use `gap` instead of margins inside Flexbox/Grid containers.
- Test hover/focus states with keyboard (Tab key), not just mouse.
- Validate contrast: text on `--color-bg-dark` should use `--color-white`, not `--color-text`.
- Keep the palette consistent — no new colors outside the defined variables.

---

**Instructor note:** This project is designed to be assigned after Session 8 is complete, as a comprehensive review of the entire course.

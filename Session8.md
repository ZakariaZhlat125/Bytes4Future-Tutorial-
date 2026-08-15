# Session 8: Advanced Selectors + Responsive + Final Project

## Duration Breakdown
- **1 Hour**: Theoretical Explanation + Live Coding
- **1.5 Hours**: Practical Application (Student writes code)
- **0.5 Hours**: Review, Questions, Problem Solving

---

## Part 1: Theoretical Explanation + Live Coding (1 Hour)

### Section 1: Relationship Selectors

## Descendant Selector (Space)
```css
div p {
    color: blue; /* All <p> inside <div> */
}
```

## Child Selector (>)
```css
div > p {
    color: red; /* Only direct child <p> of <div> */
}
```

## Adjacent Sibling Selector (+)
```css
h2 + p {
    color: green; /* <p> immediately after <h2> */
}
```

## General Sibling Selector (~)
```css
h2 ~ p {
    color: purple; /* All <p> after <h2> */
}
```

---

### Section 2: Attribute Selectors

## Has Attribute
```css
[target] {
    border: 2px solid blue;
}
```

## Exact Match
```css
[type="text"] {
    background-color: yellow;
}
```

## Starts With
```css
[href^="https"] {
    color: green;
}
```

## Ends With
```css
[src$=".jpg"] {
    border: 2px solid red;
}
```

## Contains
```css
[class*="button"] {
    padding: 10px;
}
```

---

### Section 3: Responsive Design

## Viewport Meta Tag
```html
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

This is essential for responsive design.

## Media Queries
```css
/* Mobile First Approach */
.container {
    width: 100%;
    padding: 10px;
}

/* Tablet */
@media (min-width: 768px) {
    .container {
        width: 750px;
        padding: 20px;
    }
}

/* Desktop */
@media (min-width: 1024px) {
    .container {
        width: 1000px;
        padding: 30px;
    }
}
```

## Max Width Media Query
```css
@media (max-width: 600px) {
    .navigation {
        flex-direction: column;
    }
}
```

## Min Width Media Query
```css
@media (min-width: 768px) {
    .sidebar {
        display: block;
    }
}
```

---

### Section 4: Mobile-First Design

## What is Mobile-First
Start designing for mobile devices first, then progressively enhance for larger screens.

## Benefits
- Faster mobile performance
- Better user experience
- Cleaner code
- Progressive enhancement

## Example
```css
/* Base styles (mobile) */
.container {
    width: 100%;
    padding: 15px;
}

/* Tablet */
@media (min-width: 768px) {
    .container {
        width: 750px;
        padding: 20px;
    }
}

/* Desktop */
@media (min-width: 1024px) {
    .container {
        width: 1000px;
        padding: 30px;
    }
}
```

---

### Section 5: Inheritance Values

## inherit
```css
.child {
    color: inherit; /* Inherit from parent */
}
```

## unset
```css
.child {
    color: unset; /* Acts as inherit if inherited, otherwise initial */
}
```

## initial
```css
.child {
    color: initial; /* Use initial CSS value */
}
```

## revert
```css
.child {
    color: revert; /* Revert to browser default */
}
```

---

### Section 6: Debugging CSS with DevTools

## Elements Panel
- View HTML structure
- Inspect elements
- See applied styles
- Modify DOM

## Styles Panel
- View all CSS rules
- Edit CSS in real-time
- Disable specific rules
- See specificity calculation

## Computed Panel
- See final computed values
- Understand inherited values
- Debug layout issues

## Common Debugging Steps
1. Inspect the element
2. Check applied styles
3. Look for crossed-out rules
4. Check specificity
5. Verify CSS file is loaded
6. Check for syntax errors

---

### Section 7: Accessibility Basics

## Color Contrast
Ensure sufficient contrast between text and background.
- WCAG AA: 4.5:1 for normal text
- WCAG AAA: 7:1 for normal text

## Focus States
```css
button:focus {
    outline: 2px solid #2563eb;
    outline-offset: 2px;
}
```

## Semantic HTML
Use proper HTML elements for accessibility.

## ARIA Attributes
```css
[aria-hidden="true"] {
    display: none;
}
```

## Font Sizes
Use relative units for text sizing.

## Alt Text
Always provide alt text for images.

---

## Part 2: Practical Application (1.5 Hours)

### Exercise 1: Advanced Selectors Practice (20 minutes)

**Task:**
Create examples using relationship and attribute selectors.

**HTML:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Advanced Selectors</title>
    <link rel="stylesheet" href="advanced-selectors.css">
</head>
<body>
    <h1>Advanced Selectors</h1>
    
    <h2>Relationship Selectors</h2>
    <div class="parent">
        <p>Direct child paragraph</p>
        <div>
            <p>Nested paragraph (not direct child)</p>
        </div>
    </div>
    
    <h2>Attribute Selectors</h2>
    <a href="https://example.com" target="_blank">External Link</a>
    <a href="page.html">Internal Link</a>
    <input type="text" placeholder="Text input">
    <input type="email" placeholder="Email input">
    <div class="button-primary">Primary Button</div>
    <div class="button-secondary">Secondary Button</div>
</body>
</html>
```

**CSS:**
```css
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    font-family: Arial, sans-serif;
    padding: 20px;
    background-color: #f3f4f6;
}

h1 {
    text-align: center;
    color: #1f2937;
    margin-bottom: 30px;
}

h2 {
    color: #1f2937;
    margin-top: 30px;
    margin-bottom: 15px;
}

/* Relationship Selectors */
.parent {
    background-color: white;
    padding: 20px;
    border-radius: 8px;
    margin-bottom: 20px;
}

.parent > p {
    color: #2563eb;
    font-weight: bold;
}

.parent div p {
    color: #4b5563;
}

/* Attribute Selectors */
[target="_blank"] {
    color: #10b981;
    font-weight: bold;
}

[type="text"] {
    border: 2px solid #2563eb;
    padding: 10px;
    border-radius: 4px;
    margin-right: 10px;
}

[type="email"] {
    border: 2px solid #f59e0b;
    padding: 10px;
    border-radius: 4px;
}

[class*="button"] {
    padding: 10px 20px;
    border-radius: 4px;
    display: inline-block;
    margin: 5px;
}

.button-primary {
    background-color: #2563eb;
    color: white;
}

.button-secondary {
    background-color: #10b981;
    color: white;
}
```

---

### Exercise 2: Responsive Navigation (25 minutes)

**Task:**
Create a responsive navigation using media queries.

**HTML:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Responsive Navigation</title>
    <link rel="stylesheet" href="responsive-nav.css">
</head>
<body>
    <nav class="navbar">
        <div class="logo">MyBrand</div>
        <ul class="nav-links">
            <li><a href="#home">Home</a></li>
            <li><a href="#about">About</a></li>
            <li><a href="#services">Services</a></li>
            <li><a href="#contact">Contact</a></li>
        </ul>
        <button class="menu-toggle">☰</button>
    </nav>
    
    <div class="content">
        <h1>Responsive Navigation</h1>
        <p>Resize the browser to see the navigation adapt.</p>
    </div>
</body>
</html>
```

**CSS:**
```css
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    font-family: Arial, sans-serif;
    background-color: #f3f4f6;
}

.navbar {
    background-color: #2563eb;
    color: white;
    padding: 15px 30px;
    display: flex;
    justify-content: space-between;
    align-items: center;
}

.logo {
    font-size: 24px;
    font-weight: bold;
}

.nav-links {
    list-style: none;
    display: flex;
    gap: 30px;
}

.nav-links a {
    color: white;
    text-decoration: none;
    font-weight: 500;
}

.menu-toggle {
    display: none;
    background: none;
    border: none;
    color: white;
    font-size: 24px;
    cursor: pointer;
}

.content {
    max-width: 800px;
    margin: 40px auto;
    padding: 20px;
    text-align: center;
}

/* Tablet and below */
@media (max-width: 768px) {
    .nav-links {
        display: none;
        flex-direction: column;
        position: absolute;
        top: 60px;
        left: 0;
        right: 0;
        background-color: #2563eb;
        padding: 20px;
        gap: 15px;
    }
    
    .nav-links.active {
        display: flex;
    }
    
    .menu-toggle {
        display: block;
    }
}

/* Mobile */
@media (max-width: 480px) {
    .navbar {
        padding: 10px 20px;
    }
    
    .logo {
        font-size: 20px;
    }
    
    .content {
        padding: 15px;
    }
}
```

---

### Exercise 3: Final Project - Responsive Product Store (45 minutes)

**Task:**
Create a complete responsive product store using all CSS concepts learned.

**HTML:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Responsive Product Store</title>
    <link rel="stylesheet" href="final-project.css">
</head>
<body>
    <header class="header">
        <div class="logo">TechStore</div>
        <nav class="nav">
            <ul class="nav-links">
                <li><a href="#home">Home</a></li>
                <li><a href="#products">Products</a></li>
                <li><a href="#about">About</a></li>
                <li><a href="#contact">Contact</a></li>
            </ul>
        </nav>
        <button class="cart-button">Cart (0)</button>
    </header>
    
    <main class="main-content">
        <section class="hero">
            <h1>Welcome to TechStore</h1>
            <p>Discover the latest technology products</p>
            <button class="cta-button">Shop Now</button>
        </section>
        
        <section class="products-section">
            <h2>Featured Products</h2>
            <div class="products-grid">
                <div class="product-card">
                    <div class="product-image">
                        <img src="https://via.placeholder.com/300" alt="Laptop">
                    </div>
                    <h3>Laptop Pro</h3>
                    <p class="price">$999.99</p>
                    <button class="add-to-cart">Add to Cart</button>
                </div>
                
                <div class="product-card">
                    <div class="product-image">
                        <img src="https://via.placeholder.com/300" alt="Headphones">
                    </div>
                    <h3>Wireless Headphones</h3>
                    <p class="price">$199.99</p>
                    <button class="add-to-cart">Add to Cart</button>
                </div>
                
                <div class="product-card">
                    <div class="product-image">
                        <img src="https://via.placeholder.com/300" alt="Smartphone">
                    </div>
                    <h3>Smartphone X</h3>
                    <p class="price">$699.99</p>
                    <button class="add-to-cart">Add to Cart</button>
                </div>
                
                <div class="product-card">
                    <div class="product-image">
                        <img src="https://via.placeholder.com/300" alt="Tablet">
                    </div>
                    <h3>Tablet Pro</h3>
                    <p class="price">$449.99</p>
                    <button class="add-to-cart">Add to Cart</button>
                </div>
            </div>
        </section>
    </main>
    
    <footer class="footer">
        <p>&copy; 2024 TechStore. All rights reserved.</p>
    </footer>
</body>
</html>
```

**CSS:**
```css
/* CSS Variables */
:root {
    --primary-color: #2563eb;
    --secondary-color: #10b981;
    --text-color: #1f2937;
    --background-color: #f3f4f6;
    --white: #ffffff;
    --spacing-sm: 10px;
    --spacing-md: 20px;
    --spacing-lg: 30px;
    --border-radius: 8px;
    --shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

/* Reset */
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    font-family: Arial, sans-serif;
    background-color: var(--background-color);
    color: var(--text-color);
    line-height: 1.6;
}

/* Header */
.header {
    background-color: var(--white);
    padding: var(--spacing-md) var(--spacing-lg);
    display: flex;
    justify-content: space-between;
    align-items: center;
    box-shadow: var(--shadow);
    position: sticky;
    top: 0;
    z-index: 1000;
}

.logo {
    font-size: 24px;
    font-weight: bold;
    color: var(--primary-color);
}

.nav-links {
    list-style: none;
    display: flex;
    gap: var(--spacing-md);
}

.nav-links a {
    color: var(--text-color);
    text-decoration: none;
    font-weight: 500;
    transition: color 0.3s;
}

.nav-links a:hover {
    color: var(--primary-color);
}

.cart-button {
    background-color: var(--primary-color);
    color: var(--white);
    border: none;
    padding: var(--spacing-sm) var(--spacing-md);
    border-radius: var(--border-radius);
    cursor: pointer;
    font-weight: 600;
    transition: background-color 0.3s;
}

.cart-button:hover {
    background-color: #1d4ed8;
}

/* Main Content */
.main-content {
    max-width: 1200px;
    margin: 0 auto;
    padding: var(--spacing-lg);
}

/* Hero Section */
.hero {
    background: linear-gradient(135deg, var(--primary-color), #7c3aed);
    color: var(--white);
    padding: 60px var(--spacing-lg);
    border-radius: var(--border-radius);
    text-align: center;
    margin-bottom: var(--spacing-lg);
}

.hero h1 {
    font-size: 48px;
    margin-bottom: var(--spacing-md);
}

.hero p {
    font-size: 18px;
    margin-bottom: var(--spacing-lg);
    opacity: 0.9;
}

.cta-button {
    background-color: var(--white);
    color: var(--primary-color);
    border: none;
    padding: var(--spacing-md) var(--spacing-lg);
    border-radius: var(--border-radius);
    font-size: 18px;
    font-weight: 600;
    cursor: pointer;
    transition: transform 0.3s, box-shadow 0.3s;
}

.cta-button:hover {
    transform: translateY(-2px);
    box-shadow: 0 6px 12px rgba(0, 0, 0, 0.2);
}

/* Products Section */
.products-section h2 {
    color: var(--text-color);
    margin-bottom: var(--spacing-lg);
    font-size: 32px;
}

.products-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
    gap: var(--spacing-md);
}

.product-card {
    background-color: var(--white);
    border-radius: var(--border-radius);
    padding: var(--spacing-md);
    box-shadow: var(--shadow);
    transition: transform 0.3s, box-shadow 0.3s;
}

.product-card:hover {
    transform: translateY(-5px);
    box-shadow: 0 8px 16px rgba(0, 0, 0, 0.15);
}

.product-image {
    margin-bottom: var(--spacing-md);
}

.product-image img {
    width: 100%;
    height: 200px;
    object-fit: cover;
    border-radius: var(--border-radius);
}

.product-card h3 {
    color: var(--text-color);
    margin-bottom: var(--spacing-sm);
    font-size: 18px;
}

.price {
    color: var(--primary-color);
    font-size: 24px;
    font-weight: bold;
    margin-bottom: var(--spacing-md);
}

.add-to-cart {
    width: 100%;
    padding: var(--spacing-sm);
    background-color: var(--primary-color);
    color: var(--white);
    border: none;
    border-radius: var(--border-radius);
    font-size: 16px;
    font-weight: 600;
    cursor: pointer;
    transition: background-color 0.3s;
}

.add-to-cart:hover {
    background-color: #1d4ed8;
}

/* Footer */
.footer {
    background-color: var(--text-color);
    color: var(--white);
    text-align: center;
    padding: var(--spacing-lg);
    margin-top: var(--spacing-lg);
}

/* Responsive Design */
@media (max-width: 768px) {
    .header {
        flex-direction: column;
        gap: var(--spacing-md);
    }
    
    .nav-links {
        flex-wrap: wrap;
        justify-content: center;
    }
    
    .hero h1 {
        font-size: 32px;
    }
    
    .products-grid {
        grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
    }
}

@media (max-width: 480px) {
    .header {
        padding: var(--spacing-md);
    }
    
    .hero {
        padding: 40px var(--spacing-md);
    }
    
    .hero h1 {
        font-size: 24px;
    }
    
    .hero p {
        font-size: 16px;
    }
    
    .main-content {
        padding: var(--spacing-md);
    }
    
    .products-grid {
        grid-template-columns: 1fr;
    }
}
```

---

## Part 3: Review, Questions, Problem Solving (0.5 Hours)

### Final Checklist (10 minutes)

## CSS Fundamentals ✅
- Box Model (padding, margin, border)
- Display property
- Position types
- CSS selectors
- CSS specificity

## Typography & Colors ✅
- Font properties
- Text styling
- Color formats (hex, rgb, rgba, hsl)
- Text alignment and spacing

## Layout Systems ✅
- Flexbox (parent and child properties)
- CSS Grid (parent and child properties)
- Responsive design principles

## Advanced Features ✅
- CSS Variables
- Transitions and animations
- Transforms (2D and 3D)
- Filters and gradients
- Pseudo-classes and pseudo-elements

## Best Practices ✅
- CSS organization
- Accessibility considerations
- Performance optimization
- Cross-browser compatibility

---

### Review Questions (10 minutes)

**Question 1:** What is the difference between `>` and space in selectors?
**Answer:** `>` selects direct children only, space selects all descendants.

**Question 2:** What does `min-width` in media queries target?
**Answer:** Screens larger than or equal to the specified width (mobile-first approach).

**Question 3:** What is the purpose of the viewport meta tag?
**Answer:** It controls how the webpage is displayed on mobile devices, essential for responsive design.

**Question 4:** What does `inherit` do in CSS?
**Answer:** Inherits the computed value of the property from the parent element.

**Question 5:** How do you debug CSS using DevTools?
**Answer:** Use the Elements panel to inspect, Styles panel to view and edit CSS, and Computed panel to see final values.

**Question 6:** What is WCAG contrast requirement for normal text?
**Answer:** 4.5:1 contrast ratio for AA compliance.

**Question 7:** What is mobile-first design?
**Answer:** Designing for mobile devices first, then progressively enhancing for larger screens.

**Question 8:** What is the difference between `initial` and `unset`?
**Answer:** `initial` uses the CSS default value, `unset` acts as inherit if the property is inherited, otherwise initial.

---

### Final Project Review (5 minutes)

**Congratulations!** You have completed the CSS Complete Course.

**Key Achievements:**
- Mastered CSS fundamentals
- Learned modern layout systems (Flexbox & Grid)
- Implemented responsive design
- Created animations and transitions
- Applied accessibility best practices
- Built a complete responsive project

**Next Steps:**
- Practice building real projects
- Learn JavaScript for interactivity
- Explore CSS frameworks (optional)
- Stay updated with CSS features
- Build a portfolio of your work

---

## End of Session 8

## 🎉 Course Completion!

**You have successfully completed the CSS Complete Course!**

**What you've learned:**
- CSS Fundamentals (Sessions 1-2)
- Advanced Layout Systems (Sessions 3-6)
- Animations and Visual Effects (Session 7)
- Responsive Design and Accessibility (Session 8)

**Keep practicing and building amazing websites!**

**Happy Coding! 🚀**
# Session 5: CSS Variables + Flexbox

## Duration Breakdown (2.5 Hours Total)
- **1.5 Hours**: Interactive Learning (All Sections 1-4 with Explain → Example → Try it yourself → Predict → Experiment → Challenge → Bug Hunting → Quiz)
- **20 Minutes**: Comprehensive Project (Single component combining all concepts)
- **10 Minutes**: Review & Assessment

---

## Part 1: Interactive Learning (1.5 Hours)

### Section 1: CSS Variables

## Explain
CSS Variables (Custom Properties) are reusable values that can be stored and used throughout your CSS:

- Must start with `--` (double hyphen)
- Case-sensitive naming
- Created with `--name: value`
- Used with `var(--name)`
- Support fallback values

## Example
```css
:root {
    --primary-color: #2563eb;
    --secondary-color: #10b981;
    --text-color: #1f2937;
    --spacing: 20px;
}

.button {
    background-color: var(--primary-color);
    color: var(--text-color);
    padding: var(--spacing);
}
```

## Try it yourself
Create CSS variables and use them.

```html
<div class="my-card">
    <h3>Card Title</h3>
    <p>Card content</p>
</div>
```

```css
:root {
    /* Define your variables */
}

.my-card {
    /* Use your variables */
}
```

## Predict
What will happen with this code?

```css
:root {
    --color: blue;
}

.element {
    color: var(--color, red);
}
```

## Experiment
1. What happens if a variable doesn't exist?
2. Can you use variables in media queries?
3. Try overriding a variable locally

## Challenge
**Task:** Create a theme system with:
- At least 5 CSS variables for colors
- Variables for spacing and typography
- Local variable override for a specific component
- Fallback values for browser support
- Time: 12 minutes
- Hint: Use `:root` for global, local scope for component-specific

<details>
<summary>Solution</summary>

```css
:root {
    --primary-color: #2563eb;
    --secondary-color: #10b981;
    --accent-color: #f59e0b;
    --text-color: #1f2937;
    --background-color: #ffffff;
    --spacing-sm: 10px;
    --spacing-md: 20px;
    --spacing-lg: 30px;
    --border-radius: 8px;
}

.card {
    --card-primary: #7c3aed;
    background: var(--background-color);
    padding: var(--spacing-lg);
    border-radius: var(--border-radius);
}

.card h3 {
    color: var(--card-primary, #6366f1); /* Fallback */
}
```

</details>

## Bug Hunting
Find the bug in this code:

```css
.buggy {
    color: var(--primary-color);
    /* Variable not defined anywhere */
}
```

<details>
<summary>Solution</summary>

The variable `--primary-color` is not defined in any scope. Need to define it in `:root` or locally.

</details>

## Quiz
1. What prefix is required for CSS variable names?
2. How do you use a CSS variable?
3. What's the purpose of fallback values?

<details>
<summary>Quiz Answers</summary>

1. CSS variables must start with `--` (double hyphen)
2. Use the `var()` function with the variable name: `var(--variable-name)`
3. Fallback values provide a default if the variable doesn't exist or isn't supported

</details>

---

### Section 2: Flexbox Introduction

## Explain
Flexbox is a CSS layout system for arranging elements in one dimension (row or column):

- `display: flex` enables flexbox layout
- Great for navigation, cards, centering
- One-dimensional layout system
- Powerful and flexible

## Example
```css
.container {
    display: flex;
    gap: 20px;
}

.item {
    padding: 20px;
    background: #2563eb;
    color: white;
}
```

## Try it yourself
Create a simple flex container.

```html
<div class="flex-container">
    <div class="item">Item 1</div>
    <div class="item">Item 2</div>
    <div class="item">Item 3</div>
</div>
```

```css
.flex-container {
    /* Enable flexbox */
}

.item {
    /* Add basic styling */
}
```

## Predict
What will happen with this code?

```css
.container {
    display: flex;
}
```

## Experiment
1. What happens when you enable flexbox on a container?
2. Try adding items to a flex container
3. How does flexbox change default block behavior?

## Challenge
**Task:** Create a navigation bar using flexbox with:
- Logo on the left
- Navigation links in the center
- Action button on the right
- Even spacing between links
- Time: 12 minutes
- Hint: Use `justify-content: space-between` for the main layout

<details>
<summary>Solution</summary>

```html
<nav class="navbar">
    <div class="logo">MyBrand</div>
    <ul class="nav-links">
        <li><a href="#">Home</a></li>
        <li><a href="#">About</a></li>
        <li><a href="#">Contact</a></li>
    </ul>
    <button class="action-btn">Sign Up</button>
</nav>
```

```css
.navbar {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 15px 30px;
    background: #2563eb;
    color: white;
}

.nav-links {
    display: flex;
    list-style: none;
    gap: 20px;
}

.nav-links a {
    color: white;
    text-decoration: none;
}

.action-btn {
    background: white;
    color: #2563eb;
    border: none;
    padding: 10px 20px;
    border-radius: 6px;
    cursor: pointer;
}
```

</details>

## Bug Hunting
Find the bug in this code:

```css
.buggy {
    display: flex;
    /* Items not spacing correctly */
}
```

<details>
<summary>Solution</summary>

Missing `gap` property for spacing between flex items. Add `gap: 20px;` to create consistent spacing.

</details>

## Quiz
1. What property enables flexbox layout?
2. Is flexbox one-dimensional or two-dimensional?
3. What's a common use case for flexbox?

<details>
<summary>Quiz Answers</summary>

1. `display: flex` enables flexbox layout
2. Flexbox is one-dimensional (row or column)
3. Common use cases include navigation bars, card layouts, and centering elements

</details>

---

### Section 3: Flexbox Parent Properties

## Explain
Flexbox parent properties control how flex items are arranged:

- `flex-direction` - Row or column layout
- `flex-wrap` - Allow wrapping to multiple lines
- `justify-content` - Main axis alignment
- `align-items` - Cross axis alignment
- `gap` - Spacing between items

## Example
```css
.container {
    display: flex;
    flex-direction: row;
    justify-content: center;
    align-items: center;
    gap: 20px;
    flex-wrap: wrap;
}
```

## Try it yourself
Create a flex container with different parent properties.

```html
<div class="flex-parent">
    <div class="item">Item 1</div>
    <div class="item">Item 2</div>
    <div class="item">Item 3</div>
</div>
```

```css
.flex-parent {
    display: flex;
    /* Add different parent properties */
}
```

## Predict
What will happen with this code?

```css
.container {
    justify-content: space-between;
}
```

## Experiment
1. What's the difference between `row` and `column`?
2. How does `flex-wrap: wrap` affect layout?
3. What does `gap` replace in traditional layouts?

## Challenge
**Task:** Create a card layout with:
- 3 cards per row on desktop
- 1 card per row on mobile
- Equal spacing between cards
- Centered content within cards
- Time: 12 minutes
- Hint: Use `flex-wrap: wrap` and responsive breakpoints

<details>
<summary>Solution</summary>

```html
<div class="card-layout">
    <div class="card">Card 1</div>
    <div class="card">Card 2</div>
    <div class="card">Card 3</div>
    <div class="card">Card 4</div>
</div>
```

```css
.card-layout {
    display: flex;
    flex-wrap: wrap;
    gap: 20px;
    justify-content: center;
}

.card {
    flex: 1 1 300px;
    background: white;
    padding: 20px;
    border-radius: 8px;
    box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

@media (max-width: 768px) {
    .card {
        flex: 1 1 100%;
    }
}
```

</details>

## Bug Hunting
Find the bug in this code:

```css
.buggy {
    display: flex;
    justify-content: center;
    align-items: center;
    /* Items not centered vertically */
}
```

<details>
<summary>Solution</summary>

The container might not have a defined height. Give it a height or `min-height` for vertical centering to work.

</details>

## Quiz
1. What does `justify-content` control?
2. What's the difference between `align-items` and `align-content`?
3. Why is `gap` better than margins?

<details>
<summary>Quiz Answers</summary>

1. `justify-content` controls alignment along the main axis (horizontal for row, vertical for column)
2. `align-items` aligns items within their flex line, `align-content` aligns entire flex lines
3. `gap` creates consistent spacing without margin collapse issues and is more flexible

</details>

---

### Section 4: Flexbox Child Properties

## Explain
Flexbox child properties control individual flex item behavior:

- `flex-grow` - How much an item can grow
- `flex-shrink` - How much an item can shrink
- `flex-basis` - Initial size before growing/shrinking
- `flex` - Shorthand for all three
- `order` - Visual order (doesn't affect DOM)
- `align-self` - Override parent alignment

## Example
```css
.item {
    flex: 1; /* grow: 1, shrink: 1, basis: 0% */
}

.item-large {
    flex: 2; /* grows twice as much */
}

.item-override {
    align-self: flex-start;
}
```

## Try it yourself
Create flex items with different child properties.

```html
<div class="flex-container">
    <div class="item item-1">Item 1</div>
    <div class="item item-2">Item 2</div>
    <div class="item item-3">Item 3</div>
</div>
```

```css
.item-1 {
    /* Make this grow more */
}

.item-2 {
    /* Make this shrink less */
}

.item-3 {
    /* Override alignment */
}
```

## Predict
What will happen with this code?

```css
.item {
    flex: 0 1 200px;
}
```

## Experiment
1. What happens with `flex-grow: 0`?
2. How does `order` affect the DOM vs visual order?
3. What's the default `flex` value?

## Challenge
**Task:** Create a responsive layout where:
- First item takes 2x space (flex-grow)
- Second item has fixed width (flex-basis)
- Third item fills remaining space
- Items reorder on mobile
- Time: 12 minutes
- Hint: Use `flex: grow shrink basis` shorthand

<details>
<summary>Solution</summary>

```html
<div class="responsive-layout">
    <div class="item main">Main Content</div>
    <div class="item sidebar">Sidebar</div>
    <div class="item footer">Footer</div>
</div>
```

```css
.responsive-layout {
    display: flex;
    flex-wrap: wrap;
    gap: 20px;
}

.main {
    flex: 2 1 0; /* grows twice as much */
    background: #2563eb;
    color: white;
    padding: 20px;
}

.sidebar {
    flex: 0 0 200px; /* fixed width */
    background: #10b981;
    color: white;
    padding: 20px;
}

.footer {
    flex: 1 1 0; /* fills remaining space */
    background: #f59e0b;
    color: white;
    padding: 20px;
}

@media (max-width: 768px) {
    .main {
        order: 2;
    }
    
    .sidebar {
        order: 1;
    }
    
    .footer {
        order: 3;
    }
}
```

</details>

## Bug Hunting
Find the bug in this code:

```css
.buggy {
    flex: 1;
    /* Item not growing as expected */
}
```

<details>
<summary>Solution</summary>

The parent container might not have extra space to grow into. Make sure the container has defined dimensions or is larger than its content.

</details>

## Quiz
1. What does `flex: 1` represent?
2. What's the difference between `flex-grow` and `flex-shrink`?
3. Does `order` change the DOM order?

<details>
<summary>Quiz Answers</summary>

1. `flex: 1` is shorthand for `flex: 1 1 0%` (grow: 1, shrink: 1, basis: 0%)
2. `flex-grow` controls how much items grow with extra space, `flex-shrink` controls how much they shrink with limited space
3. No, `order` only changes visual order, not the actual DOM order

</details>

---

## Part 2: Comprehensive Project (20 minutes)

### Project: Themed Component Library

**Task:** Create a comprehensive component library that demonstrates CSS Variables and Flexbox integration:

**Requirements:**
1. **CSS Variables Theme System**
   - Global variables for colors, spacing, typography
   - Component-specific variable overrides
   - Easy theme switching capability

2. **Flexbox Layout Components**
   - Navigation bar with flexbox
   - Card grid layout
   - Button group with flexbox
   - Responsive adjustments

3. **Advanced Flexbox Features**
   - Parent properties (justify-content, align-items, gap)
   - Child properties (flex-grow, flex-shrink, order)
   - Responsive layouts with flex-wrap

4. **Variable & Flexbox Integration**
   - Use variables for flex properties
   - Dynamic spacing with variables
   - Theme-aware flexbox layouts

**Time:** 20 minutes
**Hint:** Create a reusable component system that can be easily themed

<details>
<summary>Solution</summary>

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Themed Component Library</title>
    <link rel="stylesheet" href="theme-library.css">
</head>
<body>
    <!-- Navigation -->
    <nav class="navbar">
        <div class="navbar-brand">ThemeLib</div>
        <ul class="navbar-menu">
            <li><a href="#" class="nav-link">Home</a></li>
            <li><a href="#" class="nav-link">Components</a></li>
            <li><a href="#" class="nav-link">Docs</a></li>
        </ul>
        <button class="navbar-toggle">Theme</button>
    </nav>

    <!-- Hero Section -->
    <section class="hero">
        <div class="hero-content">
            <h1 class="hero-title">Themed Component Library</h1>
            <p class="hero-text">CSS Variables + Flexbox integration</p>
            <button class="hero-button">Get Started</button>
        </div>
    </section>

    <!-- Component Showcase -->
    <section class="showcase">
        <h2 class="section-title">Button Components</h2>
        <div class="button-group">
            <button class="btn btn-primary">Primary</button>
            <button class="btn btn-secondary">Secondary</button>
            <button class="btn btn-outline">Outline</button>
        </div>

        <h2 class="section-title">Card Grid</h2>
        <div class="card-grid">
            <div class="card">
                <div class="card-icon">🎨</div>
                <h3 class="card-title">Theming</h3>
                <p class="card-desc">Easy theme switching with CSS variables</p>
            </div>
            <div class="card">
                <div class="card-icon">📱</div>
                <h3 class="card-title">Responsive</h3>
                <p class="card-desc">Flexbox-powered responsive layouts</p>
            </div>
            <div class="card">
                <div class="card-icon">⚡</div>
                <h3 class="card-title">Flexible</h3>
                <p class="card-desc">Dynamic content with flex properties</p>
            </div>
        </div>

        <h2 class="section-title">Stats Dashboard</h2>
        <div class="stats-container">
            <div class="stat-item stat-primary">
                <div class="stat-number">128</div>
                <div class="stat-label">Projects</div>
            </div>
            <div class="stat-item stat-secondary">
                <div class="stat-number">45</div>
                <div class="stat-label">Components</div>
            </div>
            <div class="stat-item stat-accent">
                <div class="stat-number">92%</div>
                <div class="stat-label">Usage</div>
            </div>
        </div>
    </section>
</body>
</html>
```

```css
/* CSS Variables Theme System */
:root {
    /* Colors */
    --primary-color: #2563eb;
    --primary-hover: #1d4ed8;
    --secondary-color: #10b981;
    --secondary-hover: #059669;
    --accent-color: #f59e0b;
    --accent-hover: #d97706;
    
    /* Text Colors */
    --text-primary: #1f2937;
    --text-secondary: #4b5563;
    --text-inverse: #ffffff;
    
    /* Backgrounds */
    --bg-primary: #ffffff;
    --bg-secondary: #f3f4f6;
    --bg-dark: #1f2937;
    
    /* Spacing */
    --spacing-xs: 4px;
    --spacing-sm: 8px;
    --spacing-md: 16px;
    --spacing-lg: 24px;
    --spacing-xl: 32px;
    --spacing-2xl: 48px;
    
    /* Typography */
    --font-size-sm: 14px;
    --font-size-base: 16px;
    --font-size-lg: 18px;
    --font-size-xl: 24px;
    --font-size-2xl: 32px;
    
    /* Border Radius */
    --radius-sm: 4px;
    --radius-md: 8px;
    --radius-lg: 12px;
    --radius-xl: 16px;
    
    /* Shadows */
    --shadow-sm: 0 1px 2px rgba(0, 0, 0, 0.1);
    --shadow-md: 0 4px 6px rgba(0, 0, 0, 0.1);
    --shadow-lg: 0 10px 15px rgba(0, 0, 0, 0.1);
}

/* Dark Theme Override */
.dark-theme {
    --text-primary: #f9fafb;
    --text-secondary: #d1d5db;
    --bg-primary: #1f2937;
    --bg-secondary: #374151;
}

/* Reset & Base */
*, *::before, *::after {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    background-color: var(--bg-secondary);
    color: var(--text-primary);
    line-height: 1.6;
    transition: background-color 0.3s, color 0.3s;
}

/* Navigation - Flexbox */
.navbar {
    display: flex;
    align-items: center;
    justify-content: space-between;
    background: var(--bg-primary);
    padding: var(--spacing-md) var(--spacing-xl);
    box-shadow: var(--shadow-md);
}

.navbar-brand {
    font-size: var(--font-size-xl);
    font-weight: bold;
    color: var(--primary-color);
}

.navbar-menu {
    display: flex;
    list-style: none;
    gap: var(--spacing-lg);
}

.nav-link {
    color: var(--text-secondary);
    text-decoration: none;
    font-weight: 500;
    transition: color 0.3s;
}

.nav-link:hover {
    color: var(--primary-color);
}

.navbar-toggle {
    background: var(--primary-color);
    color: var(--text-inverse);
    border: none;
    padding: var(--spacing-sm) var(--spacing-md);
    border-radius: var(--radius-md);
    cursor: pointer;
    font-weight: 600;
    transition: background-color 0.3s;
}

.navbar-toggle:hover {
    background: var(--primary-hover);
}

/* Hero Section */
.hero {
    background: linear-gradient(135deg, var(--primary-color) 0%, var(--secondary-color) 100%);
    color: var(--text-inverse);
    padding: var(--spacing-2xl) var(--spacing-xl);
    text-align: center;
}

.hero-title {
    font-size: var(--font-size-2xl);
    margin-bottom: var(--spacing-md);
}

.hero-text {
    font-size: var(--font-size-lg);
    margin-bottom: var(--spacing-xl);
    opacity: 0.9;
}

.hero-button {
    background: var(--bg-primary);
    color: var(--primary-color);
    border: none;
    padding: var(--spacing-md) var(--spacing-xl);
    border-radius: var(--radius-lg);
    font-size: var(--font-size-base);
    font-weight: 600;
    cursor: pointer;
    transition: all 0.3s;
}

.hero-button:hover {
    background: var(--bg-secondary);
    transform: translateY(-2px);
}

/* Showcase Section */
.showcase {
    max-width: 1200px;
    margin: 0 auto;
    padding: var(--spacing-2xl) var(--spacing-xl);
}

.section-title {
    color: var(--text-primary);
    margin-bottom: var(--spacing-lg);
    font-size: var(--font-size-xl);
}

/* Button Group - Flexbox */
.button-group {
    display: flex;
    gap: var(--spacing-md);
    margin-bottom: var(--spacing-2xl);
}

.btn {
    padding: var(--spacing-md) var(--spacing-lg);
    border: none;
    border-radius: var(--radius-md);
    font-size: var(--font-size-base);
    font-weight: 600;
    cursor: pointer;
    transition: all 0.3s;
}

.btn-primary {
    background: var(--primary-color);
    color: var(--text-inverse);
}

.btn-primary:hover {
    background: var(--primary-hover);
}

.btn-secondary {
    background: var(--secondary-color);
    color: var(--text-inverse);
}

.btn-secondary:hover {
    background: var(--secondary-hover);
}

.btn-outline {
    background: transparent;
    color: var(--primary-color);
    border: 2px solid var(--primary-color);
}

.btn-outline:hover {
    background: var(--primary-color);
    color: var(--text-inverse);
}

/* Card Grid - Flexbox */
.card-grid {
    display: flex;
    flex-wrap: wrap;
    gap: var(--spacing-lg);
    margin-bottom: var(--spacing-2xl);
}

.card {
    flex: 1 1 300px;
    background: var(--bg-primary);
    padding: var(--spacing-xl);
    border-radius: var(--radius-lg);
    box-shadow: var(--shadow-md);
    transition: transform 0.3s, box-shadow 0.3s;
}

.card:hover {
    transform: translateY(-5px);
    box-shadow: var(--shadow-lg);
}

.card-icon {
    font-size: 48px;
    margin-bottom: var(--spacing-md);
}

.card-title {
    color: var(--text-primary);
    margin-bottom: var(--spacing-sm);
    font-size: var(--font-size-lg);
}

.card-desc {
    color: var(--text-secondary);
    line-height: 1.6;
}

/* Stats Container - Flexbox */
.stats-container {
    display: flex;
    gap: var(--spacing-lg);
    margin-bottom: var(--spacing-2xl);
}

.stat-item {
    flex: 1;
    padding: var(--spacing-xl);
    border-radius: var(--radius-lg);
    text-align: center;
    color: var(--text-inverse);
}

.stat-primary {
    background: var(--primary-color);
}

.stat-secondary {
    background: var(--secondary-color);
}

.stat-accent {
    background: var(--accent-color);
}

.stat-number {
    font-size: var(--font-size-2xl);
    font-weight: bold;
    margin-bottom: var(--spacing-xs);
}

.stat-label {
    font-size: var(--font-size-sm);
    opacity: 0.9;
}

/* Responsive */
@media (max-width: 768px) {
    .navbar-menu {
        display: none;
    }
    
    .card-grid {
        flex-direction: column;
    }
    
    .stats-container {
        flex-direction: column;
    }
    
    .button-group {
        flex-direction: column;
    }
}
```

</details>

---

## Part 3: Review & Assessment (10 minutes)

### Quick Review Quiz

**Question 1:** How do you create a CSS variable?
<details>
<summary>Answer</summary>

Use the `--` prefix, like `--primary-color: #2563eb;` in `:root` or local scope

</details>

**Question 2:** What is the difference between `justify-content` and `align-items`?
<details>
<summary>Answer</summary>

`justify-content` aligns items along the main axis (horizontal for row), `align-items` aligns items along the cross axis (vertical for row)

</details>

**Question 3:** What does `flex-grow: 1` do?
<details>
<summary>Answer</summary>

Allows the item to grow equally with other items when there's extra space available

</details>

**Question 4:** How do you center items both horizontally and vertically in Flexbox?
<details>
<summary>Answer</summary>

`justify-content: center; align-items: center;` on the flex container

</details>

**Question 5:** What does `flex-wrap: wrap` do?
<details>
<summary>Answer</summary>

Allows flex items to wrap to multiple lines when there's not enough space on one line

</details>

### Final Challenge

**Task:** Create a responsive pricing card component that uses ALL concepts:
- CSS variables for theme colors and spacing
- Flexbox for layout (parent and child properties)
- Responsive design with flex-wrap
- Variable overrides for different card tiers
- Time: 10 minutes
- Goal: Build a 3-tier pricing section with basic, pro, and enterprise cards

<details>
<summary>Solution Hint</summary>

Use `flex: 1 1 300px` for cards, different variables for each tier's accent color, and `justify-content: center` for layout

</details>

---

## Homework Assignment

**Task:** Create a complete component using CSS Variables and Flexbox.

**Requirements:**
- Define CSS variables for colors, spacing, and typography
- Create a responsive card layout using Flexbox
- Implement a navigation bar with Flexbox
- Use flex child properties (grow, shrink, order)
- Create a theme that can be easily modified
- Make the layout responsive
- Include hover effects

**Due Date:** Next session

---

## End of Session 5

**Summary:** In this session, you learned about CSS Variables (creation, usage, scopes, overriding) and Flexbox (introduction, parent properties like flex-direction, justify-content, align-items, gap, and child properties like flex-grow, flex-shrink, flex-basis, order, align-self). ALL concepts (Sections 1-4) were practiced through interactive exercises (Explain → Example → Try it yourself → Predict → Experiment → Challenge → Bug Hunting → Quiz), followed by a comprehensive themed component library project that integrated CSS variables and Flexbox for real-world application.

**Next Session:** CSS Grid (Grid parent properties, Grid child properties, minmax/auto-fill/auto-fit, Product gallery and Dashboard implementation)
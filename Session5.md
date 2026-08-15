# Session 5: CSS Variables + Flexbox

## Duration Breakdown
- **1 Hour**: Theoretical Explanation + Live Coding
- **1.5 Hours**: Practical Application (Student writes code)
- **0.5 Hours**: Review, Questions, Problem Solving

---

## Part 1: Theoretical Explanation + Live Coding (1 Hour)

### Section 1: CSS Variables

## What are CSS Variables
CSS Variables (also called Custom Properties) are reusable values that can be stored and used throughout your CSS.

## Creating CSS Variables
```css
:root {
    --primary-color: #2563eb;
    --secondary-color: #10b981;
    --text-color: #1f2937;
    --background-color: #ffffff;
    --spacing: 20px;
    --border-radius: 8px;
    --font-size: 16px;
}
```

**Naming Rules:**
- Must start with `--`
- Case-sensitive
- Can contain letters, numbers, hyphens
- Should be descriptive

---

## Using CSS Variables
```css
.button {
    background-color: var(--primary-color);
    color: var(--text-color);
    padding: var(--spacing);
    border-radius: var(--border-radius);
}
```

## Variable Fallback
```css
.button {
    color: var(--text-color, #333); /* Fallback if variable doesn't exist */
}
```

---

## Local Variables
Variables don't have to be global. You can create them within specific selectors.

```css
.card {
    --card-padding: 30px;
    --card-background: #ffffff;
    
    padding: var(--card-padding);
    background-color: var(--card-background);
}
```

The variable is available inside the `.card` element and its descendants.

---

## Why Use CSS Variables

### Without Variables
```css
.button {
    background: #2563eb;
}

.link {
    color: #2563eb;
}

.heading {
    color: #2563eb;
}
```

If you want to change the primary color, you must change it in multiple places.

### With Variables
```css
:root {
    --primary-color: #2563eb;
}

.button {
    background: var(--primary-color);
}

.link {
    color: var(--primary-color);
}

.heading {
    color: var(--primary-color);
}
```

Now you only need to change one value.

---

## Variable Scopes

### Global Scope (:root)
```css
:root {
    --global-color: #2563eb;
}
```
Available everywhere in the document.

### Local Scope
```css
.component {
    --local-color: #10b981;
}
```
Available only within `.component` and its children.

---

## Overriding Variables
```css
:root {
    --primary-color: #2563eb;
}

.dark-theme {
    --primary-color: #3b82f6;
}
```

---

### Section 2: Flexbox Introduction

## What is Flexbox
Flexbox is a CSS layout system designed to arrange elements in one dimension (either a row or a column).

## When to Use Flexbox
- Navigation bars
- Button groups
- Card layouts
- Centering elements
- Equal height columns
- Spacing between items

## Enable Flexbox
```css
.container {
    display: flex;
}
```

---

### Section 3: Flexbox Parent Properties

## Flex Direction
Controls the main axis direction.

```css
.row {
    flex-direction: row; /* Default: → → → */
}

.row-reverse {
    flex-direction: row-reverse; /* ← ← ← */
}

.column {
    flex-direction: column; /* ↓ ↓ ↓ */
}

.column-reverse {
    flex-direction: column-reverse; /* ↑ ↑ ↑ */
}
```

---

## Flex Wrap
Controls whether items wrap to multiple lines.

```css
.nowrap {
    flex-wrap: nowrap; /* Default: items stay on one line */
}

.wrap {
    flex-wrap: wrap; /* Items wrap to new lines */
}

.wrap-reverse {
    flex-wrap: wrap-reverse; /* Items wrap in reverse order */
}
```

---

## Flex Flow
Shorthand for `flex-direction` and `flex-wrap`.

```css
.container {
    flex-flow: row wrap; /* flex-direction: row; flex-wrap: wrap; */
}
```

---

## Justify Content
Controls how items are distributed along the main axis.

```css
.start {
    justify-content: flex-start; /* Items start at the beginning */
}

.end {
    justify-content: flex-end; /* Items move to the end */
}

.center {
    justify-content: center; /* Items are centered */
}

.space-between {
    justify-content: space-between; /* Equal space between items */
}

.space-around {
    justify-content: space-around; /* Space around each item */
}

.space-evenly {
    justify-content: space-evenly; /* Equal space everywhere */
}
```

---

## Align Items
Controls alignment along the cross axis.

```css
.start {
    align-items: flex-start; /* Items align to start */
}

.end {
    align-items: flex-end; /* Items align to end */
}

.center {
    align-items: center; /* Items are centered */
}

.stretch {
    align-items: stretch; /* Items stretch to fill container */
}

.baseline {
    align-items: baseline; /* Items align along text baseline */
}
```

---

## Align Content
Controls distribution of multiple flex lines (only works with `flex-wrap: wrap`).

```css
.content-start {
    align-content: flex-start;
}

.content-center {
    align-content: center;
}

.content-space-between {
    align-content: space-between;
}
```

---

## Gap
Creates consistent space between flex items.

```css
.container {
    gap: 20px; /* Same gap in both directions */
}

.container {
    row-gap: 20px; /* Gap between rows */
    column-gap: 30px; /* Gap between columns */
}
```

---

### Section 4: Flexbox Child Properties

## Flex Grow
Controls how much an item can grow when extra space is available.

```css
.item {
    flex-grow: 1; /* Item will grow equally */
}

.item-large {
    flex-grow: 2; /* Item will grow twice as much */
}
```

---

## Flex Shrink
Controls how an item shrinks when there's not enough space.

```css
.item {
    flex-shrink: 1; /* Default: item can shrink */
}

.no-shrink {
    flex-shrink: 0; /* Item won't shrink */
}
```

---

## Flex Basis
Defines the initial main-axis size of a flex item.

```css
.item {
    flex-basis: 300px; /* Initial width */
}
```

---

## Flex Shorthand
Combines `flex-grow`, `flex-shrink`, and `flex-basis`.

```css
.item {
    flex: 1; /* flex: 1 1 0% */
}

.item {
    flex: 1 1 200px; /* grow: 1, shrink: 1, basis: 200px */
}
```

---

## Order
Controls the visual order of flex items.

```css
.item-1 {
    order: 3; /* Will appear third */
}

.item-2 {
    order: 1; /* Will appear first */
}

.item-3 {
    order: 2; /* Will appear second */
}
```

---

## Align Self
Overrides the parent's `align-items` for one specific child.

```css
.container {
    align-items: center;
}

.item {
    align-self: flex-start; /* This item aligns to start */
}
```

---

## Part 2: Practical Application (1.5 Hours)

### Exercise 1: CSS Variables Practice (20 minutes)

**Task:**
Create a page using CSS variables for theming.

**HTML:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>CSS Variables Practice</title>
    <link rel="stylesheet" href="variables.css">
</head>
<body>
    <div class="container">
        <h1>CSS Variables Theme</h1>
        
        <div class="card">
            <h2>Primary Card</h2>
            <p>This card uses the primary color theme.</p>
            <button class="button">Primary Button</button>
        </div>
        
        <div class="card secondary">
            <h2>Secondary Card</h2>
            <p>This card uses the secondary color theme.</p>
            <button class="button secondary">Secondary Button</button>
        </div>
    </div>
</body>
</html>
```

**CSS:**
```css
/* Global Variables */
:root {
    --primary-color: #2563eb;
    --primary-hover: #1d4ed8;
    --secondary-color: #10b981;
    --secondary-hover: #059669;
    --text-color: #1f2937;
    --background-color: #f3f4f6;
    --card-background: #ffffff;
    --spacing-sm: 10px;
    --spacing-md: 20px;
    --spacing-lg: 30px;
    --border-radius: 8px;
    --font-size: 16px;
}

body {
    font-family: Arial, sans-serif;
    background-color: var(--background-color);
    color: var(--text-color);
    font-size: var(--font-size);
    padding: var(--spacing-lg);
    margin: 0;
}

.container {
    max-width: 800px;
    margin: 0 auto;
}

h1 {
    text-align: center;
    color: var(--primary-color);
    margin-bottom: var(--spacing-lg);
}

.card {
    background-color: var(--card-background);
    padding: var(--spacing-lg);
    border-radius: var(--border-radius);
    margin-bottom: var(--spacing-md);
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.card h2 {
    color: var(--primary-color);
    margin-bottom: var(--spacing-sm);
}

.card.secondary h2 {
    color: var(--secondary-color);
}

.card p {
    color: var(--text-color);
    line-height: 1.6;
    margin-bottom: var(--spacing-md);
}

.button {
    background-color: var(--primary-color);
    color: white;
    padding: var(--spacing-sm) var(--spacing-md);
    border: none;
    border-radius: var(--border-radius);
    cursor: pointer;
    font-size: var(--font-size);
    transition: background-color 0.3s;
}

.button:hover {
    background-color: var(--primary-hover);
}

.button.secondary {
    background-color: var(--secondary-color);
}

.button.secondary:hover {
    background-color: var(--secondary-hover);
}
```

---

### Exercise 2: Flexbox Navbar (25 minutes)

**Task:**
Create a responsive navigation bar using Flexbox.

**HTML:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Flexbox Navbar</title>
    <link rel="stylesheet" href="navbar.css">
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
        <button class="login-btn">Login</button>
    </nav>
    
    <div class="content">
        <h1>Welcome to My Website</h1>
        <p>This page demonstrates a Flexbox navigation bar.</p>
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

/* Navbar Styling */
.navbar {
    display: flex;
    align-items: center;
    justify-content: space-between;
    background-color: #2563eb;
    color: white;
    padding: 15px 30px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.logo {
    font-size: 24px;
    font-weight: bold;
}

.nav-links {
    display: flex;
    list-style: none;
    gap: 30px;
}

.nav-links a {
    color: white;
    text-decoration: none;
    font-weight: 500;
    transition: color 0.3s;
}

.nav-links a:hover {
    color: #bfdbfe;
}

.login-btn {
    background-color: white;
    color: #2563eb;
    border: none;
    padding: 10px 20px;
    border-radius: 6px;
    font-weight: 600;
    cursor: pointer;
    transition: all 0.3s;
}

.login-btn:hover {
    background-color: #bfdbfe;
    transform: translateY(-2px);
}

/* Content Styling */
.content {
    max-width: 800px;
    margin: 40px auto;
    padding: 20px;
    text-align: center;
}

.content h1 {
    color: #1f2937;
    margin-bottom: 20px;
}

.content p {
    color: #4b5563;
    line-height: 1.6;
}
```

---

### Exercise 3: Flex Cards (25 minutes)

**Task:**
Create responsive card layout using Flexbox.

**HTML:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Flex Cards</title>
    <link rel="stylesheet" href="flex-cards.css">
</head>
<body>
    <h1>Flex Card Layout</h1>
    
    <div class="card-container">
        <div class="card">
            <h3>Card 1</h3>
            <p>This is the first card with some content.</p>
            <button>Learn More</button>
        </div>
        
        <div class="card">
            <h3>Card 2</h3>
            <p>This is the second card with different content.</p>
            <button>Learn More</button>
        </div>
        
        <div class="card">
            <h3>Card 3</h3>
            <p>This is the third card with more content.</p>
            <button>Learn More</button>
        </div>
        
        <div class="card">
            <h3>Card 4</h3>
            <p>This is the fourth card with unique content.</p>
            <button>Learn More</button>
        </div>
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
    padding: 20px;
}

h1 {
    text-align: center;
    color: #1f2937;
    margin-bottom: 30px;
}

.card-container {
    display: flex;
    flex-wrap: wrap;
    gap: 20px;
    justify-content: center;
}

.card {
    background-color: white;
    border-radius: 12px;
    padding: 30px;
    min-width: 250px;
    flex: 1 1 300px; /* grow, shrink, basis */
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    transition: transform 0.3s, box-shadow 0.3s;
}

.card:hover {
    transform: translateY(-5px);
    box-shadow: 0 8px 12px rgba(0, 0, 0, 0.15);
}

.card h3 {
    color: #2563eb;
    margin-bottom: 15px;
}

.card p {
    color: #4b5563;
    line-height: 1.6;
    margin-bottom: 20px;
}

.card button {
    background-color: #2563eb;
    color: white;
    border: none;
    padding: 10px 20px;
    border-radius: 6px;
    cursor: pointer;
    font-weight: 500;
    transition: background-color 0.3s;
}

.card button:hover {
    background-color: #1d4ed8;
}
```

---

### Exercise 4: Flexbox Child Properties (20 minutes)

**Task:**
Create examples demonstrating flex child properties.

**HTML:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Flex Child Properties</title>
    <link rel="stylesheet" href="child-properties.css">
</head>
<body>
    <h1>Flex Child Properties</h1>
    
    <h2>Flex Grow</h2>
    <div class="container grow-container">
        <div class="item grow-1">Grow 1</div>
        <div class="item grow-2">Grow 2</div>
        <div class="item grow-1">Grow 1</div>
    </div>
    
    <h2>Flex Shrink</h2>
    <div class="container shrink-container">
        <div class="item shrink-0">No Shrink</div>
        <div class="item shrink-1">Can Shrink</div>
        <div class="item shrink-0">No Shrink</div>
    </div>
    
    <h2>Order</h2>
    <div class="container order-container">
        <div class="item order-3">Order 3</div>
        <div class="item order-1">Order 1</div>
        <div class="item order-2">Order 2</div>
    </div>
    
    <h2>Align Self</h2>
    <div class="container align-self-container">
        <div class="item align-start">Start</div>
        <div class="item align-center">Center</div>
        <div class="item align-end">End</div>
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

.container {
    display: flex;
    background-color: white;
    padding: 20px;
    border-radius: 8px;
    margin-bottom: 20px;
    gap: 10px;
}

.item {
    background-color: #2563eb;
    color: white;
    padding: 20px;
    border-radius: 4px;
    text-align: center;
    font-weight: bold;
}

/* Flex Grow */
.grow-container {
    justify-content: space-between;
}

.grow-1 {
    flex-grow: 1;
}

.grow-2 {
    flex-grow: 2;
}

/* Flex Shrink */
.shrink-container {
    width: 500px;
}

.shrink-0 {
    flex-shrink: 0;
    width: 150px;
}

.shrink-1 {
    flex-shrink: 1;
    flex-grow: 1;
}

/* Order */
.order-1 {
    order: 1;
}

.order-2 {
    order: 2;
}

.order-3 {
    order: 3;
}

/* Align Self */
.align-self-container {
    align-items: center;
    height: 150px;
}

.align-start {
    align-self: flex-start;
}

.align-center {
    align-self: center;
}

.align-end {
    align-self: flex-end;
}
```

---

## Part 3: Review, Questions, Problem Solving (0.5 Hours)

### Flexbox Froggy Practice (10 minutes)

**Introduction to Flexbox Froggy:**
Flexbox Froggy is an interactive game that helps you learn Flexbox through practice.

**How to Access:**
1. Go to https://flexboxfroggy.com/
2. Complete the levels to practice Flexbox properties
3. Each level teaches a different Flexbox concept

**What You'll Practice:**
- `justify-content`
- `align-items`
- `flex-direction`
- `flex-wrap`
- `align-content`
- `order`
- `align-self`

---

### Review Questions (10 minutes)

**Question 1:** How do you create a CSS variable?
**Answer:** Use the `--` prefix, like `--primary-color: #2563eb;`

**Question 2:** What is the difference between global and local CSS variables?
**Answer:** Global variables are defined in `:root` and available everywhere, local variables are defined within specific selectors.

**Question 3:** What does `flex-direction: column` do?
**Answer:** Arranges flex items vertically from top to bottom.

**Question 4:** What is the difference between `justify-content` and `align-items`?
**Answer:** `justify-content` aligns items along the main axis, `align-items` aligns items along the cross axis.

**Question 5:** What does `flex-grow: 1` do?
**Answer:** Allows the item to grow equally with other items when there's extra space.

**Question 6:** How do you center items both horizontally and vertically in Flexbox?
**Answer:** `justify-content: center; align-items: center;`

**Question 7:** What does `flex-wrap: wrap` do?
**Answer:** Allows flex items to wrap to multiple lines when there's not enough space.

**Question 8:** What is the purpose of the `gap` property in Flexbox?
**Answer:** Creates consistent space between flex items without using margins.

---

### Practice Challenges (5 minutes)

**Challenge 1:** Create a navigation bar with logo on left, links in center, and button on right using Flexbox.

**Challenge 2:** Create a row of 3 cards where the middle card is twice as wide as the others using `flex-grow`.

**Challenge 3:** Create a CSS variable theme with at least 5 variables and use them throughout a component.

---

### Homework Assignment

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

**Next Session:** CSS Grid (Grid parent properties, Grid child properties, minmax/auto-fill/auto-fit, Product gallery and Dashboard implementation)
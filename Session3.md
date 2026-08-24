# Session 3: Inheritance, Typography, Positioning

## Duration Breakdown (2.5 Hours Total)
- **1 Hour**: Theoretical Explanation + Live Coding (Sections 1-6)
- **1 Hour**: Interactive Learning (Sections 7-12 with Explain → Example → Try it yourself → Predict → Experiment → Challenge → Bug Hunting → Quiz)
- **20 Minutes**: Comprehensive Project (Single component combining all concepts)
- **10 Minutes**: Review & Assessment

---

## Part 1: Theoretical Explanation + Live Coding (1 Hour)

### Section 1: CSS Inheritance {#section-1}

**What is CSS Inheritance:**
- Some CSS properties are automatically inherited from parent elements by their children
- Not all properties are inherited
- Inheritance creates a natural cascade of styles

**Example:**
```html
<div class="parent">
    <p>This paragraph inherits color from parent</p>
</div>
```

```css
.parent {
    color: blue;
    font-size: 18px;
}
```

The paragraph will be blue and 18px even without explicit styling.

---

## Properties That Commonly Inherit

```css
color
font-family
font-size
font-style
font-weight
line-height
text-align
visibility
cursor
```

---

## Properties That Usually Do Not Inherit

```css
margin
padding
border
background
width
height
position
display
```

---

## Controlling Inheritance

### Force Inheritance
```css
.child {
    color: inherit; /* Explicitly inherit from parent */
}
```

### Prevent Inheritance
```css
.child {
    color: initial; /* Use initial value, not inherited */
}
```

### Reset to Default
```css
.child {
    color: unset; /* Acts as inherit if inherited, otherwise initial */
}
```

---

### Section 2: Typography - Font Family {#section-2}

## Font Family Basics
```css
body {
    font-family: Arial, sans-serif;
}
```

## Font Stacks
```css
body {
    /* Multiple fonts as fallbacks */
    font-family: "Helvetica Neue", Arial, sans-serif;
}
```

## Common Font Families

### Serif
```css
.serif {
    font-family: Georgia, "Times New Roman", Times, serif;
}
```

### Sans-Serif
```css
.sans-serif {
    font-family: Arial, "Helvetica Neue", Helvetica, sans-serif;
}
```

### Monospace
```css
.monospace {
    font-family: "Courier New", Courier, monospace;
}
```

### Cursive
```css
.cursive {
    font-family: "Brush Script MT", cursive;
}
```

### Fantasy
```css
.fantasy {
    font-family: "Papyrus", fantasy;
}
```

## Web Fonts
```css
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap');

body {
    font-family: 'Roboto', sans-serif;
}
```

---

### Section 3: Font Size {#section-3}

## Absolute Sizes
```css
.small {
    font-size: 12px;
}

.medium {
    font-size: 16px;
}

.large {
    font-size: 24px;
}
```

## Relative Sizes
```css
.parent {
    font-size: 16px;
}

.child {
    font-size: 1.5em; /* 24px (16 × 1.5) */
}
```

## Root Relative (rem)
```css
html {
    font-size: 16px;
}

.body {
    font-size: 1rem; /* 16px */
}

.heading {
    font-size: 2rem; /* 32px */
}
```

## Percentage
```css
.parent {
    font-size: 20px;
}

.child {
    font-size: 80%; /* 16px (20 × 0.8) */
}
```

## Viewport Units
```css
.responsive {
    font-size: 4vw; /* 4% of viewport width */
}
```

---

### Section 4: CSS Units {#section-4}

## Absolute Units
```css
.px {
    width: 100px; /* Pixels */
}

.pt {
    font-size: 12pt; /* Points */
}

.cm {
    width: 2cm; /* Centimeters */
}

.mm {
    width: 20mm; /* Millimeters */
}

.in {
    width: 1in; /* Inches */
}
```

**Use when:** You need exact, physical measurements.

---

## Relative Units

### em
```css
.parent {
    font-size: 16px;
}

.child {
    width: 2em; /* 32px (16 × 2) */
}
```
- Relative to parent's font size
- Compounds with nested elements

### rem
```css
html {
    font-size: 16px;
}

.element {
    width: 2rem; /* 32px (always based on root) */
}
```
- Relative to root font size
- Doesn't compound
- Recommended for most cases

### Percentage
```css
.container {
    width: 80%; /* 80% of parent */
}
```
- Relative to parent element
- Common for layouts

### Viewport Units
```css
.vw {
    width: 50vw; /* 50% of viewport width */
}

.vh {
    height: 50vh; /* 50% of viewport height */
}

.vmin {
    width: 50vmin; /* 50% of smaller dimension */
}

.vmax {
    width: 50vmax; /* 50% of larger dimension */
}
```

---

## When to Use Each Unit

| Unit | Best For | Example |
|------|----------|---------|
| px | Borders, precise sizes | border: 1px solid |
| rem | Typography, spacing | font-size: 1.5rem |
| em | Component-relative sizing | padding: 1em |
| % | Layouts, responsive width | width: 80% |
| vw/vh | Full-screen elements | height: 100vh |

---

### Section 5: Font Style {#section-5}

## Values
```css
.normal {
    font-style: normal; /* Default */
}

.italic {
    font-style: italic;
}

.oblique {
    font-style: oblique; /* Similar to italic */
}
```

**Use Cases:**
- Emphasis
- Citations
- Book titles
- Foreign words

---

### Section 6: Font Variant {#section-6}

## Small Caps
```css
.small-caps {
    font-variant: small-caps;
}
```

## Normal
```css
.normal {
    font-variant: normal;
}
```

**Use Cases:**
- Headings
- Acronyms
- Stylistic choices

---

### Section 7: Font Weight {#section-7}

## Numeric Values
```css
.light {
    font-weight: 300;
}

.normal {
    font-weight: 400;
}

.medium {
    font-weight: 500;
}

.bold {
    font-weight: 700;
}

.extra-bold {
    font-weight: 900;
}
```

## Named Values
```css
.normal {
    font-weight: normal; /* 400 */
}

.bold {
    font-weight: bold; /* 700 */
}

.lighter {
    font-weight: lighter; /* Relative to parent */
}

.bolder {
    font-weight: bolder; /* Relative to parent */
}
```

---

### Section 8: Mouse Cursor {#section-8}

## Common Cursor Values
```css
.default {
    cursor: default; /* Default arrow */
}

.pointer {
    cursor: pointer; /* Hand cursor */
}

.text {
    cursor: text; /* I-beam cursor */
}

.wait {
    cursor: wait; /* Hourglass */
}

.crosshair {
    cursor: crosshair; /* Crosshair */
}

.not-allowed {
    cursor: not-allowed; /* Circle with line */
}

.help {
    cursor: help; /* Question mark */
}

.move {
    cursor: move; /* Move icon */
}
```

**Use Cases:**
- Buttons (pointer)
- Links (pointer)
- Text areas (text)
- Dragging elements (move)
- Disabled elements (not-allowed)

---

### Section 9: CSS Calculations {#section-9}

## Explain
The `calc()` function allows you to perform mathematical calculations in CSS values:

- Can mix different units (px, %, em, rem, vw, etc.)
- Useful for responsive design
- Supports +, -, *, / operators
- Must include spaces around operators

## Example
```css
.container {
    width: calc(100% - 20px);
}

.responsive {
    font-size: calc(16px + 1vw);
}

.complex {
    width: calc(50% - 10px + 2em);
}
```

## Try it yourself
Create a container that subtracts padding from width.

```html
<div class="responsive-box">Responsive Box</div>
```

```css
.responsive-box {
    width: calc(100% - 40px);
    /* Add padding to complete the calculation */
}
```

## Predict
What will happen with this code?

```css
.mystery {
    width: calc(100%+20px);
}
```

## Experiment
1. What happens if you forget spaces around operators?
2. Can you use `calc()` with font-size?
3. Try combining viewport units with pixels

## Challenge
**Task:** Create a responsive card where:
- Width is 50% minus 20px
- Padding is calculated using calc()
- Font size scales with viewport
- Time: 10 minutes
- Hint: Use calc() for both width and padding

<details>
<summary>Solution</summary>

```html
<div class="calc-card">
    <h3>Responsive Card</h3>
    <p>This card uses calc() for responsive sizing</p>
</div>
```

```css
.calc-card {
    width: calc(50% - 20px);
    padding: calc(10px + 1vw);
    font-size: calc(14px + 0.5vw);
    background: white;
    border: 1px solid #ddd;
    border-radius: 8px;
    margin: 10px;
}
```

</details>

## Bug Hunting
Find the bug in this code:

```css
.buggy-calc {
    width: calc(100%-20px);
    /* Why might this cause issues? */
}
```

<details>
<summary>Solution</summary>

Missing spaces around the minus operator. Should be `calc(100% - 20px)`.

</details>

## Quiz
1. What operators does calc() support?
2. Why are spaces required around operators?
3. Can you mix different units in calc()?

<details>
<summary>Quiz Answers</summary>

1. +, -, *, / (addition, subtraction, multiplication, division)
2. Spaces are required for the CSS parser to correctly identify the operators
3. Yes, you can mix units like px, %, em, rem, vw, vh in the same calculation

</details>

---

### Section 10: Opacity {#section-10}

## Explain
Opacity controls the transparency level of an entire element:

- Values range from 0 (fully transparent) to 1 (fully opaque)
- Affects the entire element including content
- Different from RGBA which only affects color transparency

## Example
```css
.fully-opaque {
    opacity: 1; /* 100% opaque */
}

.half-transparent {
    opacity: 0.5; /* 50% transparent */
}

.fully-transparent {
    opacity: 0; /* 100% transparent */
}
```

## Try it yourself
Create elements with different opacity levels.

```html
<div class="opacity-1">Level 1</div>
<div class="opacity-2">Level 2</div>
<div class="opacity-3">Level 3</div>
```

```css
.opacity-1 {
    background: blue;
    /* Set opacity to 0.3 */
}

.opacity-2 {
    background: green;
    /* Set opacity to 0.6 */
}

.opacity-3 {
    background: red;
    /* Set opacity to 0.9 */
}
```

## Predict
What will happen with this code?

```css
.box {
    opacity: 0.5;
    color: red;
}
```

## Experiment
1. What's the difference between `opacity: 0.5` and `rgba(255,0,0,0.5)`?
2. Can text inside an element with opacity be fully opaque?
3. Try opacity on a parent element with children

## Challenge
**Task:** Create a card with hover effect where:
- Normal state has full opacity
- Hover state reduces opacity to 0.8
- Text remains readable
- Time: 8 minutes
- Hint: Consider using RGBA for background instead of opacity to keep text readable

<details>
<summary>Solution</summary>

```html
<div class="opacity-card">
    <h3>Hover Card</h3>
    <p>Hover over this card to see the opacity effect</p>
</div>
```

```css
.opacity-card {
    background: rgba(37, 99, 235, 1);
    color: white;
    padding: 20px;
    border-radius: 8px;
    transition: opacity 0.3s;
}

.opacity-card:hover {
    opacity: 0.8;
}

/* Alternative using RGBA to keep text readable */
.opacity-card-alt {
    background: rgba(37, 99, 235, 1);
    color: white;
    padding: 20px;
    border-radius: 8px;
    transition: background 0.3s;
}

.opacity-card-alt:hover {
    background: rgba(37, 99, 235, 0.8);
}
```

</details>

## Bug Hunting
Find the bug in this code:

```css
.buggy-opacity {
    opacity: 1.5;
    /* What's wrong with this value? */
}
```

<details>
<summary>Solution</summary>

Opacity values should be between 0 and 1. Values above 1 are clamped to 1, but this shows a misunderstanding of the opacity property.

</details>

## Quiz
1. What is the valid range for opacity values?
2. How does opacity differ from RGBA transparency?
3. Does opacity affect the element's layout space?

<details>
<summary>Quiz Answers</summary>

1. Opacity values range from 0 (fully transparent) to 1 (fully opaque)
2. Opacity affects the entire element including content, RGBA affects only the color
3. No, opacity doesn't affect layout space - the element still takes up the same space

</details>

---

### Section 11: Position {#section-11}

## Explain
The `position` property controls how elements are positioned in the document:

- `static` - Default, follows normal flow
- `relative` - Positioned relative to its normal position
- `absolute` - Positioned relative to nearest positioned ancestor
- `fixed` - Positioned relative to viewport, stays fixed during scroll
- `sticky` - Acts like relative until scroll threshold, then like fixed

## Example
```css
.static {
    position: static;
}

.relative {
    position: relative;
    top: 10px;
    left: 20px;
}

.absolute {
    position: absolute;
    top: 0;
    right: 0;
}

.fixed {
    position: fixed;
    top: 0;
    left: 0;
}

.sticky {
    position: sticky;
    top: 10px;
}
```

## Try it yourself
Create elements with different position values.

```html
<div class="parent">
    <div class="relative-box">Relative</div>
    <div class="absolute-box">Absolute</div>
</div>
```

```css
.parent {
    position: relative;
    height: 200px;
    background: #f0f0f0;
}

.relative-box {
    /* Make this relative and move it */
}

.absolute-box {
    /* Make this absolute and position it */
}
```

## Predict
What will happen with this code?

```css
.child {
    position: absolute;
    top: 10px;
}
```

## Experiment
1. What happens when you use absolute without a positioned parent?
2. Try fixed positioning - what happens when you scroll?
3. What's the difference between relative and absolute?

## Challenge
**Task:** Create a card with:
- Absolute positioned badge in top-right corner
- Relative positioned container
- Content that flows normally
- Time: 12 minutes
- Hint: Parent needs position: relative for child absolute positioning

<details>
<summary>Solution</summary>

```html
<div class="card-container">
    <div class="badge">New</div>
    <h3>Card Title</h3>
    <p>Card content goes here</p>
</div>
```

```css
.card-container {
    position: relative;
    background: white;
    padding: 20px;
    border-radius: 8px;
    box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

.badge {
    position: absolute;
    top: -10px;
    right: -10px;
    background: #e11d48;
    color: white;
    padding: 5px 10px;
    border-radius: 20px;
    font-size: 12px;
    font-weight: bold;
}
```

</details>

## Bug Hunting
Find the bug in this code:

```css
.child {
    position: absolute;
    top: 10px;
    right: 10px;
}

.parent {
    /* Missing position property */
}
```

<details>
<summary>Solution</summary>

The parent element needs `position: relative` (or any non-static position) for the child's absolute positioning to work relative to the parent.

</details>

## Quiz
1. What is the default position value?
2. What's the difference between absolute and fixed?
3. When does sticky positioning change behavior?

<details>
<summary>Quiz Answers</summary>

1. The default position value is `static`
2. Absolute is positioned relative to nearest positioned ancestor, fixed is always relative to viewport
3. Sticky acts like relative until the element reaches the specified scroll threshold, then acts like fixed

</details>

---

### Section 12: Z-Index {#section-12}

## Explain
The `z-index` property controls the stacking order of positioned elements:

- Only works on elements with position (not static)
- Higher values appear on top of lower values
- Can be negative for background layers
- Default value is `auto`

## Example
```css
.bottom {
    position: relative;
    z-index: 1;
}

.middle {
    position: relative;
    z-index: 2;
}

.top {
    position: relative;
    z-index: 3;
}
```

## Try it yourself
Create overlapping elements with different z-index values.

```html
<div class="container">
    <div class="box layer-1">Layer 1</div>
    <div class="box layer-2">Layer 2</div>
    <div class="box layer-3">Layer 3</div>
</div>
```

```css
.container {
    position: relative;
    height: 200px;
}

.box {
    position: absolute;
    /* Add width, height, colors, and z-index */
}
```

## Predict
What will happen with this code?

```css
.layer-1 {
    position: relative;
    z-index: 10;
}

.layer-2 {
    z-index: 5; /* Missing position */
}
```

## Experiment
1. What happens if you use z-index without position?
2. Try negative z-index values
3. What happens with equal z-index values?

## Challenge
**Task:** Create a modal overlay with:
- Semi-transparent background (z-index: 1000)
- Modal content on top (z-index: 1001)
- Page content behind (z-index: 1)
- Time: 10 minutes
- Hint: Use fixed positioning for the overlay

<details>
<summary>Solution</summary>

```html
<div class="page-content">
    <h1>Page Content</h1>
    <p>This is the main page content</p>
</div>

<div class="modal-overlay">
    <div class="modal-content">
        <h3>Modal Title</h3>
        <p>Modal content goes here</p>
        <button class="close-btn">Close</button>
    </div>
</div>
```

```css
.page-content {
    position: relative;
    z-index: 1;
    padding: 20px;
}

.modal-overlay {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, 0.5);
    z-index: 1000;
    display: flex;
    align-items: center;
    justify-content: center;
}

.modal-content {
    background: white;
    padding: 30px;
    border-radius: 8px;
    z-index: 1001;
    max-width: 400px;
}
```

</details>

## Bug Hunting
Find the bug in this code:

```css
.buggy-zindex {
    z-index: 100;
    /* Why won't this work? */
}
```

<details>
<summary>Solution</summary>

z-index only works on positioned elements (not static). Add `position: relative`, `absolute`, `fixed`, or `sticky`.

</details>

## Quiz
1. What is required for z-index to work?
2. What does a higher z-index value do?
3. Can z-index be negative?

<details>
<summary>Quiz Answers</summary>

1. The element must have a position value other than static
2. Higher z-index values appear on top of elements with lower values
3. Yes, z-index can be negative, which places elements behind the normal flow

</details>

---

## Part 2: Comprehensive Project (20 minutes)

### Project: Profile Card with Advanced Styling

**Task:** Create a sophisticated profile card that demonstrates all positioning, typography, and calculation concepts:

**Requirements:**
1. **CSS Inheritance** - Use inherited properties for text styling
2. **Typography** - Mix of font families, sizes, weights using rem units
3. **CSS Units** - Use rem, em, %, and calc() for sizing
4. **Position** - Absolute positioned badge, relative container
5. **Z-Index** - Layering for overlap effects
6. **Opacity** - Hover effects with transparency
7. **calc()** - Responsive width calculations

**Time:** 20 minutes
**Hint:** Start with inheritance, then add positioning, then apply calculations

<details>
<summary>Solution</summary>

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Profile Card</title>
    <link rel="stylesheet" href="profile-card.css">
</head>
<body>
    <div class="card-container">
        <div class="profile-card">
            <div class="badge">Featured</div>
            <div class="avatar-section">
                <div class="avatar">JD</div>
            </div>
            <div class="profile-info">
                <h2 class="name">John Doe</h2>
                <p class="title">Senior Developer</p>
                <p class="bio">Passionate about creating beautiful web experiences with modern CSS.</p>
            </div>
            <div class="card-actions">
                <button class="btn-primary">Follow</button>
                <button class="btn-secondary">Message</button>
            </div>
        </div>
    </div>
</body>
</html>
```

```css
/* CSS Reset */
*, *::before, *::after {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

/* Base styles with inheritance */
html {
    font-size: 16px;
}

body {
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    line-height: 1.6;
    color: #333;
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    min-height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 20px;
}

/* Card Container */
.card-container {
    position: relative;
    max-width: 400px;
    width: 100%;
}

/* Profile Card */
.profile-card {
    position: relative;
    background: white;
    border-radius: 16px;
    overflow: hidden;
    box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2);
    transition: transform 0.3s, box-shadow 0.3s;
}

.profile-card:hover {
    transform: translateY(-5px);
    box-shadow: 0 15px 40px rgba(0, 0, 0, 0.3);
}

/* Absolute Positioned Badge */
.badge {
    position: absolute;
    top: 15px;
    right: 15px;
    background: linear-gradient(135deg, #f093fb 0%, #f5576c 100%);
    color: white;
    padding: 0.5em 1em;
    border-radius: 20px;
    font-size: 0.75rem;
    font-weight: bold;
    z-index: 10;
    text-transform: uppercase;
    letter-spacing: 1px;
}

/* Avatar Section */
.avatar-section {
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    padding: 2rem;
    display: flex;
    justify-content: center;
    align-items: center;
}

.avatar {
    width: 80px;
    height: 80px;
    background: white;
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 2rem;
    font-weight: bold;
    color: #667eea;
    border: 4px solid rgba(255, 255, 255, 0.3);
}

/* Profile Info - Inherits body font family */
.profile-info {
    padding: calc(1.5rem + 0.5em);
    text-align: center;
}

.name {
    font-size: 1.5rem;
    font-weight: 700;
    color: #1f2937;
    margin-bottom: 0.25em;
}

.title {
    font-size: 0.875rem;
    font-weight: 500;
    color: #6b7280;
    text-transform: uppercase;
    letter-spacing: 0.05em;
    margin-bottom: 1em;
}

.bio {
    font-size: 0.95rem;
    line-height: 1.6;
    color: #4b5563;
    margin-bottom: 1.5em;
}

/* Card Actions */
.card-actions {
    display: flex;
    gap: 1rem;
    padding: 0 1.5rem 1.5rem;
}

.btn-primary, .btn-secondary {
    flex: 1;
    padding: 0.75em 1em;
    border-radius: 8px;
    font-size: 0.9rem;
    font-weight: 600;
    cursor: pointer;
    transition: all 0.3s;
    border: none;
}

.btn-primary {
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    color: white;
}

.btn-primary:hover {
    opacity: 0.9;
    transform: translateY(-2px);
}

.btn-secondary {
    background: #f3f4f6;
    color: #374151;
}

.btn-secondary:hover {
    background: #e5e7eb;
}

/* Responsive */
@media (max-width: 480px) {
    .card-container {
        max-width: calc(100% - 40px);
    }
    
    .profile-info {
        padding: 1.25rem;
    }
    
    .card-actions {
        flex-direction: column;
    }
}
```

</details>

---

## Part 3: Review & Assessment (10 minutes)

### Quick Review Quiz

**Question 1:** What is CSS inheritance?
<details>
<summary>Answer</summary>

Some CSS properties automatically pass from parent elements to their children, like color, font-family, and font-size.

</details>

**Question 2:** What's the difference between `em` and `rem`?
<details>
<summary>Answer</summary>

`em` is relative to the parent element's font size, while `rem` is relative to the root (html) font size.

</details>

**Question 3:** What does `position: sticky` do?
<details>
<summary>Answer</summary>

Acts like relative positioning until the element reaches a specified scroll threshold, then acts like fixed positioning.

</details>

**Question 4:** When does `z-index` not work?
<details>
<summary>Answer</summary>

z-index only works on elements with a position value other than static (relative, absolute, fixed, or sticky).

</details>

**Question 5:** What's the difference between absolute and fixed positioning?
<details>
<summary>Answer</summary>

Absolute positioning is relative to the nearest positioned ancestor, while fixed positioning is always relative to the viewport.

</details>

### Final Challenge

**Task:** Create a tooltip component that uses ALL concepts from this session:
- Inheritance for text styling
- Typography with rem units
- calc() for positioning
- Absolute positioning for the tooltip
- z-index for layering
- Opacity for fade-in effect

**Time:** 10 minutes
**Goal:** Build a hover tooltip that appears when hovering over an element

<details>
<summary>Solution Hint</summary>

Use a parent container with position: relative, child tooltip with position: absolute, and hover state for opacity transition

</details>

---

## Homework Assignment

**Task:** Create a styled blog post card using all concepts from this session.

**Requirements:**
- CSS inheritance for consistent text styling
- Typography using rem units and different font weights
- Absolute positioned category badge
- calc() for responsive width calculations
- Opacity hover effects
- z-index for overlapping elements
- Position types (relative for container, absolute for badge)

**Due Date:** Next session

---

## End of Session 3

**Summary:** In this session, you learned about CSS inheritance, typography (font families, sizes, weights, styles), CSS units (absolute, relative, viewport), CSS calculations with calc(), opacity, positioning (static, relative, absolute, fixed, sticky), and z-index layering. Each concept was practiced through interactive exercises (Explain → Example → Try it yourself → Predict → Experiment → Challenge → Bug Hunting → Quiz), followed by a comprehensive profile card project that integrated all concepts.

**Next Session:** Pseudo-classes/Elements + Cascade (List/Table Styling, Pseudo-classes, Pseudo-elements, Border-radius, Box-shadow, Forms Styling, Specificity, !important, Margin Collapse)
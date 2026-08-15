# Session 3: Inheritance, Typography, Positioning

## Duration Breakdown
- **1 Hour**: Theoretical Explanation + Live Coding
- **1.5 Hours**: Practical Application (Student writes code)
- **0.5 Hours**: Review, Questions, Problem Solving

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

## calc() Function
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

## Use Cases
- Responsive sizing
- Combining different units
- Complex calculations
- Dynamic layouts

## Best Practices
- Always include spaces around operators
- Can mix different units
- Supported in all modern browsers

---

### Section 10: Opacity {#section-10}

## Opacity Values
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

## RGBA vs Opacity
```css
/* Opacity affects entire element */
.box {
    opacity: 0.5;
}

/* RGBA affects only color */
.box {
    background-color: rgba(255, 0, 0, 0.5);
}
```

**Use Cases:**
- Hover effects
- Disabled states
- Layering
- Visual feedback

---

### Section 11: Position {#section-11}

## Position Values

### Static (Default)
```css
.static {
    position: static; /* Default positioning */
}
```
- Follows normal document flow
- Not affected by top, right, bottom, left

### Relative
```css
.relative {
    position: relative;
    top: 10px;
    left: 20px;
}
```
- Positioned relative to its normal position
- Affects child absolute positioning
- Still takes up space in document flow

### Absolute
```css
.absolute {
    position: absolute;
    top: 0;
    right: 0;
}
```
- Positioned relative to nearest positioned ancestor
- Removed from normal document flow
- Doesn't affect other elements

### Fixed
```css
.fixed {
    position: fixed;
    top: 0;
    left: 0;
}
```
- Positioned relative to viewport
- Stays in place during scrolling
- Removed from document flow

### Sticky
```css
.sticky {
    position: sticky;
    top: 10px;
}
```
- Acts like relative until scroll position
- Then acts like fixed
- Great for headers and sidebars

---

### Section 12: Z-Index {#section-12}

## Layering Elements
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

## Important Rules
- Only works on positioned elements
- Higher z-index appears on top
- Can be negative
- Default z-index is auto

## Use Cases
- Modals
- Dropdowns
- Tooltips
- Overlapping elements

---

## Part 2: Practical Application (1.5 Hours)

### Exercise 1: CSS Inheritance Practice (15 minutes)

**Task:**
Create a page demonstrating inheritance properties.

**HTML:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>CSS Inheritance</title>
    <link rel="stylesheet" href="inheritance.css">
</head>
<body>
    <div class="parent">
        <h2>Parent Element</h2>
        <p>This paragraph inherits styles from parent.</p>
        <div class="child">
            <h3>Child Element</h3>
            <p>This paragraph inherits from both parent and child.</p>
            <span class="override">This span overrides inherited color.</span>
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
    padding: 20px;
    background-color: #f3f4f6;
}

.parent {
    color: #2563eb;
    font-size: 18px;
    line-height: 1.6;
    background-color: white;
    padding: 20px;
    border-radius: 8px;
    margin-bottom: 20px;
}

.parent h2 {
    color: #1f2937; /* Overrides inherited color */
}

.child {
    color: #10b981; /* Child inherits from parent but overrides color */
    padding: 15px;
    background-color: #f3f4f6;
    border-radius: 4px;
    margin-top: 10px;
}

.child h3 {
    color: #1f2937;
}

.override {
    color: #e11d48; /* Explicit override */
    font-weight: bold;
}
```

---

### Exercise 2: Typography Complete (20 minutes)

**Task:**
Create a typography showcase with all font properties.

**HTML:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Typography Practice</title>
    <link rel="stylesheet" href="typography.css">
</head>
<body>
    <div class="container">
        <h1 class="main-title">Typography Showcase</h1>
        
        <h2>Font Families</h2>
        <p class="serif">Serif font: Georgia, Times New Roman</p>
        <p class="sans-serif">Sans-serif font: Arial, Helvetica</p>
        <p class="monospace">Monospace font: Courier New</p>
        
        <h2>Font Sizes</h2>
        <p class="small">Small text (12px)</p>
        <p class="medium">Medium text (16px)</p>
        <p class="large">Large text (24px)</p>
        <p class="responsive">Responsive text (2rem)</p>
        
        <h2>Font Styles</h2>
        <p class="normal">Normal style</p>
        <p class="italic">Italic style</p>
        <p class="oblique">Oblique style</p>
        
        <h2>Font Weights</h2>
        <p class="light">Light weight (300)</p>
        <p class="normal-weight">Normal weight (400)</p>
        <p class="bold">Bold weight (700)</p>
        
        <h2>Font Variants</h2>
        <p class="small-caps">Small Caps Text</p>
        <p class="normal-variant">Normal Text</p>
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
    padding: 40px 20px;
}

.container {
    max-width: 800px;
    margin: 0 auto;
    background-color: white;
    padding: 40px;
    border-radius: 12px;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

.main-title {
    color: #2563eb;
    text-align: center;
    margin-bottom: 40px;
}

h2 {
    color: #1f2937;
    margin-top: 30px;
    margin-bottom: 15px;
    border-bottom: 2px solid #e5e7eb;
    padding-bottom: 10px;
}

/* Font Families */
.serif {
    font-family: Georgia, "Times New Roman", Times, serif;
    margin-bottom: 10px;
}

.sans-serif {
    font-family: Arial, "Helvetica Neue", Helvetica, sans-serif;
    margin-bottom: 10px;
}

.monospace {
    font-family: "Courier New", Courier, monospace;
    margin-bottom: 10px;
}

/* Font Sizes */
.small {
    font-size: 12px;
    margin-bottom: 10px;
}

.medium {
    font-size: 16px;
    margin-bottom: 10px;
}

.large {
    font-size: 24px;
    margin-bottom: 10px;
}

.responsive {
    font-size: 2rem;
    margin-bottom: 10px;
}

/* Font Styles */
.normal {
    font-style: normal;
    margin-bottom: 10px;
}

.italic {
    font-style: italic;
    margin-bottom: 10px;
}

.oblique {
    font-style: oblique;
    margin-bottom: 10px;
}

/* Font Weights */
.light {
    font-weight: 300;
    margin-bottom: 10px;
}

.normal-weight {
    font-weight: 400;
    margin-bottom: 10px;
}

.bold {
    font-weight: 700;
    margin-bottom: 10px;
}

/* Font Variants */
.small-caps {
    font-variant: small-caps;
    margin-bottom: 10px;
}

.normal-variant {
    font-variant: normal;
    margin-bottom: 10px;
}
```

---

### Exercise 3: CSS Units Practice (15 minutes)

**Task:**
Create boxes demonstrating different CSS units.

**HTML:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>CSS Units Practice</title>
    <link rel="stylesheet" href="units.css">
</head>
<body>
    <h1>CSS Units Examples</h1>
    
    <h2>Absolute Units</h2>
    <div class="box px">100px width</div>
    <div class="box pt">12pt font size</div>
    
    <h2>Relative Units</h2>
    <div class="box em">2em width (parent-based)</div>
    <div class="box rem">2rem width (root-based)</div>
    <div class="box percent">50% width</div>
    
    <h2>Viewport Units</h2>
    <div class="box vw">50vw width</div>
    <div class="box vh">30vh height</div>
    
    <h2>Calculation</h2>
    <div class="box calc">calc(100% - 40px)</div>
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
    margin-bottom: 30px;
    color: #1f2937;
}

h2 {
    color: #1f2937;
    margin-top: 30px;
    margin-bottom: 15px;
}

.box {
    background-color: white;
    border: 2px solid #2563eb;
    border-radius: 8px;
    display: flex;
    align-items: center;
    justify-content: center;
    margin: 10px 0;
    color: #1f2937;
    font-weight: bold;
}

/* Absolute Units */
.px {
    width: 100px;
    height: 60px;
}

.pt {
    width: 200px;
    height: 60px;
    font-size: 12pt;
}

/* Relative Units */
.em {
    width: 2em;
    height: 60px;
}

.rem {
    width: 2rem;
    height: 60px;
}

.percent {
    width: 50%;
    height: 60px;
}

/* Viewport Units */
.vw {
    width: 50vw;
    height: 60px;
}

.vh {
    width: 200px;
    height: 30vh;
}

/* Calculation */
.calc {
    width: calc(100% - 40px);
    height: 60px;
}
```

---

### Exercise 4: Position Practice (25 minutes)

**Task:**
Create examples demonstrating all position types.

**HTML:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Position Practice</title>
    <link rel="stylesheet" href="position.css">
</head>
<body>
    <h1>Position Examples</h1>
    
    <h2>Static (Default)</h2>
    <div class="static-demo">
        <div class="static-box">Static Box 1</div>
        <div class="static-box">Static Box 2</div>
        <div class="static-box">Static Box 3</div>
    </div>
    
    <h2>Relative</h2>
    <div class="relative-demo">
        <div class="relative-box">Relative Box (moved 10px down, 20px right)</div>
        <div class="static-box">Normal Box</div>
    </div>
    
    <h2>Absolute</h2>
    <div class="absolute-demo">
        <div class="absolute-box">Absolute Box (top-right of parent)</div>
        <div class="static-box">Normal Box</div>
    </div>
    
    <h2>Fixed</h2>
    <div class="fixed-demo">
        <div class="fixed-box">Fixed Box (always top-right of viewport)</div>
        <p>Scroll down to see the fixed box stay in place...</p>
        <div class="scroll-content">Scroll content...</div>
    </div>
    
    <h2>Sticky</h2>
    <div class="sticky-demo">
        <div class="sticky-header">Sticky Header (sticks at top)</div>
        <div class="scroll-content">
            <p>Scroll down to see the header stick...</p>
            <!-- Add more content to enable scrolling -->
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
    padding: 20px;
    background-color: #f3f4f6;
}

h1 {
    text-align: center;
    margin-bottom: 30px;
    color: #1f2937;
}

h2 {
    color: #1f2937;
    margin-top: 30px;
    margin-bottom: 15px;
}

/* Static Demo */
.static-demo {
    background-color: white;
    padding: 20px;
    border-radius: 8px;
    margin-bottom: 20px;
}

.static-box {
    background-color: #2563eb;
    color: white;
    padding: 15px;
    margin: 10px 0;
    border-radius: 4px;
}

/* Relative Demo */
.relative-demo {
    background-color: white;
    padding: 20px;
    border-radius: 8px;
    margin-bottom: 20px;
    position: relative;
}

.relative-box {
    background-color: #10b981;
    color: white;
    padding: 15px;
    margin: 10px 0;
    border-radius: 4px;
    position: relative;
    top: 10px;
    left: 20px;
}

/* Absolute Demo */
.absolute-demo {
    background-color: white;
    padding: 20px;
    border-radius: 8px;
    margin-bottom: 20px;
    position: relative;
    height: 150px;
}

.absolute-box {
    background-color: #f59e0b;
    color: white;
    padding: 15px;
    border-radius: 4px;
    position: absolute;
    top: 10px;
    right: 10px;
}

/* Fixed Demo */
.fixed-demo {
    background-color: white;
    padding: 20px;
    border-radius: 8px;
    margin-bottom: 20px;
    height: 300px;
    overflow-y: auto;
}

.fixed-box {
    background-color: #e11d48;
    color: white;
    padding: 15px;
    border-radius: 4px;
    position: fixed;
    top: 10px;
    right: 10px;
    z-index: 1000;
}

.scroll-content {
    height: 500px;
    background-color: #f3f4f6;
    padding: 20px;
    border-radius: 4px;
    margin-top: 20px;
}

/* Sticky Demo */
.sticky-demo {
    background-color: white;
    border-radius: 8px;
    margin-bottom: 20px;
    overflow-y: auto;
    height: 300px;
}

.sticky-header {
    background-color: #7c3aed;
    color: white;
    padding: 15px;
    position: sticky;
    top: 0;
    z-index: 100;
}
```

---

### Exercise 5: Z-Index Practice (15 minutes)

**Task:**
Create overlapping elements with different z-index values.

**HTML:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Z-Index Practice</title>
    <link rel="stylesheet" href="zindex.css">
</head>
<body>
    <h1>Z-Index Examples</h1>
    
    <h2>Layering Order</h2>
    <div class="layer-container">
        <div class="layer bottom">Bottom Layer (z-index: 1)</div>
        <div class="layer middle">Middle Layer (z-index: 2)</div>
        <div class="layer top">Top Layer (z-index: 3)</div>
    </div>
    
    <h2>Negative Z-Index</h2>
    <div class="negative-container">
        <div class="background-layer">Background (z-index: -1)</div>
        <div class="foreground-layer">Foreground (z-index: 1)</div>
    </div>
    
    <h2>Positioning Required</h2>
    <div class="position-container">
        <div class="no-position">No Position (z-index won't work)</div>
        <div class="with-position">With Position (z-index: 10)</div>
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
    margin-bottom: 30px;
    color: #1f2937;
}

h2 {
    color: #1f2937;
    margin-top: 30px;
    margin-bottom: 15px;
}

/* Layer Container */
.layer-container {
    position: relative;
    height: 200px;
    background-color: white;
    border-radius: 8px;
    margin-bottom: 20px;
}

.layer {
    position: absolute;
    width: 200px;
    height: 100px;
    display: flex;
    align-items: center;
    justify-content: center;
    color: white;
    font-weight: bold;
    border-radius: 8px;
}

.bottom {
    background-color: #2563eb;
    top: 20px;
    left: 20px;
    z-index: 1;
}

.middle {
    background-color: #10b981;
    top: 50px;
    left: 80px;
    z-index: 2;
}

.top {
    background-color: #f59e0b;
    top: 80px;
    left: 140px;
    z-index: 3;
}

/* Negative Container */
.negative-container {
    position: relative;
    height: 150px;
    background-color: white;
    border-radius: 8px;
    margin-bottom: 20px;
}

.background-layer {
    position: absolute;
    width: 100%;
    height: 100%;
    background-color: #e5e7eb;
    display: flex;
    align-items: center;
    justify-content: center;
    z-index: -1;
}

.foreground-layer {
    position: relative;
    background-color: #2563eb;
    color: white;
    padding: 15px;
    border-radius: 8px;
    margin: 20px;
    z-index: 1;
}

/* Position Container */
.position-container {
    position: relative;
    height: 150px;
    background-color: white;
    border-radius: 8px;
    margin-bottom: 20px;
}

.no-position {
    background-color: #ef4444;
    color: white;
    padding: 15px;
    border-radius: 8px;
    margin: 20px;
    z-index: 100; /* Won't work without position */
}

.with-position {
    position: relative;
    background-color: #10b981;
    color: white;
    padding: 15px;
    border-radius: 8px;
    margin: -50px 20px 20px 20px;
    z-index: 10; /* Will work */
}
```

---

### Exercise 6: Sticky Navigation (10 minutes)

**Task:**
Create a sticky navigation bar.

**HTML:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Sticky Navigation</title>
    <link rel="stylesheet" href="sticky-nav.css">
</head>
<body>
    <nav class="sticky-nav">
        <div class="logo">My Website</div>
        <ul class="nav-links">
            <li><a href="#home">Home</a></li>
            <li><a href="#about">About</a></li>
            <li><a href="#services">Services</a></li>
            <li><a href="#contact">Contact</a></li>
        </ul>
    </nav>
    
    <div class="content">
        <section id="home" class="section">
            <h1>Home Section</h1>
            <p>Scroll down to see the navigation stick to the top.</p>
        </section>
        
        <section id="about" class="section">
            <h1>About Section</h1>
            <p>About content goes here...</p>
        </section>
        
        <section id="services" class="section">
            <h1>Services Section</h1>
            <p>Services content goes here...</p>
        </section>
        
        <section id="contact" class="section">
            <h1>Contact Section</h1>
            <p>Contact content goes here...</p>
        </section>
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

/* Sticky Navigation */
.sticky-nav {
    position: sticky;
    top: 0;
    background-color: #2563eb;
    color: white;
    padding: 15px 30px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    z-index: 1000;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.logo {
    font-size: 24px;
    font-weight: bold;
}

.nav-links {
    list-style: none;
    display: flex;
    gap: 20px;
}

.nav-links a {
    color: white;
    text-decoration: none;
    font-weight: 500;
}

.nav-links a:hover {
    text-decoration: underline;
}

/* Content */
.content {
    max-width: 800px;
    margin: 0 auto;
    padding: 20px;
}

.section {
    background-color: white;
    padding: 40px;
    border-radius: 8px;
    margin-bottom: 20px;
    min-height: 400px;
}

.section h1 {
    color: #1f2937;
    margin-bottom: 20px;
}

.section p {
    color: #4b5563;
    line-height: 1.6;
}
```

---

## Part 3: Review, Questions, Problem Solving (0.5 Hours)

### Difference Between Absolute and Fixed (10 minutes)

**Absolute Positioning:**
- Positioned relative to nearest positioned ancestor
- If no positioned ancestor, relative to document body
- Scrolls with the page
- Removed from normal flow

**Fixed Positioning:**
- Always positioned relative to viewport
- Doesn't scroll with the page
- Great for headers, footers, modals
- Removed from normal flow

**Key Difference:**
- Absolute = relative to parent/ancestor
- Fixed = relative to browser window

---

### Review Questions (10 minutes)

**Question 1:** What is CSS inheritance?
**Answer:** Some CSS properties automatically pass from parent elements to their children.

**Question 2:** Which properties are commonly inherited?
**Answer:** color, font-family, font-size, line-height, text-align, visibility.

**Question 3:** What is the difference between `em` and `rem`?
**Answer:** `em` is relative to parent font size, `rem` is relative to root font size.

**Question 4:** What does `position: sticky` do?
**Answer:** Acts like relative until scroll position, then acts like fixed.

**Question 5:** When does `z-index` not work?
**Answer:** When the element doesn't have a position value (not static).

**Question 6:** What is the difference between absolute and fixed positioning?
**Answer:** Absolute is relative to positioned ancestor, fixed is relative to viewport.

**Question 7:** What does `calc()` allow you to do?
**Answer:** Perform calculations with different CSS units.

**Question 8:** What is the difference between `opacity` and `rgba`?
**Answer:** Opacity affects entire element, rgba affects only color transparency.

---

### Practice Challenges (5 minutes)

**Challenge 1:** Create a tooltip that appears when hovering over an element using absolute positioning.

**Challenge 2:** Create a modal overlay with fixed positioning and high z-index.

**Challenge 3:** Use `calc()` to create a responsive width that subtracts padding.

---

### Homework Assignment

**Task:** Create a complete card component with positioning and typography.

**Requirements:**
- Card with absolute positioned badge
- Typography using rem units
- Different font weights and styles
- Opacity hover effect
- calc() for responsive dimensions
- Z-index layering for overlap effects

**Due Date:** Next session

---

## End of Session 3

**Next Session:** Pseudo-classes/Elements + Cascade (List/Table Styling, Pseudo-classes, Pseudo-elements, Border-radius, Box-shadow, Forms Styling, Specificity, !important, Margin Collapse)
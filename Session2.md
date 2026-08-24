# Session 2: Dimensions and Visual Formatting

## Duration Breakdown (2.5 Hours Total)
- **1 Hour**: Theoretical Explanation + Live Coding (Sections 1-11)
- **1 Hour**: Interactive Learning (Sections 12-16 with Explain → Example → Try it yourself → Predict → Experiment → Challenge → Bug Hunting → Quiz)
- **20 Minutes**: Comprehensive Project (Single component combining all concepts)
- **10 Minutes**: Review & Assessment

---

## Part 1: Theoretical Explanation + Live Coding (1 Hour)

### Section 1: Width & Height {#section-1}

## Fixed Dimensions
```css
.box {
    width: 300px;
    height: 200px;
}
```

**Use when:** You need exact, predictable sizes.

---

## Percentage Dimensions
```css
.container {
    width: 80%;
    height: 50%;
}
```

**Use when:** You want responsive sizing relative to parent.

**Important:** Percentage height only works if parent has explicit height.

---

## Viewport Units
```css
.full-width {
    width: 100vw; /* Full viewport width */
}

.half-height {
    height: 50vh; /* Half viewport height */
}

.small-font {
    font-size: 4vw; /* Responsive font size */
}
```

**Viewport Units:**
- `vw` - 1% of viewport width
- `vh` - 1% of viewport height
- `vmin` - 1% of smaller viewport dimension
- `vmax` - 1% of larger viewport dimension

**Use when:** You want sizing relative to the browser window.

---

## Fit Content
```css
.container {
    width: fit-content;
    height: fit-content;
}
```

**Use when:** You want element to size based on its content.

---

## Max and Min Dimensions
```css
.responsive-box {
    min-width: 200px;
    max-width: 800px;
    width: 100%;
    
    min-height: 100px;
    max-height: 500px;
}
```

**Use when:** You want flexible but constrained sizing.

---

### Section 2: Overflow {#section-2}

## Overflow Values

### Visible (Default)
```css
.box {
    overflow: visible; /* Content shows outside box */
}
```

### Hidden
```css
.box {
    overflow: hidden; /* Content is clipped */
}
```

### Scroll
```css
.box {
    overflow: scroll; /* Always show scrollbars */
}
```

### Auto
```css
.box {
    overflow: auto; /* Scrollbars only when needed */
}
```

## Individual Axis Control
```css
.box {
    overflow-x: hidden;  /* Control horizontal */
    overflow-y: scroll;  /* Control vertical */
}
```

**Common Use Cases:**
- Image galleries
- Text containers
- Scrollable content areas
- Responsive layouts

---

### Section 3: Text Styling - Color {#section-3}

## Basic Color
```css
.text {
    color: #333333; /* Hex color */
}
```

## RGB Color
```css
.text {
    color: rgb(51, 51, 51);
}
```

## Color Names
```css
.text {
    color: darkblue;
}
```

---

### Section 4: Text Shadow {#section-4}

## Basic Text Shadow
```css
h1 {
    text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5);
}
```

**Syntax:** `offset-x offset-y blur-radius color`

## Multiple Shadows
```css
h1 {
    text-shadow: 
        2px 2px 0px #fff,
        4px 4px 0px #000;
}
```

**Use Cases:**
- Headings
- Buttons
- Logo text
- Special effects

---

### Section 5: Text Alignment {#section-5}

## Horizontal Alignment
```css
.left {
    text-align: left;
}

.center {
    text-align: center;
}

.right {
    text-align: right;
}

.justify {
    text-align: justify; /* Equal spacing between words */
}
```

## Vertical Alignment
```css
.container {
    display: flex;
    align-items: center; /* Flexbox method */
}

/* Or with line-height */
.text {
    line-height: 100px; /* Center text in 100px height */
}
```

---

### Section 6: Text Direction {#section-6}

## Writing Direction
```css
.ltr {
    direction: ltr; /* Left to right */
}

.rtl {
    direction: rtl; /* Right to left */
}
```

## Text Orientation
```css
.vertical {
    writing-mode: vertical-rl;
}
```

**Use Cases:**
- Multi-language websites
- Arabic/Hebrew content
- Vertical text effects

---

### Section 7: Vertical Alignment {#section-7}

## With Line-Height
```css
.center-vertical {
    height: 100px;
    line-height: 100px;
}
```

## With Flexbox
```css
.container {
    display: flex;
    align-items: center;
    justify-content: center;
}
```

## Vertical-Align Property
```css
/* Works with inline/inline-block elements */
.icon {
    vertical-align: middle;
}
```

**Values:** `top`, `middle`, `bottom`, `text-top`, `text-bottom`

---

### Section 8: Text Decoration {#section-8}

## Values
```css
.none {
    text-decoration: none; /* Remove underline from links */
}

.underline {
    text-decoration: underline;
}

.line-through {
    text-decoration: line-through;
}

.overline {
    text-decoration: overline;
}
```

## Multiple Decorations
```cssfancy {
    text-decoration: underline overline line-through;
}
```

**Use Cases:**
- Links (remove default underline)
- Strikethrough prices
- Special text effects

---

### Section 9: Text Transform {#section-9}

## Values
```css
.uppercase {
    text-transform: uppercase; /* ALL CAPS */
}

.lowercase {
    text-transform: lowercase; /* all lowercase */
}

.capitalize {
    text-transform: capitalize; /* First Letter Caps */
}
```

**Use Cases:**
- Headings
- Buttons
- Form labels
- Stylistic choices

---

### Section 10: Text Spacing {#section-10}

## Letter Spacing
```css
.wide {
    letter-spacing: 2px; /* Space between letters */
}

.tight {
    letter-spacing: -1px; /* Tighter spacing */
}
```

## Word Spacing
```css
.spaced {
    word-spacing: 10px; /* Space between words */
}
```

## Line Height
```css
.single {
    line-height: 1; /* Single spacing */
}

.double {
    line-height: 2; /* Double spacing */
}

.custom {
    line-height: 1.6; /* Custom spacing */
}
```

## Text Indent
```css
.indented {
    text-indent: 50px; /* First line indent */
}
```

## White Space
```css
.normal {
    white-space: normal; /* Default */
}

.nowrap {
    white-space: nowrap; /* No line breaks */
}

.pre {
    white-space: pre; /* Preserve formatting */
}

.pre-wrap {
    white-space: pre-wrap; /* Preserve but wrap */
}
```

---

### Section 11: Word Break {#section-11}

## Break Words
```css
.break-word {
    word-break: break-word; /* Break long words */
}

.break-all {
    word-break: break-all; /* Break anywhere */
}
```

**Use Cases:**
- Long URLs
- Mobile responsiveness
- Preventing overflow

---

### Section 12: Text Overflow {#section-12}

## Explain
Text overflow happens when text is too long for its container. CSS gives us tools to handle this gracefully:

- `text-overflow: ellipsis` - Shows "..." when text is cut off
- `text-overflow: clip` - Simply cuts off the text without any indicator
- Requires `white-space: nowrap` and `overflow: hidden` to work

## Example
```html
<div class="truncate">This is a very long text that will be truncated with ellipsis</div>
<div class="clip">This is a very long text that will be clipped without ellipsis</div>
```

```css
.truncate {
    width: 200px;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
}

.clip {
    width: 200px;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: clip;
}
```

## Try it yourself
Create a box that shows "..." when text is too long.

```html
<div class="my-box">Try to make this text show ellipsis when it overflows</div>
```

```css
.my-box {
    width: 150px;
    /* Add the CSS properties needed for ellipsis */
}
```

## Predict
What will happen with this code?

```css
.poor-ellipsis {
    width: 100px;
    text-overflow: ellipsis;
}
```

## Experiment
1. What happens if you remove `white-space: nowrap`?
2. What happens if you remove `overflow: hidden`?
3. Try using `text-overflow: clip` instead of `ellipsis`

## Challenge
**Task:** Create a product card component where:
- The product title truncates with ellipsis if it's too long
- The description shows at most 2 lines (hint: use `-webkit-line-clamp`)
- Time: 10 minutes
- Hint: You'll need `display: -webkit-box` and `-webkit-box-orient: vertical` for multi-line truncation

<details>
<summary>Solution</summary>

```html
<div class="product-card">
    <h3 class="product-title">Super Long Product Name That Should Be Truncated</h3>
    <p class="product-desc">This is a product description that should be limited to 2 lines maximum when displayed on the card.</p>
</div>
```

```css
.product-card {
    width: 250px;
    border: 1px solid #ddd;
    padding: 15px;
}

.product-title {
    width: 100%;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
    margin-bottom: 10px;
}

.product-desc {
    display: -webkit-box;
    -webkit-line-clamp: 2;
    -webkit-box-orient: vertical;
    overflow: hidden;
    line-height: 1.4;
}
```

</details>

## Bug Hunting
Find the bug in this code:

```css
.buggy-truncate {
    width: 150px;
    overflow: hidden;
    text-overflow: ellipsis;
    /* Why isn't this working? */
}
```

<details>
<summary>Solution</summary>

Missing `white-space: nowrap` - the text will wrap to multiple lines instead of staying on one line.

</details>

## Quiz
1. What two properties are required for `text-overflow: ellipsis` to work?
2. What's the difference between `ellipsis` and `clip`?
3. Can you use `text-overflow` without `overflow: hidden`?

<details>
<summary>Quiz Answers</summary>

1. `white-space: nowrap` and `overflow: hidden`
2. `ellipsis` shows "..." when cut off, `clip` just cuts off without indicator
3. No, `overflow: hidden` is required for `text-overflow` to work

</details>

---

### Section 13: Display Property {#section-13}

## Explain
The `display` property controls how elements are laid out and how they interact with each other:

- `block` - Takes full width, starts on new line
- `inline` - Takes only necessary width, stays on same line
- `inline-block` - Takes necessary width but can have dimensions
- `none` - Completely hides element (takes no space)

## Example
```html
<div class="block-demo">Block element</div>
<div class="block-demo">Another block</div>

<span class="inline-demo">Inline</span>
<span class="inline-demo">Inline</span>

<div class="inline-block-demo">Inline-block</div>
<div class="inline-block-demo">Inline-block</div>

<div class="hidden">This won't show</div>
```

```css
.block-demo {
    display: block;
    background: #e0e0e0;
    margin: 5px;
}

.inline-demo {
    display: inline;
    background: #90EE90;
    margin: 5px;
}

.inline-block-demo {
    display: inline-block;
    width: 100px;
    height: 50px;
    background: #87CEEB;
    margin: 5px;
}

.hidden {
    display: none;
}
```

## Try it yourself
Create three elements with different display values.

```html
<div class="first">First</div>
<div class="second">Second</div>
<div class="third">Third</div>
```

```css
.first {
    /* Make this a block element */
}

.second {
    /* Make this an inline element */
}

.third {
    /* Make this an inline-block with dimensions */
}
```

## Predict
What will happen with this code?

```css
.mystery {
    display: inline;
    width: 200px;
    height: 100px;
}
```

## Experiment
1. Try setting `width` on an inline element - what happens?
2. What's the difference between `display: none` and `visibility: hidden`?
3. Can you make a `div` behave like a `span`?

## Challenge
**Task:** Create a navigation bar where:
- Logo is on the left (block or inline-block)
- Navigation links are inline-block with equal spacing
- Mobile menu button is hidden on desktop, shown on mobile
- Time: 15 minutes
- Hint: Use media queries for the mobile menu button

<details>
<summary>Solution</summary>

```html
<nav class="navbar">
    <div class="logo">MyBrand</div>
    <div class="nav-links">
        <a href="#" class="nav-link">Home</a>
        <a href="#" class="nav-link">About</a>
        <a href="#" class="nav-link">Contact</a>
    </div>
    <button class="mobile-menu">☰</button>
</nav>
```

```css
.navbar {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 15px 20px;
    background: #333;
}

.logo {
    display: inline-block;
    color: white;
    font-size: 24px;
    font-weight: bold;
}

.nav-links {
    display: inline-block;
}

.nav-link {
    display: inline-block;
    color: white;
    text-decoration: none;
    padding: 10px 15px;
    margin: 0 5px;
}

.mobile-menu {
    display: none;
    background: none;
    border: none;
    color: white;
    font-size: 24px;
}

@media (max-width: 768px) {
    .nav-links {
        display: none;
    }
    .mobile-menu {
        display: inline-block;
    }
}
```

</details>

## Bug Hunting
Find the bug in this code:

```css
.button-group button {
    display: inline;
    width: 100px;
    height: 40px;
    /* Why aren't the dimensions working? */
}
```

<details>
<summary>Solution</summary>

Inline elements don't accept width/height. Change to `display: inline-block` or `display: block`.

</details>

## Quiz
1. Which display value allows you to set width and height?
2. What's the main difference between `display: none` and `visibility: hidden`?
3. Can inline elements have margins?

<details>
<summary>Quiz Answers</summary>

1. `inline-block` and `block` allow width/height
2. `display: none` removes element from layout (takes no space), `visibility: hidden` hides but keeps space
3. Yes, inline elements can have horizontal margins but not vertical margins

</details>

---

### Section 14: Backgrounds {#section-14}

## Explain
Background properties control what appears behind elements:

- `background-color` - Solid color background
- `background-image` - Image as background
- `background-repeat` - How image repeats (no-repeat, repeat-x, repeat-y)
- `background-position` - Where image is placed
- `background-size` - How image scales (cover, contain, custom)
- `background-attachment` - Whether image scrolls with content

## Example
```html
<div class="color-box">Solid Color</div>
<div class="image-box">Image Background</div>
<div class="gradient-box">Gradient</div>
```

```css
.color-box {
    background-color: #3498db;
    color: white;
    padding: 20px;
}

.image-box {
    background-image: url('https://via.placeholder.com/400');
    background-size: cover;
    color: white;
    padding: 20px;
}

.gradient-box {
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    color: white;
    padding: 20px;
}
```

## Try it yourself
Create a box with a gradient background.

```html
<div class="my-gradient">Gradient Box</div>
```

```css
.my-gradient {
    padding: 30px;
    /* Add a gradient background */
}
```

## Predict
What will happen with this code?

```css
.mystery-bg {
    background-image: url('image.jpg');
    background-size: cover;
    background-position: center;
}
```

## Experiment
1. What happens when you use `background-size: contain` vs `cover`?
2. Try using `background-attachment: fixed` - what does it do?
3. Create a gradient with 3 colors instead of 2

## Challenge
**Task:** Create a hero section with:
- Full-screen background image with cover
- Overlay gradient to make text readable
- Centered text content
- Background stays fixed when scrolling
- Time: 15 minutes
- Hint: Use multiple backgrounds or pseudo-elements for the overlay

<details>
<summary>Solution</summary>

```html
<div class="hero">
    <div class="hero-content">
        <h1>Welcome to My Website</h1>
        <p>Amazing content awaits you</p>
    </div>
</div>
```

```css
.hero {
    height: 100vh;
    background: 
        linear-gradient(rgba(0, 0, 0, 0.5), rgba(0, 0, 0, 0.5)),
        url('https://images.unsplash.com/photo-1506905925346-21bda4d32df4?w=1200');
    background-size: cover;
    background-position: center;
    background-attachment: fixed;
    display: flex;
    align-items: center;
    justify-content: center;
}

.hero-content {
    text-align: center;
    color: white;
}

.hero h1 {
    font-size: 48px;
    margin-bottom: 20px;
}

.hero p {
    font-size: 24px;
}
```

</details>

## Bug Hunting
Find the bug in this code:

```css
.buggy-bg {
    background: url('image.jpg');
    background-size: cover;
    width: 100%;
    height: 0;
    /* Why isn't the background showing? */
}
```

<details>
<summary>Solution</summary>

`height: 0` means the element has no height, so the background won't be visible. Set a proper height or use `min-height`.

</details>

## Quiz
1. What's the difference between `cover` and `contain` for background-size?
2. What does `background-attachment: fixed` do?
3. Can you use multiple background images?

<details>
<summary>Quiz Answers</summary>

1. `cover` scales image to cover entire area (may crop), `contain` fits entire image (may leave space)
2. `fixed` keeps background in place while content scrolls, creating a parallax effect
3. Yes, you can use comma-separated values for multiple background images

</details>

---

### Section 15: Advanced Colors {#section-15}

## Explain
CSS offers advanced color formats beyond basic hex codes:

- `rgba()` - RGB with alpha transparency (0-1)
- `hsla()` - Hue, Saturation, Lightness with alpha
- Named colors - Predefined color names
- `currentColor` - Inherits the element's text color
- Color contrast is important for accessibility

## Example
```html
<div class="rgba-box">RGBA Color</div>
<div class="hsla-box">HSLA Color</div>
<div class="named-box">Named Color</div>
<div class="current-box">Current Color Border</div>
```

```css
.rgba-box {
    color: rgba(255, 0, 0, 0.5); /* Red with 50% opacity */
    background: #f0f0f0;
    padding: 20px;
}

.hsla-box {
    color: hsla(120, 100%, 50%, 0.7); /* Green with 70% opacity */
    background: #f0f0f0;
    padding: 20px;
}

.named-box {
    color: tomato;
    background: lightblue;
    padding: 20px;
}

.current-box {
    color: blue;
    border: 3px solid currentColor;
    padding: 20px;
}
```

## Try it yourself
Create elements with different transparency levels.

```html
<div class="opacity-1">Level 1</div>
<div class="opacity-2">Level 2</div>
<div class="opacity-3">Level 3</div>
```

```css
.opacity-1 {
    background: rgba(255, 0, 0, 0.3);
    /* Set opacity to 30% */
}

.opacity-2 {
    background: rgba(0, 255, 0, 0.6);
    /* Set opacity to 60% */
}

.opacity-3 {
    background: rgba(0, 0, 255, 0.9);
    /* Set opacity to 90% */
}
```

## Predict
What will happen with this code?

```css
.parent {
    color: purple;
}

.child {
    border: 2px solid currentColor;
    color: green;
}
```

## Experiment
1. What happens when you use `rgba(255, 0, 0, 0)` vs `rgba(255, 0, 0, 1)`?
2. Try creating a color using HSLA - what does each value control?
3. Test different named colors - which ones work?

## Challenge
**Task:** Create a glassmorphism card with:
- Semi-transparent background using RGBA
- Blur effect (hint: backdrop-filter)
- Border with subtle transparency
- Text that remains readable
- Time: 15 minutes
- Hint: Use rgba for background and border

<details>
<summary>Solution</summary>

```html
<div class="glass-card">
    <h3>Glass Card</h3>
    <p>This card has a glassmorphism effect</p>
</div>
```

```css
body {
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    min-height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
}

.glass-card {
    background: rgba(255, 255, 255, 0.1);
    backdrop-filter: blur(10px);
    border: 1px solid rgba(255, 255, 255, 0.2);
    border-radius: 15px;
    padding: 30px;
    max-width: 300px;
    color: white;
    box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
}

.glass-card h3 {
    margin-top: 0;
    margin-bottom: 15px;
}

.glass-card p {
    margin: 0;
    opacity: 0.9;
}
```

</details>

## Bug Hunting
Find the bug in this code:

```css
.buggy-color {
    color: rgba(255, 0, 0, 1.5);
    /* Why might this cause issues? */
}
```

<details>
<summary>Solution</summary>

Alpha values should be between 0 and 1. Values above 1 are clamped to 1, but this shows a misunderstanding of the rgba format.

</details>

## Quiz
1. What does the alpha value in rgba represent?
2. What's the difference between hue and saturation in HSLA?
3. Why is color contrast important?

<details>
<summary>Quiz Answers</summary>

1. Alpha represents opacity/transparency (0 = transparent, 1 = opaque)
2. Hue is the color type (0-360 degrees), saturation is color intensity (0-100%)
3. Color contrast ensures text is readable for all users, especially those with visual impairments

</details>

---

### Section 16: CSS Reset {#section-16}

## Explain
Browsers have different default styles for elements. CSS Reset removes these defaults to ensure consistency:

- `margin: 0; padding: 0;` - Removes default spacing
- `box-sizing: border-box;` - Makes width/height include padding/border
- Reset vs Normalize - Reset removes everything, Normalize keeps useful defaults

## Example
```css
/* Simple CSS Reset */
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    font-family: Arial, sans-serif;
    line-height: 1.6;
}
```

## Try it yourself
Create a simple reset for your page.

```css
/* Add your reset here */
```

## Predict
What will happen to a `<ul>` element after applying this reset?

```css
* {
    margin: 0;
    padding: 0;
}
```

## Experiment
1. Create a page without reset - what default styles do you see?
2. Apply the reset - what changes?
3. Try removing the `box-sizing: border-box` - what happens to element dimensions?

## Challenge
**Task:** Create a comprehensive reset that:
- Removes all default margins and padding
- Sets consistent box-sizing
- Removes default list styles
- Sets default font family
- Removes default borders from images
- Time: 10 minutes
- Hint: Target specific elements like `ul`, `ol`, `img`, `button`

<details>
<summary>Solution</summary>

```css
/* Comprehensive CSS Reset */
*, *::before, *::after {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

html {
    font-size: 16px;
}

body {
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, sans-serif;
    line-height: 1.6;
    color: #333;
}

/* Remove list styles */
ul, ol {
    list-style: none;
}

/* Remove image borders */
img {
    max-width: 100%;
    height: auto;
    border: none;
}

/* Remove button defaults */
button {
    font: inherit;
    border: none;
    background: none;
    cursor: pointer;
}

/* Remove link underline by default */
a {
    text-decoration: none;
    color: inherit;
}

/* Remove heading default margins */
h1, h2, h3, h4, h5, h6 {
    margin: 0;
    font-weight: normal;
}
```

</details>

## Bug Hunting
Find the potential issue with this reset:

```css
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

/* What might this break? */
```

<details>
<summary>Solution</summary>

Using the universal selector `*` can affect pseudo-elements and may cause issues with some form elements. A more targeted approach is often better.

</details>

## Quiz
1. Why do we need CSS resets?
2. What's the difference between reset and normalize?
3. What does `box-sizing: border-box` do?

<details>
<summary>Quiz Answers</summary>

1. Browsers have different default styles; resets ensure consistency across browsers
2. Reset removes all defaults, Normalize preserves useful defaults while normalizing differences
3. Makes width/height include padding and border in the total dimension calculation

</details>

---

## Part 2: Comprehensive Project (20 minutes)

### Project: Modern Card Component

**Task:** Create a single, polished card component that demonstrates all visual formatting concepts:

**Requirements:**
1. **CSS Reset** - Implement a clean reset at the top
2. **Display Properties** - Use inline-block for layout elements
3. **Text Overflow** - Truncate long card titles with ellipsis
4. **Backgrounds** - Gradient background for the card header
5. **Advanced Colors** - Use RGBA for overlay and HSLA for accents
6. **Responsive** - Card adapts to different screen sizes

**Time:** 20 minutes
**Hint:** Start with the structure, then add styling layer by layer

<details>
<summary>Solution</summary>

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Modern Card</title>
    <link rel="stylesheet" href="card.css">
</head>
<body>
    <div class="card">
        <div class="card-header">
            <h3 class="card-title">Very Long Card Title That Should Be Truncated When It's Too Long</h3>
        </div>
        <div class="card-body">
            <p class="card-description">This is a card description that demonstrates proper text formatting and spacing.</p>
            <div class="card-footer">
                <span class="card-tag">Featured</span>
                <button class="card-button">Learn More</button>
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

body {
    font-family: Arial, sans-serif;
    line-height: 1.6;
    background: #f5f5f5;
    display: flex;
    align-items: center;
    justify-content: center;
    min-height: 100vh;
    padding: 20px;
}

/* Card Component */
.card {
    width: 100%;
    max-width: 350px;
    background: white;
    border-radius: 12px;
    overflow: hidden;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

/* Card Header with Gradient Background */
.card-header {
    background: linear-gradient(135deg, hsla(220, 80%, 60%, 1) 0%, hsla(260, 70%, 60%, 1) 100%);
    padding: 20px;
}

/* Text Overflow for Title */
.card-title {
    color: white;
    font-size: 18px;
    font-weight: bold;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
    margin: 0;
}

/* Card Body */
.card-body {
    padding: 20px;
}

.card-description {
    color: #333;
    line-height: 1.6;
    margin-bottom: 20px;
}

/* Card Footer */
.card-footer {
    display: flex;
    justify-content: space-between;
    align-items: center;
}

/* Tag with RGBA */
.card-tag {
    display: inline-block;
    background: rgba(99, 102, 241, 0.1);
    color: hsla(239, 80%, 60%, 1);
    padding: 5px 12px;
    border-radius: 20px;
    font-size: 12px;
    font-weight: bold;
}

/* Button with Display Property */
.card-button {
    display: inline-block;
    background: hsla(239, 80%, 60%, 1);
    color: white;
    padding: 8px 16px;
    border-radius: 6px;
    font-size: 14px;
    cursor: pointer;
    transition: background 0.3s;
}

.card-button:hover {
    background: hsla(239, 80%, 50%, 1);
}
```

</details>

---

## Part 3: Review & Assessment (10 minutes)

### Quick Review Quiz

**Question 1:** What two properties are required for `text-overflow: ellipsis` to work?
<details>
<summary>Answer</summary>

`white-space: nowrap` and `overflow: hidden`

</details>

**Question 2:** What's the difference between `display: none` and `visibility: hidden`?
<details>
<summary>Answer</summary>

`display: none` removes element from layout (takes no space), `visibility: hidden` hides but keeps space

</details>

**Question 3:** What does `background-size: cover` do?
<details>
<summary>Answer</summary>

Scales the background image to cover the entire container, potentially cropping parts of the image

</details>

**Question 4:** What does the alpha value in `rgba()` represent?
<details>
<summary>Answer</summary>

Opacity/transparency (0 = transparent, 1 = opaque)

</details>

**Question 5:** Why do we need CSS resets?
<details>
<summary>Answer</summary>

Browsers have different default styles; resets ensure consistency across browsers

</details>

### Final Challenge

**Task:** Create a complete component that uses ALL concepts from this session:
- Text overflow for long content
- Multiple display properties
- Background techniques (gradient + image)
- Advanced colors (RGBA/HSLA)
- CSS reset

**Time:** 10 minutes
**Goal:** Build a user profile card component

<details>
<summary>Solution Hint</summary>

Think about what a profile card needs: avatar (background), name (might overflow), bio (text styling), buttons (display properties), background colors (advanced colors)

</details>

---

## Homework Assignment

**Task:** Create a styled blog post card using all visual formatting concepts.

**Requirements:**
- External CSS with reset
- Blog card with:
  - Featured image (background image with cover)
  - Blog title (text styling with shadow + overflow handling)
  - Excerpt (proper line-height and spacing)
  - Read more button (inline-block with hover effect)
- Use advanced colors (RGBA/HSLA)
- Implement text overflow for long titles
- Use proper display values
- Add background effects (gradient or pattern)

**Due Date:** Next session

---

## End of Session 2

**Summary:** In this session, you learned about visual formatting in CSS including text overflow, display properties, backgrounds, advanced colors, and CSS resets. Each concept was practiced through interactive exercises (Explain → Example → Try it yourself → Predict → Experiment → Challenge → Bug Hunting → Quiz), followed by a comprehensive project that integrated all concepts.

**Next Session:** Inheritance, Typography, Positioning (CSS Inheritance, Font Properties, CSS Units, Position types, Z-Index)
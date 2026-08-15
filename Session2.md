# Session 2: Dimensions and Visual Formatting

## Duration Breakdown
- **1 Hour**: Theoretical Explanation + Live Coding
- **1.5 Hours**: Practical Application (Student writes code)
- **0.5 Hours**: Review, Questions, Problem Solving

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

## Ellipsis
```css
.truncate {
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis; /* Shows "..." */
}
```

## Clip
```css
.clip {
    white-space: nowrap;
    overflow: hidden;
    text-overflow: clip; /* Simply cuts off */
}
```

**Use Cases:**
- Product titles
- File names
- Limited space content

---

### Section 13: Display Property {#section-13}

## Common Display Values

### Display: Block
```css
.block {
    display: block;
}
```
- Takes full width
- Starts on new line
- Examples: `div`, `p`, `h1-h6`

### Display: Inline
```css
.inline {
    display: inline;
}
```
- Takes only necessary width
- Stays on same line
- Examples: `span`, `a`, `strong`

### Display: Inline-Block
```css
.inline-block {
    display: inline-block;
}
```
- Takes only necessary width
- Can have width/height
- Stays on same line
- Best of both worlds

### Display: None
```css
.hidden {
    display: none;
}
```
- Element is completely hidden
- Takes no space in layout
- Different from `visibility: hidden`

## When to Use Each

| Display Type | Use When | Examples |
|-------------|----------|----------|
| block | Layout containers, sections | div, p, header |
| inline | Text within content | span, a, strong |
| inline-block | Buttons, small widgets | button, input |
| none | Hidden elements, menus | hidden content |

---

### Section 14: Backgrounds {#section-14}

## Background Color
```css
.box {
    background-color: #f0f0f0;
}
```

## Background Image
```css
.box {
    background-image: url('image.jpg');
}
```

## Background Repeat
```css
.no-repeat {
    background-repeat: no-repeat;
}

.repeat-x {
    background-repeat: repeat-x;
}

.repeat-y {
    background-repeat: repeat-y;
}
```

## Background Position
```css
.center {
    background-position: center;
}

.custom {
    background-position: 20px 30px; /* x y */
}
```

## Background Size
```css
.cover {
    background-size: cover; /* Cover entire area */
}

.contain {
    background-size: contain; /* Fit entire image */
}

.custom {
    background-size: 50% 50%;
}
```

## Background Attachment
```css
.fixed {
    background-attachment: fixed; /* Doesn't scroll */
}

.scroll {
    background-attachment: scroll; /* Scrolls with content */
}
```

## Background Shorthand
```css
.box {
    background: 
        url('image.jpg') 
        no-repeat 
        center 
        / cover 
        fixed;
}
```

**Order:** color image repeat position / size attachment

---

### Section 15: Advanced Colors {#section-15}

## RGBA Colors
```css
.transparent {
    color: rgba(255, 0, 0, 0.5); /* Red with 50% opacity */
}
```

**Syntax:** `rgba(red, green, blue, alpha)`

## HSLA Colors
```css
.hsla-color {
    color: hsla(120, 100%, 50%, 0.7);
}
```

**Syntax:** `hsla(hue, saturation, lightness, alpha)`

## Named Colors
```css
.named {
    color: tomato;
    background-color: lightblue;
}
```

## CurrentColor
```css
.parent {
    color: blue;
}

.child {
    border: 2px solid currentColor; /* Inherits parent's color */
}
```

## Color Contrast (Accessibility)
```css
/* Good contrast */
.good {
    color: #000000;
    background-color: #ffffff;
}

/* Poor contrast */
.bad {
    color: #cccccc;
    background-color: #dddddd;
}
```

**Guideline:** WCAG AA requires 4.5:1 contrast ratio for normal text.

---

### Section 16: CSS Reset {#section-16}

## Why Use a Reset?
- Browsers have different default styles
- Ensures consistency across browsers
- Provides clean slate for styling

## Simple CSS Reset
```css
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}
```

## Box Sizing Reset
```css
*, *::before, *::after {
    box-sizing: border-box;
}
```

## CSS Normalize
- More sophisticated than reset
- Preserves useful defaults
- Normalizes differences between browsers

**When to Use Reset vs Normalize:**
- Reset: When you want complete control
- Normalize: When you want sensible defaults

---

## Part 2: Practical Application (1.5 Hours)

### Exercise 1: Width/Height Practice (20 minutes)

**Task:**
Create boxes demonstrating different dimension units.

**HTML:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Dimensions Practice</title>
    <link rel="stylesheet" href="dimensions.css">
</head>
<body>
    <h1>Width & Height Examples</h1>
    
    <div class="box fixed">Fixed: 300px × 200px</div>
    <div class="box percentage">Percentage: 80% × 50%</div>
    <div class="box viewport">Viewport: 50vw × 30vh</div>
    <div class="box constrained">Min: 200px, Max: 400px</div>
    <div class="box fit-content">Fit Content</div>
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

.box {
    background-color: white;
    border: 2px solid #2563eb;
    border-radius: 8px;
    display: flex;
    align-items: center;
    justify-content: center;
    margin: 20px 0;
    color: #1f2937;
    font-weight: bold;
}

.fixed {
    width: 300px;
    height: 200px;
}

.percentage {
    width: 80%;
    height: 200px;
}

.viewport {
    width: 50vw;
    height: 30vh;
}

.constrained {
    width: 100%;
    min-width: 200px;
    max-width: 400px;
    height: 150px;
}

.fit-content {
    width: fit-content;
    height: fit-content;
    padding: 20px;
}
```

---

### Exercise 2: Overflow Practice (15 minutes)

**Task:**
Create containers demonstrating different overflow values.

**HTML:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Overflow Practice</title>
    <link rel="stylesheet" href="overflow.css">
</head>
<body>
    <h1>Overflow Examples</h1>
    
    <div class="container">
        <h2>Visible (Default)</h2>
        <div class="box visible">
            This content is very long and will overflow the container because overflow is set to visible.
        </div>
    </div>
    
    <div class="container">
        <h2>Hidden</h2>
        <div class="box hidden">
            This content is very long and will be clipped because overflow is set to hidden.
        </div>
    </div>
    
    <div class="container">
        <h2>Scroll</h2>
        <div class="box scroll">
            This content is very long and will show scrollbars because overflow is set to scroll. Scrollbars are always visible even if not needed.
        </div>
    </div>
    
    <div class="container">
        <h2>Auto</h2>
        <div class="box auto">
            This content is very long and will show scrollbars only when needed because overflow is set to auto.
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

.container {
    margin: 30px 0;
}

.container h2 {
    margin-bottom: 10px;
    color: #1f2937;
}

.box {
    width: 300px;
    height: 100px;
    background-color: white;
    border: 2px solid #2563eb;
    border-radius: 8px;
    padding: 15px;
}

.visible {
    overflow: visible;
}

.hidden {
    overflow: hidden;
}

.scroll {
    overflow: scroll;
}

.auto {
    overflow: auto;
}
```

---

### Exercise 3: Text Styling Complete (25 minutes)

**Task:**
Create a text-styling showcase with all text properties.

**HTML:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Text Styling Practice</title>
    <link rel="stylesheet" href="text-styling.css">
</head>
<body>
    <div class="container">
        <h1 class="main-title">Text Styling Showcase</h1>
        
        <h2 class="section-title">Color & Shadow</h2>
        <p class="colored-text">This text has a custom color with shadow effect.</p>
        
        <h2 class="section-title">Alignment</h2>
        <p class="left-align">Left aligned text</p>
        <p class="center-align">Center aligned text</p>
        <p class="right-align">Right aligned text</p>
        
        <h2 class="section-title">Decoration</h2>
        <p class="underline">Underlined text</p>
        <p class="line-through">Strikethrough text</p>
        <p class="overline">Overlined text</p>
        
        <h2 class="section-title">Transform</h2>
        <p class="uppercase">UPPERCASE TEXT</p>
        <p class="lowercase">LOWERCASE TEXT</p>
        <p class="capitalize">Capitalized Text</p>
        
        <h2 class="section-title">Spacing</h2>
        <p class="letter-spacing">Wide letter spacing</p>
        <p class="word-spacing">Wide word spacing</p>
        <p class="line-height">Custom line height for better readability</p>
        
        <h2 class="section-title">Overflow</h2>
        <p class="truncate">This is a very long text that will be truncated with ellipsis when it overflows the container.</p>
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
    text-shadow: 2px 2px 4px rgba(37, 99, 235, 0.3);
}

.section-title {
    color: #1f2937;
    margin-top: 30px;
    margin-bottom: 15px;
    border-bottom: 2px solid #e5e7eb;
    padding-bottom: 10px;
}

/* Color & Shadow */
.colored-text {
    color: #7c3aed;
    text-shadow: 1px 1px 2px rgba(124, 58, 237, 0.3);
}

/* Alignment */
.left-align {
    text-align: left;
}

.center-align {
    text-align: center;
}

.right-align {
    text-align: right;
}

/* Decoration */
.underline {
    text-decoration: underline;
    color: #2563eb;
}

.line-through {
    text-decoration: line-through;
    color: #dc2626;
}

.overline {
    text-decoration: overline;
    color: #059669;
}

/* Transform */
.uppercase {
    text-transform: uppercase;
    color: #1f2937;
}

.lowercase {
    text-transform: lowercase;
    color: #4b5563;
}

.capitalize {
    text-transform: capitalize;
    color: #7c3aed;
}

/* Spacing */
.letter-spacing {
    letter-spacing: 3px;
    color: #1f2937;
}

.word-spacing {
    word-spacing: 15px;
    color: #1f2937;
}

.line-height {
    line-height: 2;
    color: #4b5563;
}

/* Overflow */
.truncate {
    width: 100%;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
    background-color: #f3f4f6;
    padding: 10px;
    border-radius: 4px;
}
```

---

### Exercise 4: Display Property Practice (20 minutes)

**Task:**
Create examples demonstrating different display values.

**HTML:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Display Property Practice</title>
    <link rel="stylesheet" href="display.css">
</head>
<body>
    <h1>Display Property Examples</h1>
    
    <h2>Block Elements</h2>
    <div class="block-example">
        <div class="block-item">Block Item 1</div>
        <div class="block-item">Block Item 2</div>
        <div class="block-item">Block Item 3</div>
    </div>
    
    <h2>Inline Elements</h2>
    <div class="inline-example">
        <span class="inline-item">Inline Item 1</span>
        <span class="inline-item">Inline Item 2</span>
        <span class="inline-item">Inline Item 3</span>
    </div>
    
    <h2>Inline-Block Elements</h2>
    <div class="inline-block-example">
        <div class="inline-block-item">Inline-Block 1</div>
        <div class="inline-block-item">Inline-Block 2</div>
        <div class="inline-block-item">Inline-Block 3</div>
    </div>
    
    <h2>None (Hidden)</h2>
    <div class="none-example">
        <div class="visible-item">Visible Item</div>
        <div class="hidden-item">Hidden Item (display: none)</div>
        <div class="visible-item">Another Visible Item</div>
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

/* Block Example */
.block-example {
    background-color: white;
    padding: 20px;
    border-radius: 8px;
    margin-bottom: 20px;
}

.block-item {
    display: block;
    background-color: #2563eb;
    color: white;
    padding: 15px;
    margin: 10px 0;
    border-radius: 4px;
}

/* Inline Example */
.inline-example {
    background-color: white;
    padding: 20px;
    border-radius: 8px;
    margin-bottom: 20px;
}

.inline-item {
    display: inline;
    background-color: #10b981;
    color: white;
    padding: 10px;
    margin: 5px;
    border-radius: 4px;
}

/* Inline-Block Example */
.inline-block-example {
    background-color: white;
    padding: 20px;
    border-radius: 8px;
    margin-bottom: 20px;
}

.inline-block-item {
    display: inline-block;
    background-color: #f59e0b;
    color: white;
    padding: 15px;
    margin: 5px;
    border-radius: 4px;
    width: 100px;
    text-align: center;
}

/* None Example */
.none-example {
    background-color: white;
    padding: 20px;
    border-radius: 8px;
}

.visible-item {
    display: block;
    background-color: #2563eb;
    color: white;
    padding: 15px;
    margin: 10px 0;
    border-radius: 4px;
}

.hidden-item {
    display: none;
    background-color: #dc2626;
    color: white;
    padding: 15px;
    margin: 10px 0;
    border-radius: 4px;
}
```

---

### Exercise 5: Backgrounds Complete (20 minutes)

**Task:**
Create a showcase of different background techniques.

**HTML:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Backgrounds Practice</title>
    <link rel="stylesheet" href="backgrounds.css">
</head>
<body>
    <h1>Background Examples</h1>
    
    <h2>Solid Color</h2>
    <div class="box solid-color">Solid Background Color</div>
    
    <h2>Gradient Background</h2>
    <div class="box gradient">Linear Gradient</div>
    
    <h2>Background Image (Cover)</h2>
    <div class="box image-cover">Background Image - Cover</div>
    
    <h2>Background Image (Contain)</h2>
    <div class="box image-contain">Background Image - Contain</div>
    
    <h2>Fixed Background</h2>
    <div class="box fixed-bg">Fixed Background Attachment</div>
    
    <h2>RGBA Background</h2>
    <div class="box rgba-bg">RGBA Background with Transparency</div>
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
    height: 200px;
    border-radius: 12px;
    display: flex;
    align-items: center;
    justify-content: center;
    color: white;
    font-size: 18px;
    font-weight: bold;
    text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5);
    margin-bottom: 20px;
}

/* Solid Color */
.solid-color {
    background-color: #2563eb;
}

/* Gradient */
.gradient {
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
}

/* Image Cover */
.image-cover {
    background: url('https://images.unsplash.com/photo-1506905925346-21bda4d32df4?w=800') no-repeat center;
    background-size: cover;
}

/* Image Contain */
.image-contain {
    background: url('https://images.unsplash.com/photo-1506905925346-21bda4d32df4?w=800') no-repeat center;
    background-size: contain;
    background-color: #1f2937;
}

/* Fixed Background */
.fixed-bg {
    background: url('https://images.unsplash.com/photo-1506905925346-21bda4d32df4?w=800') no-repeat center;
    background-size: cover;
    background-attachment: fixed;
}

/* RGBA Background */
.rgba-bg {
    background-color: rgba(37, 99, 235, 0.7);
    border: 2px solid #2563eb;
}
```

---

### Exercise 6: CSS Reset Implementation (10 minutes)

**Task:**
Create a page with and without CSS reset to see the difference.

**HTML (without reset):**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Without Reset</title>
</head>
<body>
    <h1>Without CSS Reset</h1>
    <p>This page uses browser default styles.</p>
    <ul>
        <li>Item 1</li>
        <li>Item 2</li>
    </ul>
</body>
</html>
```

**HTML (with reset):**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>With Reset</title>
    <link rel="stylesheet" href="reset.css">
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <h1>With CSS Reset</h1>
    <p>This page uses CSS reset for consistent styling.</p>
    <ul>
        <li>Item 1</li>
        <li>Item 2</li>
    </ul>
</body>
</html>
```

**CSS (reset.css):**
```css
/* Simple CSS Reset */
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

html, body {
    height: 100%;
}

body {
    font-family: Arial, sans-serif;
    line-height: 1.6;
}
```

**CSS (styles.css):**
```css
body {
    padding: 20px;
    background-color: #f3f4f6;
}

h1 {
    color: #2563eb;
    margin-bottom: 20px;
}

p {
    color: #4b5563;
    margin-bottom: 15px;
}

ul {
    list-style: none;
}

li {
    background-color: white;
    padding: 10px;
    margin: 5px 0;
    border-radius: 4px;
}
```

---

## Part 3: Review, Questions, Problem Solving (0.5 Hours)

### When to Use Each Display Type (10 minutes)

**Display: Block**
- Layout containers and sections
- When you want elements on new lines
- Examples: `div`, `p`, `header`, `section`

**Display: Inline**
- Text formatting within content
- When you want elements on same line
- Examples: `span`, `a`, `strong`, `em`

**Display: Inline-Block**
- Buttons and small widgets
- When you need width/height but same-line placement
- Examples: `button`, `input`, navigation items

**Display: None**
- Hidden content (menus, modals)
- When element should take no space
- Different from `visibility: hidden`

---

### Review Questions (10 minutes)

**Question 1:** What is the difference between `overflow: hidden` and `overflow: scroll`?
**Answer:** `hidden` clips content without scrollbars, `scroll` always shows scrollbars.

**Question 2:** What does `vw` and `vh` stand for?
**Answer:** Viewport Width and Viewport Height - 1% of the browser window dimensions.

**Question 3:** What is the correct order for background shorthand?
**Answer:** color image repeat position / size attachment

**Question 4:** What is the difference between `rgba` and `rgb`?
**Answer:** `rgba` includes an alpha channel for transparency, `rgb` does not.

**Question 5:** When should you use `display: inline-block` instead of `inline`?
**Answer:** When you need to set width/height on an element that should flow inline.

**Question 6:** What does `text-overflow: ellipsis` require to work?
**Answer:** `white-space: nowrap` and `overflow: hidden`

**Question 7:** What is the purpose of CSS reset?
**Answer:** To remove browser default styles and ensure consistency across browsers.

**Question 8:** What does `box-sizing: border-box` do?
**Answer:** Includes padding and border in the element's total width and height.

---

### Practice Challenges (5 minutes)

**Challenge 1:** Create a box with a gradient background that covers the entire area.

**Challenge 2:** Create text that truncates with ellipsis when it's too long.

**Challenge 3:** Create a button using `display: inline-block` with custom dimensions.

---

### Homework Assignment

**Task:** Create a styled blog post card using all visual formatting concepts.

**Requirements:**
- External CSS with reset
- Blog card with:
  - Featured image (background image with cover)
  - Blog title (text styling with shadow)
  - Excerpt (proper line-height and spacing)
  - Read more button (inline-block with hover effect)
- Use advanced colors (RGBA/HSLA)
- Implement text overflow for long titles
- Use proper display values
- Add background effects

**Due Date:** Next session

---

## End of Session 2

**Next Session:** Inheritance, Typography, Positioning (CSS Inheritance, Font Properties, CSS Units, Position types, Z-Index)
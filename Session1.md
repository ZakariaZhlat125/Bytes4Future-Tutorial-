# Session 1: CSS Fundamentals

## Duration Breakdown
- **1 Hour**: Theoretical Explanation + Live Coding
- **1.5 Hours**: Practical Application (Student writes code)
- **0.5 Hours**: Review, Questions, Problem Solving

---

## Part 1: Theoretical Explanation + Live Coding (1 Hour)

### Section 1: What is CSS?

**Definition:**
- CSS stands for **Cascading Style Sheets**
- CSS is used to style HTML elements and control the appearance of web pages
- CSS separates content (HTML) from presentation (styles)

**The Web Development Trinity:**
- **HTML** → The Skeleton 🦴 (Structure)
- **CSS** → The Design & Colors 🎨 (Style)
- **JavaScript** → The Brain & Interactions 🧠 (Functionality)

**Example Without CSS:**
```html
<h1>Welcome</h1>
<p>This is plain text without styling.</p>
```

**Example With CSS:**
```html
<h1 style="color: blue; font-size: 32px;">Welcome</h1>
<p style="color: gray; line-height: 1.6;">This is styled text.</p>
```

---

### Section 2: CSS and HTML Relationship {#section-2}

**How CSS Works with HTML:**
1. HTML provides the structure and content
2. CSS provides the styling and layout
3. The browser combines both to render the final page

**The "Cascading" in CSS:**
- "Cascading" means styles flow down from parent to child
- Multiple style sources can conflict
- CSS has rules to resolve conflicts

---

### Section 3: Ways to Add CSS {#section-3}

## 1. Inline CSS
Styles added directly to HTML elements using the `style` attribute.

```html
<h1 style="color: blue; font-size: 24px;">Blue Heading</h1>
<p style="color: red;">Red paragraph</p>
```

**Pros:**
- Quick for testing
- No separate files needed

**Cons:**
- Hard to maintain
- Repeats styles
- Mixes content with presentation
- Not recommended for production

---

## 2. Internal CSS
Styles added in the `<head>` section using the `<style>` tag.

```html
<!DOCTYPE html>
<html>
<head>
    <style>
        h1 {
            color: blue;
            font-size: 24px;
        }
        p {
            color: red;
        }
    </style>
</head>
<body>
    <h1>Blue Heading</h1>
    <p>Red paragraph</p>
</body>
</html>
```

**Pros:**
- Good for single-page websites
- Keeps styles separate from content
- Easy to test

**Cons:**
- Not reusable across pages
- Increases HTML file size

---

## 3. External CSS (Recommended)
Styles in a separate `.css` file linked to HTML.

**HTML file (index.html):**
```html
<!DOCTYPE html>
<html>
<head>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <h1>Blue Heading</h1>
    <p>Red paragraph</p>
</body>
</html>
```

**CSS file (styles.css):**
```css
h1 {
    color: blue;
    font-size: 24px;
}

p {
    color: red;
}
```

**Pros:**
- Reusable across multiple pages
- Separates concerns completely
- Easier to maintain
- Browser caching
- Industry standard

**Cons:**
- Requires additional HTTP request (minor)
- Need to manage multiple files

---

### Section 4: CSS Syntax {#section-4}

**Basic Structure:**
```css
selector {
    property: value;
    property: value;
}
```

**Example:**
```css
h1 {
    color: blue;
    font-size: 24px;
    text-align: center;
}
```

**Breaking Down the Syntax:**
- **Selector**: Targets HTML elements to style
- **Property**: The CSS attribute you want to change
- **Value**: The value for the property
- **Declaration**: Property + Value pair
- **Declaration Block**: All declarations inside `{ }`

**Rules:**
- Properties end with semicolons `;`
- Declaration blocks are enclosed in `{ }`
- Multiple declarations are separated by `;`
- CSS is case-insensitive (use lowercase for consistency)

---

### Section 5: CSS Selectors {#section-5}

## Element Selector
Targets all elements of a specific type.

```css
h1 {
    color: blue;
}

p {
    color: black;
}

button {
    background: green;
}
```

**Use when:** You want to style all elements of a certain type.

---

## Class Selector
Targets elements with a specific class attribute.

**HTML:**
```html
<div class="highlight">Important text</div>
<p class="highlight">Also important</p>
```

**CSS:**
```css
.highlight {
    background-color: yellow;
    font-weight: bold;
}
```

**Rules:**
- Class names start with `.` in CSS
- Multiple elements can have the same class
- One element can have multiple classes
- Class names should be descriptive

**Use when:** You want to style multiple different elements the same way.

---

## ID Selector
Targets a single element with a specific ID attribute.

**HTML:**
```html
<div id="main-header">Main Header</div>
```

**CSS:**
```css
#main-header {
    background: darkblue;
    color: white;
}
```

**Rules:**
- IDs start with `#` in CSS
- Each ID should be unique on a page
- One element can only have one ID
- IDs have higher specificity than classes

**Use when:** You need to target a single, unique element.

---

## Which Should You Use?

| Selector | Usage | Uniqueness | Example |
|----------|-------|------------|---------|
| Element | All elements of type | None | `h1`, `p` |
| Class | Multiple elements | Reusable | `.button`, `.card` |
| ID | Single element | Unique | `#header`, `#footer` |

**Best Practice:**
- Use **classes** for most styling
- Use **element selectors** for base styles
- Use **IDs** sparingly (only for unique elements like layout containers)

---

### Section 6: CSS Comments {#section-6}

**Single Line Comment:**
```css
/* This is a comment */
h1 {
    color: blue;
}
```

**Multi-line Comment:**
```css
/*
This is a multi-line comment
It can span multiple lines
Useful for explaining complex code
*/
p {
    color: black;
}
```

**Why Use Comments:**
- Explain your code
- Temporarily disable rules
- Organize large CSS files
- Help other developers understand your code

**Best Practice:**
- Comment complex logic
- Comment organization sections
- Don't over-comment obvious code

---

## Part 2: Practical Application (1.5 Hours)

### Exercise 1: Connect CSS to HTML (20 minutes)

**Task:**
1. Create an HTML file with basic content (from previous HTML lessons)
2. Create a separate CSS file
3. Link the CSS file to HTML
4. Add basic styles to verify the connection

**HTML (index.html):**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My First Styled Page</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <h1>Welcome to My Website</h1>
    <p>This is my first paragraph with CSS styling.</p>
    <button>Click Me</button>
</body>
</html>
```

**CSS (styles.css):**
```css
/* Basic styling to test the connection */
h1 {
    color: #2563eb;
    font-size: 32px;
}

p {
    color: #374151;
    line-height: 1.6;
}

button {
    background-color: #10b981;
    color: white;
    padding: 10px 20px;
    border: none;
    border-radius: 5px;
}
```

**Verification:**
- Open the HTML file in a browser
- Check if styles are applied
- If not working, verify the file path in the `href` attribute

---

### Exercise 2: Complete Box Model Application (30 minutes)

**Task:**
Create a card component demonstrating all Box Model properties.

**HTML:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Box Model Practice</title>
    <link rel="stylesheet" href="boxmodel.css">
</head>
<body>
    <div class="card">
        <h2>Box Model Card</h2>
        <p>This card demonstrates the complete Box Model with padding, margin, and border.</p>
    </div>
</body>
</html>
```

**CSS (boxmodel.css):**
```css
/* Reset for demo */
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    background-color: #f3f4f6;
    display: flex;
    justify-content: center;
    align-items: center;
    min-height: 100vh;
    font-family: Arial, sans-serif;
}

/* Card with complete Box Model */
.card {
    /* Content dimensions */
    width: 300px;
    height: auto;
    
    /* Padding - creates space inside the border */
    padding: 20px;
    
    /* Border - creates a border around the padding */
    border: 3px solid #2563eb;
    border-radius: 10px;
    
    /* Margin - creates space outside the border */
    margin: 20px;
    
    /* Background to visualize the content area */
    background-color: white;
    
    /* Text styling */
    color: #1f2937;
}

.card h2 {
    margin-bottom: 10px;
    color: #2563eb;
}

.card p {
    line-height: 1.6;
    color: #4b5563;
}
```

**Visual Explanation:**
```
┌─────────────────────────────────┐ ← Margin (outside space)
│  ┌───────────────────────────┐  │
│  │  ┌─────────────────────┐  │  │ ← Border (visible line)
│  │  │  ┌───────────────┐  │  │  │
│  │  │  │               │  │  │  │ ← Padding (inside space)
│  │  │  │   Content     │  │  │  │
│  │  │  │               │  │  │  │
│  │  │  └───────────────┘  │  │  │
│  │  └─────────────────────┘  │  │
│  └───────────────────────────┘  │
└─────────────────────────────────┘
```

---

### Exercise 3: Padding Shorthand Practice (20 minutes)

**Task:**
Create boxes demonstrating different padding shorthand syntaxes.

**HTML:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Padding Shorthand</title>
    <link rel="stylesheet" href="padding.css">
</head>
<body>
    <h1>Padding Shorthand Examples</h1>
    
    <div class="box one-value">
        <h3>One Value: 20px</h3>
        <p>Same padding on all sides</p>
    </div>
    
    <div class="box two-values">
        <h3>Two Values: 20px 40px</h3>
        <p>Top/bottom: 20px, Left/right: 40px</p>
    </div>
    
    <div class="box three-values">
        <h3>Three Values: 20px 40px 10px</h3>
        <p>Top: 20px, Left/right: 40px, Bottom: 10px</p>
    </div>
    
    <div class="box four-values">
        <h3>Four Values: 10px 20px 30px 40px</h3>
        <p>Top: 10px, Right: 20px, Bottom: 30px, Left: 40px</p>
    </div>
</body>
</html>
```

**CSS (padding.css):**
```css
body {
    font-family: Arial, sans-serif;
    padding: 20px;
    background-color: #f3f4f6;
}

h1 {
    text-align: center;
    color: #1f2937;
}

.box {
    background-color: white;
    border: 2px solid #2563eb;
    margin: 20px 0;
    border-radius: 8px;
}

/* One value: same on all sides */
.one-value {
    padding: 20px;
}

/* Two values: top/bottom, left/right */
.two-values {
    padding: 20px 40px;
}

/* Three values: top, left/right, bottom */
.three-values {
    padding: 20px 40px 10px;
}

/* Four values: top, right, bottom, left (clockwise) */
.four-values {
    padding: 10px 20px 30px 40px;
}

h3 {
    color: #2563eb;
    margin-bottom: 10px;
}

p {
    color: #4b5563;
    margin: 0;
}
```

---

### Exercise 4: Margin Shorthand Practice (20 minutes)

**Task:**
Create boxes demonstrating different margin shorthand syntaxes and auto centering.

**HTML:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Margin Shorthand</title>
    <link rel="stylesheet" href="margin.css">
</head>
<body>
    <h1>Margin Shorthand Examples</h1>
    
    <div class="box one-value">
        <h3>One Value: 20px</h3>
    </div>
    
    <div class="box two-values">
        <h3>Two Values: 20px 40px</h3>
    </div>
    
    <div class="box three-values">
        <h3>Three Values: 20px 40px 10px</h3>
    </div>
    
    <div class="box four-values">
        <h3>Four Values: 10px 20px 30px 40px</h3>
    </div>
    
    <div class="box auto-center">
        <h3>Auto Center: margin: 20px auto</h3>
    </div>
</body>
</html>
```

**CSS (margin.css):**
```css
body {
    font-family: Arial, sans-serif;
    padding: 20px;
    background-color: #f3f4f6;
}

h1 {
    text-align: center;
    color: #1f2937;
}

.box {
    background-color: white;
    border: 2px solid #2563eb;
    padding: 20px;
    border-radius: 8px;
    color: #1f2937;
}

/* One value: same on all sides */
.one-value {
    margin: 20px;
}

/* Two values: top/bottom, left/right */
.two-values {
    margin: 20px 40px;
}

/* Three values: top, left/right, bottom */
.three-values {
    margin: 20px 40px 10px;
}

/* Four values: top, right, bottom, left (clockwise) */
.four-values {
    margin: 10px 20px 30px 40px;
}

/* Auto centering */
.auto-center {
    width: 300px;
    margin: 20px auto; /* top/bottom: 20px, left/right: auto */
}

h3 {
    color: #2563eb;
    margin: 0;
}
```

---

### Exercise 5: Border Shorthand Practice (20 minutes)

**Task:**
Create elements demonstrating different border shorthand syntaxes.

**HTML:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Border Shorthand</title>
    <link rel="stylesheet" href="border.css">
</head>
<body>
    <h1>Border Shorthand Examples</h1>
    
    <div class="box basic">
        <h3>Basic Border</h3>
        <p>border: 2px solid blue</p>
    </div>
    
    <div class="box rounded">
        <h3>Rounded Border</h3>
        <p>border-radius: 10px</p>
    </div>
    
    <div class="box individual">
        <h3>Individual Sides</h3>
        <p>Different borders on each side</p>
    </div>
    
    <div class="box colored">
        <h3>Colored Border</h3>
        <p>border: 3px dashed #e11d48</p>
    </div>
</body>
</html>
```

**CSS (border.css):**
```css
body {
    font-family: Arial, sans-serif;
    padding: 20px;
    background-color: #f3f4f6;
}

h1 {
    text-align: center;
    color: #1f2937;
}

.box {
    background-color: white;
    padding: 20px;
    margin: 20px 0;
    border-radius: 8px;
}

/* Basic border shorthand: width style color */
.basic {
    border: 2px solid #2563eb;
}

/* Rounded corners */
.rounded {
    border: 2px solid #10b981;
    border-radius: 10px;
}

/* Individual sides */
.individual {
    border-top: 3px solid #e11d48;
    border-right: 3px solid #f59e0b;
    border-bottom: 3px solid #10b981;
    border-left: 3px solid #2563eb;
}

/* Different style and color */
.colored {
    border: 3px dashed #e11d48;
    border-radius: 15px;
}

h3 {
    color: #1f2937;
    margin-bottom: 10px;
}

p {
    color: #4b5563;
    margin: 0;
}
```

---

### Exercise 6: Complete Component Practice (20 minutes)

**Task:**
Create a complete component using all Box Model properties.

**HTML:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Complete Component</title>
    <link rel="stylesheet" href="component.css">
</head>
<body>
    <div class="container">
        <div class="product-card">
            <div class="product-image">
                <img src="https://via.placeholder.com/200" alt="Product">
            </div>
            <div class="product-info">
                <h3>Product Name</h3>
                <p class="description">This is a great product with amazing features.</p>
                <div class="price">$29.99</div>
                <button class="buy-button">Add to Cart</button>
            </div>
        </div>
    </div>
</body>
</html>
```

**CSS (component.css):**
```css
/* Reset */
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    font-family: Arial, sans-serif;
    background-color: #f3f4f6;
    padding: 40px;
}

.container {
    max-width: 1200px;
    margin: 0 auto;
}

/* Product Card - Complete Box Model */
.product-card {
    /* Content dimensions */
    width: 300px;
    
    /* Padding */
    padding: 20px;
    
    /* Border */
    border: 1px solid #e5e7eb;
    border-radius: 12px;
    
    /* Margin */
    margin: 20px auto;
    
    /* Background */
    background-color: white;
    
    /* Shadow for depth */
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

.product-image {
    margin-bottom: 15px;
    text-align: center;
}

.product-image img {
    width: 100%;
    border-radius: 8px;
}

.product-info {
    text-align: center;
}

.product-info h3 {
    margin-bottom: 10px;
    color: #1f2937;
    font-size: 18px;
}

.description {
    color: #6b7280;
    font-size: 14px;
    line-height: 1.5;
    margin-bottom: 15px;
}

.price {
    font-size: 24px;
    font-weight: bold;
    color: #2563eb;
    margin-bottom: 15px;
}

.buy-button {
    /* Box Model */
    width: 100%;
    padding: 12px;
    margin-top: 10px;
    
    /* Border */
    border: none;
    border-radius: 6px;
    
    /* Background and text */
    background-color: #2563eb;
    color: white;
    font-size: 16px;
    font-weight: bold;
    cursor: pointer;
}

.buy-button:hover {
    background-color: #1d4ed8;
}
```

---

## Part 3: Review, Questions, Problem Solving (0.5 Hours)

### Common Box Model Mistakes (10 minutes)

**Mistake 1: Forgetting box-sizing**
```css
/* Problem: padding adds to width */
.box {
    width: 300px;
    padding: 20px; /* Total width = 340px */
}

/* Solution: Use border-box */
* {
    box-sizing: border-box;
}
```

**Mistake 2: Margin Collapse**
```css
/* Problem: Vertical margins collapse */
div {
    margin-bottom: 20px;
}
p {
    margin-top: 30px; /* Result: 30px, not 50px */
}
```

**Mistake 3: Confusing padding and margin**
```css
/* Wrong: Using margin for internal spacing */
.button {
    margin: 10px; /* Creates space outside */
}

/* Correct: Using padding for internal spacing */
.button {
    padding: 10px; /* Creates space inside */
}
```

**Mistake 4: Not using shorthand properties**
```css
/* Verbose */
.box {
    padding-top: 10px;
    padding-right: 20px;
    padding-bottom: 10px;
    padding-left: 20px;
}

/* Efficient */
.box {
    padding: 10px 20px;
}
```

**Mistake 5: Incorrect border shorthand order**
```css
/* Wrong order */
.box {
    border: solid 2px blue; /* Won't work correctly */
}

/* Correct order: width style color */
.box {
    border: 2px solid blue;
}
```

---

### Review Questions (10 minutes)

**Question 1:** What does CSS stand for?
**Answer:** Cascading Style Sheets

**Question 2:** What are the three ways to add CSS to HTML?
**Answer:** Inline (style attribute), Internal (style tag), External (linked CSS file)

**Question 3:** Which method is recommended for production and why?
**Answer:** External CSS because it's reusable, maintainable, separates concerns, and allows browser caching.

**Question 4:** What is the correct order for border shorthand?
**Answer:** width, style, color (e.g., `border: 2px solid blue`)

**Question 5:** What does `box-sizing: border-box` do?
**Answer:** It includes padding and border in the element's total width and height.

**Question 6:** What is the difference between padding and margin?
**Answer:** Padding creates space inside the border, margin creates space outside the border.

**Question 7:** How do you center a block element horizontally?
**Answer:** `margin: 0 auto;` with a specified width.

**Question 8:** What selector should you use most often?
**Answer:** Class selectors, because they're reusable and maintainable.

---

### Practice Challenges (5 minutes)

**Challenge 1:** Create a box with different padding on each side using the 4-value shorthand.

**Challenge 2:** Center a div with 300px width using margin auto.

**Challenge 3:** Create a button with 10px padding, green background, white text, and rounded corners.

---

### Homework Assignment

**Task:** Create a styled personal profile card using all Box Model concepts.

**Requirements:**
- External CSS file linked to HTML
- Profile card with:
  - Profile image (use placeholder)
  - Name and title
  - Short bio
  - Contact information
- Use all Box Model properties:
  - Padding (try different shorthand variations)
  - Margin (including auto centering)
  - Border (with border-radius)
- Use class selectors for styling
- Add comments explaining your CSS

**Due Date:** Next session

---

## End of Session 1

**Next Session:** Dimensions and Visual Formatting (Width/Height, Overflow, Text Styling, Display Property, Backgrounds, Advanced Colors, CSS Reset)
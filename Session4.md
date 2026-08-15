# Session 4: Pseudo-classes/Elements + Cascade

## Duration Breakdown
- **1 Hour**: Theoretical Explanation + Live Coding
- **1.5 Hours**: Practical Application (Student writes code)
- **0.5 Hours**: Review, Questions, Problem Solving

---

## Part 1: Theoretical Explanation + Live Coding (1 Hour)

### Section 1: List Styling

## Remove Default Styling
```css
ul {
    list-style: none;
    padding: 0;
    margin: 0;
}
```

## Custom Bullet Points
```css
.custom-list {
    list-style-type: square;
}

.circle-list {
    list-style-type: circle;
}

.decimal-list {
    list-style-type: decimal;
}

.roman-list {
    list-style-type: upper-roman;
}
```

## Custom List Markers
```css
.styled-list {
    list-style: none;
    padding-left: 20px;
}

.styled-list li {
    position: relative;
    padding-left: 25px;
}

.styled-list li::before {
    content: "→";
    position: absolute;
    left: 0;
    color: #2563eb;
    font-weight: bold;
}
```

---

### Section 2: Table Styling

## Basic Table Styling
```css
table {
    width: 100%;
    border-collapse: collapse;
    margin: 20px 0;
}

th, td {
    padding: 12px;
    text-align: left;
    border: 1px solid #ddd;
}

th {
    background-color: #2563eb;
    color: white;
    font-weight: bold;
}

tr:nth-child(even) {
    background-color: #f9f9f9;
}

tr:hover {
    background-color: #f3f4f6;
}
```

## Responsive Table
```css
.responsive-table {
    overflow-x: auto;
}

.responsive-table table {
    min-width: 600px;
}
```

---

### Section 3: Pseudo-classes

## User Action Pseudo-classes

### :hover
```css
.button:hover {
    background-color: #1d4ed8;
    transform: scale(1.05);
}
```

### :active
```css
.button:active {
    transform: scale(0.95);
}
```

### :focus
```css
input:focus {
    outline: 2px solid #2563eb;
    border-color: #2563eb;
}
```

## Structural Pseudo-classes

### :first-child
```css
li:first-child {
    font-weight: bold;
    color: #2563eb;
}
```

### :last-child
```css
li:last-child {
    margin-bottom: 0;
}
```

### :nth-child()
```css
/* Even items */
li:nth-child(even) {
    background-color: #f9f9f9;
}

/* Odd items */
li:nth-child(odd) {
    background-color: #ffffff;
}

/* Every 3rd item */
li:nth-child(3n) {
    font-weight: bold;
}

/* Items 2-5 */
li:nth-child(n+2):nth-child(-n+5) {
    color: #2563eb;
}
```

### :nth-of-type()
```css
p:nth-of-type(2) {
    color: #2563eb;
}
```

## Form Pseudo-classes

### :checked
```css
input:checked + label {
    color: #10b981;
    font-weight: bold;
}
```

### :disabled
```css
input:disabled {
    background-color: #e5e7eb;
    cursor: not-allowed;
}
```

### :required
```css
input:required {
    border: 2px solid #f59e0b;
}
```

## Link Pseudo-classes

### :link
```css
a:link {
    color: #2563eb;
}
```

### :visited
```css
a:visited {
    color: #7c3aed;
}
```

## Negation Pseudo-class

### :not()
```css
p:not(.special) {
    color: #4b5563;
}

input:not([type="checkbox"]) {
    width: 100%;
}
```

---

### Section 4: Pseudo-elements

## ::before
```css
.card::before {
    content: "★";
    color: #f59e0b;
    margin-right: 10px;
}
```

## ::after
```css
.card::after {
    content: "";
    display: block;
    clear: both;
}
```

## ::first-letter
```css
p::first-letter {
    font-size: 3em;
    font-weight: bold;
    color: #2563eb;
    float: left;
    margin-right: 5px;
}
```

## ::first-line
```css
p::first-line {
    font-weight: bold;
    color: #1f2937;
}
```

## ::selection
```css
::selection {
    background-color: #2563eb;
    color: white;
}
```

---

### Section 5: Content Property

## Text Content
```css
.tooltip::before {
    content: "Info: ";
    color: #2563eb;
}
```

## Icon Content
```css
.external-link::after {
    content: " ↗";
}
```

## Attribute Content
```css
[data-info]::before {
    content: attr(data-info) ": ";
    color: #2563eb;
}
```

## Empty Content
```css
.clearfix::after {
    content: "";
    display: table;
    clear: both;
}
```

---

### Section 6: Border Radius

## Basic Border Radius
```css
.rounded {
    border-radius: 10px;
}
```

## Individual Corners
```css
.custom-radius {
    border-top-left-radius: 20px;
    border-top-right-radius: 5px;
    border-bottom-left-radius: 5px;
    border-bottom-right-radius: 20px;
}
```

## Shorthand
```css
.radius-shorthand {
    border-radius: 10px 20px 30px 40px; /* TL, TR, BR, BL */
}
```

## Circle
```css
.circle {
    border-radius: 50%;
    width: 100px;
    height: 100px;
}
```

## Pill Shape
```css
.pill {
    border-radius: 50px;
    padding: 10px 20px;
}
```

---

### Section 7: Box Shadow

## Basic Shadow
```css
.shadow {
    box-shadow: 2px 2px 4px rgba(0, 0, 0, 0.2);
}
```

## Syntax
```css
box-shadow: offset-x offset-y blur-radius spread-radius color;
```

## Multiple Shadows
```css
.multi-shadow {
    box-shadow: 
        0 2px 4px rgba(0, 0, 0, 0.1),
        0 4px 8px rgba(0, 0, 0, 0.1),
        0 8px 16px rgba(0, 0, 0, 0.1);
}
```

## Inner Shadow
```css
.inner-shadow {
    box-shadow: inset 0 2px 4px rgba(0, 0, 0, 0.2);
}
```

## No Shadow
```css
.no-shadow {
    box-shadow: none;
}
```

## Colored Shadow
```css
.colored-shadow {
    box-shadow: 4px 4px 0px #2563eb;
}
```

---

### Section 8: Forms Basic Styling

## Input Styling
```css
input[type="text"],
input[type="email"],
textarea {
    width: 100%;
    padding: 12px;
    border: 2px solid #e5e7eb;
    border-radius: 6px;
    font-size: 16px;
    transition: border-color 0.3s;
}

input:focus,
textarea:focus {
    outline: none;
    border-color: #2563eb;
    box-shadow: 0 0 0 3px rgba(37, 99, 235, 0.1);
}
```

## Button Styling
```css
button {
    padding: 12px 24px;
    background-color: #2563eb;
    color: white;
    border: none;
    border-radius: 6px;
    font-size: 16px;
    font-weight: 500;
    cursor: pointer;
    transition: all 0.3s;
}

button:hover {
    background-color: #1d4ed8;
    transform: translateY(-2px);
    box-shadow: 0 4px 8px rgba(37, 99, 235, 0.3);
}

button:active {
    transform: translateY(0);
}
```

## Form Group
```css
.form-group {
    margin-bottom: 20px;
}

.form-group label {
    display: block;
    margin-bottom: 8px;
    font-weight: 500;
    color: #1f2937;
}
```

---

### Section 9: CSS Specificity & Cascade

## Specificity Hierarchy
1. **Inline styles** (highest specificity)
2. **ID selectors**
3. **Class selectors, pseudo-classes, attribute selectors**
4. **Element selectors, pseudo-elements**
5. **Universal selector** (lowest specificity)

## Specificity Calculation
```css
/* Specificity: 0,0,1 (1 element) */
p {
    color: blue;
}

/* Specificity: 0,1,0 (1 class) */
.text {
    color: red;
}

/* Specificity: 1,0,0 (1 ID) */
#main {
    color: green;
}

/* Specificity: 0,1,1 (1 class + 1 element) */
p.text {
    color: purple;
}

/* Specificity: 1,1,0 (1 ID + 1 class) */
#main.text {
    color: orange;
}
```

## Cascade Rules
1. More specific selector wins
2. Later rules override earlier rules (if same specificity)
3. `!important` overrides everything (use sparingly)

---

### Section 10: !important

## When to Use
- Emergency fixes
- Overriding third-party styles
- Utility classes

## When NOT to Use
- Normal styling
- Can create maintenance issues
- Makes debugging difficult

## Example
```css
.button {
    background-color: #2563eb !important;
}
```

---

### Section 11: Margin Collapse

## What is Margin Collapse
- Vertical margins of adjacent elements can collapse
- The larger margin wins
- Only happens with vertical margins

## Example
```css
.box1 {
    margin-bottom: 20px;
}

.box2 {
    margin-top: 30px; /* Result: 30px, not 50px */
}
```

## Preventing Margin Collapse
```css
.parent {
    overflow: hidden; /* Creates new block formatting context */
}

.parent {
    display: flex; /* Also prevents collapse */
}
```

---

### Section 12: LVHA Order

## Link Visited Hover Active
```css
/* Correct order */
a:link {
    color: blue;
}

a:visited {
    color: purple;
}

a:hover {
    color: red;
}

a:active {
    color: orange;
}
```

## Why Order Matters
- Styles cascade in order
- Later rules override earlier ones
- LVHA ensures proper link styling

---

### Section 13: Float and Clear

## Float (Historical)
```css
.float-left {
    float: left;
    margin-right: 20px;
}

.float-right {
    float: right;
    margin-left: 20px;
}
```

## Clear
```css
.clearfix::after {
    content: "";
    display: table;
    clear: both;
}
```

## Modern Alternative
- Use Flexbox instead of float
- Use Grid instead of float
- Float is mainly for legacy support

---

### Section 14: Vendor Prefixes

## Common Prefixes
```css
.element {
    -webkit-transform: rotate(10deg);
    -moz-transform: rotate(10deg);
    -ms-transform: rotate(10deg);
    -o-transform: rotate(10deg);
    transform: rotate(10deg);
}
```

## Modern Approach
- Most modern browsers don't need prefixes
- Use tools like Autoprefixer
- Focus on standard properties

---

## Part 2: Practical Application (1.5 Hours)

### Exercise 1: List and Table Styling (20 minutes)

**Task:**
Create styled lists and tables.

**HTML:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>List and Table Styling</title>
    <link rel="stylesheet" href="list-table.css">
</head>
<body>
    <h1>List and Table Styling</h1>
    
    <h2>Custom List</h2>
    <ul class="custom-list">
        <li>First item with custom marker</li>
        <li>Second item with custom marker</li>
        <li>Third item with custom marker</li>
    </ul>
    
    <h2>Styled Table</h2>
    <div class="responsive-table">
        <table>
            <thead>
                <tr>
                    <th>Name</th>
                    <th>Email</th>
                    <th>Role</th>
                </tr>
            </thead>
            <tbody>
                <tr>
                    <td>John Doe</td>
                    <td>john@example.com</td>
                    <td>Developer</td>
                </tr>
                <tr>
                    <td>Jane Smith</td>
                    <td>jane@example.com</td>
                    <td>Designer</td>
                </tr>
                <tr>
                    <td>Bob Johnson</td>
                    <td>bob@example.com</td>
                    <td>Manager</td>
                </tr>
            </tbody>
        </table>
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

/* Custom List */
.custom-list {
    list-style: none;
    padding: 0;
    background-color: white;
    border-radius: 8px;
    padding: 20px;
    margin-bottom: 20px;
}

.custom-list li {
    padding: 10px 0;
    padding-left: 30px;
    position: relative;
    border-bottom: 1px solid #e5e7eb;
}

.custom-list li:last-child {
    border-bottom: none;
}

.custom-list li::before {
    content: "✓";
    position: absolute;
    left: 0;
    color: #10b981;
    font-weight: bold;
}

/* Styled Table */
.responsive-table {
    overflow-x: auto;
    margin-bottom: 20px;
}

table {
    width: 100%;
    border-collapse: collapse;
    background-color: white;
    border-radius: 8px;
    overflow: hidden;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

th, td {
    padding: 15px;
    text-align: left;
    border-bottom: 1px solid #e5e7eb;
}

th {
    background-color: #2563eb;
    color: white;
    font-weight: bold;
    text-transform: uppercase;
    font-size: 14px;
}

tr:hover {
    background-color: #f3f4f6;
}

tr:last-child td {
    border-bottom: none;
}
```

---

### Exercise 2: Pseudo-classes Practice (25 minutes)

**Task:**
Create interactive elements using various pseudo-classes.

**HTML:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Pseudo-classes Practice</title>
    <link rel="stylesheet" href="pseudo-classes.css">
</head>
<body>
    <h1>Pseudo-classes Examples</h1>
    
    <h2>User Action Pseudo-classes</h2>
    <button class="interactive-button">Hover and Click Me</button>
    <input type="text" class="styled-input" placeholder="Focus me">
    
    <h2>Structural Pseudo-classes</h2>
    <ul class="numbered-list">
        <li>First item (first-child)</li>
        <li>Second item</li>
        <li>Third item</li>
        <li>Fourth item</li>
        <li>Fifth item (last-child)</li>
    </ul>
    
    <h2>Form Pseudo-classes</h2>
    <div class="form-example">
        <label>
            <input type="checkbox" class="custom-checkbox">
            <span>Check me (styled when checked)</span>
        </label>
        <input type="text" class="required-input" placeholder="Required field" required>
        <input type="text" class="disabled-input" placeholder="Disabled field" disabled>
    </div>
    
    <h2>Link Pseudo-classes</h2>
    <a href="#" class="styled-link">Hover me</a>
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

/* User Action Pseudo-classes */
.interactive-button {
    padding: 15px 30px;
    background-color: #2563eb;
    color: white;
    border: none;
    border-radius: 8px;
    font-size: 16px;
    cursor: pointer;
    transition: all 0.3s;
    margin-bottom: 10px;
}

.interactive-button:hover {
    background-color: #1d4ed8;
    transform: scale(1.05);
    box-shadow: 0 4px 8px rgba(37, 99, 235, 0.3);
}

.interactive-button:active {
    transform: scale(0.95);
}

.styled-input {
    padding: 12px;
    border: 2px solid #e5e7eb;
    border-radius: 6px;
    font-size: 16px;
    width: 100%;
    max-width: 300px;
    transition: all 0.3s;
}

.styled-input:focus {
    outline: none;
    border-color: #2563eb;
    box-shadow: 0 0 0 3px rgba(37, 99, 235, 0.1);
}

/* Structural Pseudo-classes */
.numbered-list {
    list-style: none;
    padding: 0;
    background-color: white;
    border-radius: 8px;
    padding: 20px;
    margin-bottom: 20px;
}

.numbered-list li {
    padding: 10px;
    border-bottom: 1px solid #e5e7eb;
}

.numbered-list li:first-child {
    font-weight: bold;
    color: #2563eb;
    background-color: #f3f4f6;
}

.numbered-list li:last-child {
    border-bottom: none;
    font-weight: bold;
    color: #10b981;
}

.numbered-list li:nth-child(even) {
    background-color: #f9fafb;
}

/* Form Pseudo-classes */
.form-example {
    background-color: white;
    padding: 20px;
    border-radius: 8px;
    margin-bottom: 20px;
}

.form-example label {
    display: block;
    margin-bottom: 10px;
}

.custom-checkbox {
    margin-right: 10px;
}

.custom-checkbox:checked + span {
    color: #10b981;
    font-weight: bold;
}

.required-input {
    padding: 10px;
    border: 2px solid #f59e0b;
    border-radius: 4px;
    width: 100%;
    max-width: 300px;
}

.disabled-input {
    padding: 10px;
    background-color: #e5e7eb;
    border: 1px solid #d1d5db;
    border-radius: 4px;
    width: 100%;
    max-width: 300px;
    cursor: not-allowed;
}

/* Link Pseudo-classes */
.styled-link {
    color: #2563eb;
    text-decoration: none;
    font-weight: 500;
    transition: color 0.3s;
}

.styled-link:hover {
    color: #1d4ed8;
    text-decoration: underline;
}
```

---

### Exercise 3: Pseudo-elements Practice (20 minutes)

**Task:**
Create elements using pseudo-elements.

**HTML:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Pseudo-elements Practice</title>
    <link rel="stylesheet" href="pseudo-elements.css">
</head>
<body>
    <h1>Pseudo-elements Examples</h1>
    
    <h2>::before and ::after</h2>
    <div class="card">
        <h3>Card Title</h3>
        <p>Card content with decorative pseudo-elements.</p>
    </div>
    
    <h2>::first-letter</h2>
    <p class="drop-cap">This paragraph has a decorative first letter using the ::first-letter pseudo-element.</p>
    
    <h2>::first-line</h2>
    <p class="first-line-highlight">This is the first line of the paragraph that will be styled differently. The rest of the paragraph will have normal styling.</p>
    
    <h2>::selection</h2>
    <p class="custom-selection">Try selecting this text to see the custom selection styling.</p>
    
    <h2>External Links</h2>
    <a href="#" class="external-link">External Link</a>
    <a href="#" class="internal-link">Internal Link</a>
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

/* ::before and ::after */
.card {
    background-color: white;
    padding: 30px;
    border-radius: 8px;
    margin-bottom: 20px;
    position: relative;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.card::before {
    content: "★";
    position: absolute;
    top: 10px;
    right: 10px;
    color: #f59e0b;
    font-size: 24px;
}

.card::after {
    content: "";
    display: block;
    clear: both;
}

.card h3 {
    color: #2563eb;
    margin-bottom: 10px;
}

.card p {
    color: #4b5563;
    line-height: 1.6;
}

/* ::first-letter */
.drop-cap {
    background-color: white;
    padding: 20px;
    border-radius: 8px;
    margin-bottom: 20px;
    font-size: 18px;
    line-height: 1.6;
}

.drop-cap::first-letter {
    font-size: 3em;
    font-weight: bold;
    color: #2563eb;
    float: left;
    margin-right: 10px;
    line-height: 1;
}

/* ::first-line */
.first-line-highlight {
    background-color: white;
    padding: 20px;
    border-radius: 8px;
    margin-bottom: 20px;
    font-size: 16px;
    line-height: 1.8;
}

.first-line-highlight::first-line {
    font-weight: bold;
    color: #2563eb;
    text-transform: uppercase;
}

/* ::selection */
.custom-selection {
    background-color: white;
    padding: 20px;
    border-radius: 8px;
    margin-bottom: 20px;
    font-size: 16px;
}

::selection {
    background-color: #2563eb;
    color: white;
}

/* External Links */
.external-link {
    color: #2563eb;
    text-decoration: none;
    margin-right: 20px;
}

.external-link::after {
    content: " ↗";
    color: #2563eb;
}

.internal-link {
    color: #10b981;
    text-decoration: none;
}
```

---

### Exercise 4: Border Radius and Box Shadow (20 minutes)

**Task:**
Create cards with various border radius and box shadow effects.

**HTML:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Border Radius and Box Shadow</title>
    <link rel="stylesheet" href="radius-shadow.css">
</head>
<body>
    <h1>Border Radius and Box Shadow</h1>
    
    <h2>Border Radius Examples</h2>
    <div class="card-container">
        <div class="card basic-radius">Basic Radius</div>
        <div class="card circle">Circle</div>
        <div class="card pill">Pill Shape</div>
        <div class="card custom-radius">Custom Radius</div>
    </div>
    
    <h2>Box Shadow Examples</h2>
    <div class="card-container">
        <div class="card basic-shadow">Basic Shadow</div>
        <div class="card multi-shadow">Multi Shadow</div>
        <div class="card inner-shadow">Inner Shadow</div>
        <div class="card colored-shadow">Colored Shadow</div>
    </div>
    
    <h2>Combined Effects</h2>
    <div class="card-container">
        <div class="card premium-card">Premium Card</div>
        <div class="card floating-card">Floating Card</div>
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

.card-container {
    display: flex;
    flex-wrap: wrap;
    gap: 20px;
    margin-bottom: 20px;
}

.card {
    background-color: white;
    padding: 30px;
    min-width: 150px;
    min-height: 100px;
    display: flex;
    align-items: center;
    justify-content: center;
    color: #1f2937;
    font-weight: bold;
    font-size: 16px;
}

/* Border Radius Examples */
.basic-radius {
    border-radius: 10px;
}

.circle {
    border-radius: 50%;
    width: 100px;
    height: 100px;
}

.pill {
    border-radius: 50px;
}

.custom-radius {
    border-radius: 20px 5px 20px 5px;
}

/* Box Shadow Examples */
.basic-shadow {
    box-shadow: 2px 2px 4px rgba(0, 0, 0, 0.2);
}

.multi-shadow {
    box-shadow: 
        0 2px 4px rgba(0, 0, 0, 0.1),
        0 4px 8px rgba(0, 0, 0, 0.1),
        0 8px 16px rgba(0, 0, 0, 0.1);
}

.inner-shadow {
    box-shadow: inset 0 2px 4px rgba(0, 0, 0, 0.2);
}

.colored-shadow {
    box-shadow: 4px 4px 0px #2563eb;
}

/* Combined Effects */
.premium-card {
    border-radius: 16px;
    box-shadow: 
        0 4px 6px rgba(0, 0, 0, 0.1),
        0 10px 20px rgba(0, 0, 0, 0.15);
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    color: white;
}

.floating-card {
    border-radius: 12px;
    box-shadow: 0 8px 16px rgba(37, 99, 235, 0.3);
    transition: transform 0.3s, box-shadow 0.3s;
}

.floating-card:hover {
    transform: translateY(-5px);
    box-shadow: 0 12px 24px rgba(37, 99, 235, 0.4);
}
```

---

### Exercise 5: Forms Styling (20 minutes)

**Task:**
Create a fully styled form.

**HTML:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Forms Styling</title>
    <link rel="stylesheet" href="forms.css">
</head>
<body>
    <h1>Styled Form</h1>
    
    <form class="styled-form">
        <div class="form-group">
            <label for="name">Full Name</label>
            <input type="text" id="name" name="name" placeholder="Enter your name" required>
        </div>
        
        <div class="form-group">
            <label for="email">Email Address</label>
            <input type="email" id="email" name="email" placeholder="Enter your email" required>
        </div>
        
        <div class="form-group">
            <label for="message">Message</label>
            <textarea id="message" name="message" rows="4" placeholder="Your message"></textarea>
        </div>
        
        <div class="form-group">
            <label>
                <input type="checkbox" id="subscribe" name="subscribe">
                <span>Subscribe to newsletter</span>
            </label>
        </div>
        
        <button type="submit" class="submit-button">Send Message</button>
    </form>
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

.styled-form {
    max-width: 500px;
    margin: 0 auto;
    background-color: white;
    padding: 40px;
    border-radius: 12px;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

.form-group {
    margin-bottom: 20px;
}

.form-group label {
    display: block;
    margin-bottom: 8px;
    font-weight: 500;
    color: #1f2937;
}

input[type="text"],
input[type="email"],
textarea {
    width: 100%;
    padding: 12px;
    border: 2px solid #e5e7eb;
    border-radius: 6px;
    font-size: 16px;
    font-family: Arial, sans-serif;
    transition: all 0.3s;
}

input:focus,
textarea:focus {
    outline: none;
    border-color: #2563eb;
    box-shadow: 0 0 0 3px rgba(37, 99, 235, 0.1);
}

input::placeholder,
textarea::placeholder {
    color: #9ca3af;
}

textarea {
    resize: vertical;
}

.form-group label input[type="checkbox"] {
    width: auto;
    margin-right: 10px;
}

.submit-button {
    width: 100%;
    padding: 14px;
    background-color: #2563eb;
    color: white;
    border: none;
    border-radius: 6px;
    font-size: 16px;
    font-weight: 600;
    cursor: pointer;
    transition: all 0.3s;
}

.submit-button:hover {
    background-color: #1d4ed8;
    transform: translateY(-2px);
    box-shadow: 0 4px 8px rgba(37, 99, 235, 0.3);
}

.submit-button:active {
    transform: translateY(0);
}
```

---

## Part 3: Review, Questions, Problem Solving (0.5 Hours)

### Specificity & Cascade (10 minutes)

**Specificity Calculation Examples:**
```css
/* 0,0,1 - Element selector */
p { color: blue; }

/* 0,1,0 - Class selector */
.text { color: red; }

/* 1,0,0 - ID selector */
#main { color: green; }

/* 0,1,1 - Class + Element */
p.text { color: purple; }

/* 1,1,0 - ID + Class */
#main.text { color: orange; }

/* 1,1,1 - ID + Class + Element */
#main p.text { color: pink; }
```

**!important Rules:**
- Overrides specificity
- Use only when absolutely necessary
- Makes debugging difficult
- Creates maintenance issues

---

### Review Questions (10 minutes)

**Question 1:** What is the correct order for link pseudo-classes?
**Answer:** LVHA - Link, Visited, Hover, Active

**Question 2:** What does specificity determine in CSS?
**Answer:** Which CSS rule takes precedence when multiple rules target the same element.

**Question 3:** When should you use `!important`?
**Answer:** Only in emergencies, to override third-party styles, or for utility classes.

**Question 4:** What is margin collapse?
**Answer:** When vertical margins of adjacent elements combine into a single margin.

**Question 5:** What is the difference between `::before` and `:before`?
**Answer:** `::before` is the modern pseudo-element syntax, `:before` is the older syntax (both work).

**Question 6:** What does `:nth-child(even)` select?
**Answer:** All even-numbered children of their parent.

**Question 7:** How do you prevent margin collapse?
**Answer:** Use `overflow: hidden`, `display: flex`, or other block formatting contexts.

**Question 8:** What is the specificity of an ID selector?
**Answer:** 1,0,0 (100 points in the specificity calculation).

---

### Practice Challenges (5 minutes)

**Challenge 1:** Create a button that changes color on hover and has a scale effect on active.

**Challenge 2:** Style a table with alternating row colors and hover effects.

**Challenge 3:** Create a card with a decorative icon using `::before`.

---

### Homework Assignment

**Task:** Create a complete interactive component using pseudo-classes and pseudo-elements.

**Requirements:**
- Interactive buttons with hover/active/focus states
- Styled form with custom checkboxes
- Cards with decorative pseudo-elements
- Styled table with hover effects
- Use specificity correctly
- Avoid `!important` unless necessary
- Include proper LVHA order for links

**Due Date:** Next session

---

## End of Session 4

**Next Session:** CSS Variables + Flexbox (CSS Variables creation and usage, Flexbox parent and child properties, Navbar and Cards implementation)
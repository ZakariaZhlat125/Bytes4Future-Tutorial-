# Session 4: Pseudo-classes/Elements + Cascade

## Duration Breakdown (2.5 Hours Total)
- **1.5 Hours**: Interactive Learning (All Sections 1-14 with Explain → Example → Try it yourself → Predict → Experiment → Challenge → Bug Hunting → Quiz)
- **20 Minutes**: Comprehensive Project (Single component combining all concepts)
- **10 Minutes**: Review & Assessment

---

## Part 1: Interactive Learning (1.5 Hours)

### Section 1: List Styling

## Explain
CSS provides various ways to style lists:

- `list-style-type` - Changes bullet/number style
- `list-style: none` - Removes default styling
- `::before` pseudo-element - Custom markers
- Custom styling with positioning

## Example
```css
/* Remove default styling */
ul {
    list-style: none;
    padding: 0;
    margin: 0;
}

/* Custom bullet points */
.custom-list {
    list-style-type: square;
}

/* Custom markers with pseudo-element */
.styled-list li::before {
    content: "→";
    color: #2563eb;
    font-weight: bold;
}
```

## Try it yourself
Create a custom styled list.

```html
<ul class="my-list">
    <li>First item</li>
    <li>Second item</li>
    <li>Third item</li>
</ul>
```

```css
.my-list {
    /* Add custom styling */
}
```

## Predict
What will happen with this code?

```css
ul {
    list-style: none;
}
```

## Experiment
1. What happens when you remove `list-style: none`?
2. Try different `list-style-type` values
3. Can you use custom icons as markers?

## Challenge
**Task:** Create a custom list with:
- Checkmark icons as markers
- Hover effects on list items
- Different colors for even/odd items
- Time: 10 minutes
- Hint: Use `::before` pseudo-element for custom markers

<details>
<summary>Solution</summary>

```html
<ul class="check-list">
    <li>First item</li>
    <li>Second item</li>
    <li>Third item</li>
</ul>
```

```css
.check-list {
    list-style: none;
    padding: 0;
}

.check-list li {
    padding: 10px 0;
    padding-left: 30px;
    position: relative;
    transition: background 0.3s;
}

.check-list li::before {
    content: "✓";
    position: absolute;
    left: 0;
    color: #10b981;
    font-weight: bold;
}

.check-list li:nth-child(even) {
    background: #f9f9f9;
}

.check-list li:hover {
    background: #e5e7eb;
}
```

</details>

## Bug Hunting
Find the bug in this code:

```css
.list {
    list-style: none;
    padding: 0;
}

.list li {
    /* Custom markers not showing */
}
```

<details>
<summary>Solution</summary>

Missing the `::before` pseudo-element to create custom markers. Need to add content and positioning.

</details>

## Quiz
1. What does `list-style: none` do?
2. Can you use images as list markers?
3. What pseudo-element is commonly used for custom markers?

<details>
<summary>Quiz Answers</summary>

1. Removes the default bullet points or numbers from lists
2. Yes, you can use `list-style-image: url('image.png')` or use pseudo-elements
3. The `::before` pseudo-element is commonly used for custom markers

</details>

---

### Section 2: Table Styling

## Explain
Table styling improves data presentation and readability:

- `border-collapse: collapse` - Removes double borders
- `th` styling - Header cells with background colors
- `tr:nth-child()` - Alternating row colors
- `tr:hover` - Row highlighting on hover
- Responsive tables with overflow

## Example
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

## Try it yourself
Create a styled table.

```html
<table class="my-table">
    <thead>
        <tr>
            <th>Name</th>
            <th>Email</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>John</td>
            <td>john@example.com</td>
        </tr>
    </tbody>
</table>
```

```css
.my-table {
    /* Add table styling */
}
```

## Predict
What will happen with this code?

```css
table {
    border-collapse: separate;
}
```

## Experiment
1. What happens without `border-collapse: collapse`?
2. Try different `nth-child` patterns
3. Can you style specific columns?

## Challenge
**Task:** Create a responsive table with:
- Alternating row colors
- Hover effects on rows
- Header with gradient background
- Responsive scrolling on mobile
- Time: 12 minutes
- Hint: Use `overflow-x: auto` for responsiveness

<details>
<summary>Solution</summary>

```html
<div class="table-container">
    <table class="styled-table">
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
                <td>Admin</td>
            </tr>
            <tr>
                <td>Jane Smith</td>
                <td>jane@example.com</td>
                <td>User</td>
            </tr>
        </tbody>
    </table>
</div>
```

```css
.table-container {
    overflow-x: auto;
}

.styled-table {
    width: 100%;
    border-collapse: collapse;
    background: white;
    box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

.styled-table th {
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    color: white;
    padding: 15px;
    text-align: left;
}

.styled-table td {
    padding: 15px;
    border-bottom: 1px solid #e5e7eb;
}

.styled-table tr:nth-child(even) {
    background: #f9f9f9;
}

.styled-table tr:hover {
    background: #f3f4f6;
}
```

</details>

## Bug Hunting
Find the bug in this code:

```css
.buggy-table {
    width: 100%;
    /* Borders showing double lines */
}
```

<details>
<summary>Solution</summary>

Missing `border-collapse: collapse` to remove double borders between cells.

</details>

## Quiz
1. What does `border-collapse: collapse` do?
2. Which pseudo-class targets even rows?
3. How do you make tables responsive?

<details>
<summary>Quiz Answers</summary>

1. Merges adjacent cell borders into single borders
2. `tr:nth-child(even)` targets even-numbered rows
3. Wrap table in a container with `overflow-x: auto` for horizontal scrolling

</details>

---

### Section 3: Pseudo-classes

## Explain
Pseudo-classes target elements based on state or position:

- **User Action**: `:hover`, `:active`, `:focus`
- **Structural**: `:first-child`, `:last-child`, `:nth-child()`
- **Form**: `:checked`, `:disabled`, `:required`
- **Link**: `:link`, `:visited`
- **Negation**: `:not()`

## Example
```css
/* User Action */
.button:hover {
    background-color: #1d4ed8;
    transform: scale(1.05);
}

/* Structural */
li:first-child {
    font-weight: bold;
    color: #2563eb;
}

/* Form */
input:checked + label {
    color: #10b981;
    font-weight: bold;
}

/* Negation */
p:not(.special) {
    color: #4b5563;
}
```

## Try it yourself
Create interactive elements with pseudo-classes.

```html
<button class="my-button">Hover me</button>
<ul class="my-list">
    <li>First</li>
    <li>Second</li>
    <li>Last</li>
</ul>
```

```css
.my-button {
    /* Add hover and active states */
}

.my-list li:first-child {
    /* Style first item */
}
```

## Predict
What will happen with this code?

```css
li:nth-child(2n) {
    background: blue;
}
```

## Experiment
1. What's the difference between `:hover` and `:active`?
2. Try different `:nth-child()` patterns
3. How does `:not()` work with complex selectors?

## Challenge
**Task:** Create an interactive list with:
- First item bold (first-child)
- Last item different color (last-child)
- Even items with background (nth-child)
- Hover effect on all items
- Time: 12 minutes
- Hint: Combine multiple pseudo-classes

<details>
<summary>Solution</summary>

```html
<ul class="interactive-list">
    <li>First item</li>
    <li>Second item</li>
    <li>Third item</li>
    <li>Fourth item</li>
    <li>Last item</li>
</ul>
```

```css
.interactive-list {
    list-style: none;
    padding: 0;
}

.interactive-list li {
    padding: 10px;
    margin: 5px 0;
    background: white;
    transition: all 0.3s;
}

.interactive-list li:first-child {
    font-weight: bold;
    color: #2563eb;
}

.interactive-list li:last-child {
    color: #dc2626;
}

.interactive-list li:nth-child(even) {
    background: #f3f4f6;
}

.interactive-list li:hover {
    background: #dbeafe;
    transform: translateX(5px);
}
```

</details>

## Bug Hunting
Find the bug in this code:

```css
input:checked {
    color: green;
}
/* Label not changing color */
```

<details>
<summary>Solution</summary>

Need to use the adjacent sibling selector `input:checked + label` to style the label when checkbox is checked.

</details>

## Quiz
1. What does `:first-child` select?
2. How does `:nth-child(even)` work?
3. What's the purpose of `:not()`?

<details>
<summary>Quiz Answers</summary>

1. Selects the first child element of its parent
2. Selects all even-numbered children (2, 4, 6, etc.)
3. `:not()` selects elements that don't match the given selector

</details>

---

### Section 4: Pseudo-elements

## Explain
Pseudo-elements allow you to style specific parts of elements:

- `::before` - Insert content before element content
- `::after` - Insert content after element content
- `::first-letter` - Style the first letter
- `::first-line` - Style the first line
- `::selection` - Style selected text

## Example
```css
.card::before {
    content: "★";
    color: #f59e0b;
    margin-right: 10px;
}

.card::after {
    content: "";
    display: block;
    clear: both;
}

p::first-letter {
    font-size: 3em;
    font-weight: bold;
    color: #2563eb;
    float: left;
}

::selection {
    background-color: #2563eb;
    color: white;
}
```

## Try it yourself
Create elements with pseudo-elements.

```html
<div class="my-card">Card content</div>
<p class="my-text">Paragraph with first letter styling</p>
```

```css
.my-card::before {
    /* Add decorative content */
}

.my-text::first-letter {
    /* Style first letter */
}
```

## Predict
What will happen with this code?

```css
.empty::before {
    content: "";
}
```

## Experiment
1. What happens if you don't add `content` property?
2. Can you use pseudo-elements on inline elements?
3. Try combining multiple pseudo-elements

## Challenge
**Task:** Create a decorative card with:
- Star icon using ::before
- Divider line using ::after
- Drop cap effect on first letter
- Custom selection styling
- Time: 12 minutes
- Hint: Use positioning for decorative elements

<details>
<summary>Solution</summary>

```html
<div class="decorative-card">
    <h3>Featured Card</h3>
    <p class="drop-cap">This is a paragraph with a decorative first letter. The rest of the paragraph continues normally with regular styling.</p>
</div>
```

```css
.decorative-card {
    background: white;
    padding: 30px;
    border-radius: 8px;
    position: relative;
    box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

.decorative-card::before {
    content: "★";
    position: absolute;
    top: 15px;
    right: 15px;
    color: #f59e0b;
    font-size: 24px;
}

.decorative-card::after {
    content: "";
    display: block;
    height: 2px;
    background: linear-gradient(90deg, #667eea, #764ba2);
    margin: 20px 0;
}

.drop-cap::first-letter {
    font-size: 3em;
    font-weight: bold;
    color: #667eea;
    float: left;
    margin-right: 10px;
    line-height: 1;
}

::selection {
    background: #667eea;
    color: white;
}
```

</details>

## Bug Hunting
Find the bug in this code:

```css
.buggy::before {
    color: red;
    /* Content not showing */
}
```

<details>
<summary>Solution</summary>

Missing the `content` property. Pseudo-elements require `content` to display anything.

</details>

## Quiz
1. What is the difference between pseudo-classes and pseudo-elements?
2. Which pseudo-element styles the first letter?
3. Can pseudo-elements be selected with JavaScript?

<details>
<summary>Quiz Answers</summary>

1. Pseudo-classes (:hover) target element states, pseudo-elements (::before) create virtual elements
2. `::first-letter` styles the first letter of text content
3. No, pseudo-elements are not part of the DOM and cannot be directly selected with JavaScript

</details>

---

### Section 5: Content Property

## Explain
The `content` property is used with pseudo-elements to insert content:

- Text content - Strings or characters
- Icons - Unicode characters or symbols
- Attribute values - Using `attr()`
- Empty content - For clearing or spacing
- Only works with `::before` and `::after`

## Example
```css
.tooltip::before {
    content: "Info: ";
    color: #2563eb;
}

.external-link::after {
    content: " ↗";
}

[data-info]::before {
    content: attr(data-info) ": ";
    color: #2563eb;
}

.clearfix::after {
    content: "";
    display: table;
    clear: both;
}
```

## Try it yourself
Add content using pseudo-elements.

```html
<div class="my-tooltip" data-info="Help">Hover for info</div>
<a href="#" class="my-link">External link</a>
```

```css
.my-tooltip::before {
    /* Add tooltip content */
}

.my-link::after {
    /* Add external link icon */
}
```

## Predict
What will happen with this code?

```css
.missing::before {
    color: red;
}
```

## Experiment
1. What happens if content is empty string?
2. Can you use HTML in content property?
3. Try using `attr()` with different attributes

## Challenge
**Task:** Create a component with:
- Tooltip using `::before` with text
- External link indicator using `::after`
- Data attribute content using `attr()`
- Empty content for layout
- Time: 10 minutes
- Hint: Use `attr()` to pull values from HTML attributes

<details>
<summary>Solution</summary>

```html
<div class="info-card" data-category="Featured">
    <h3>Card Title</h3>
    <p>Card content</p>
    <a href="#" class="external-link">Learn more</a>
</div>
```

```css
.info-card::before {
    content: attr(data-category) ": ";
    color: #f59e0b;
    font-weight: bold;
    display: block;
    margin-bottom: 10px;
}

.external-link::after {
    content: " ↗";
    color: #2563eb;
}

.clearfix::after {
    content: "";
    display: block;
    clear: both;
}
```

</details>

## Bug Hunting
Find the bug in this code:

```css
.buggy::before {
    content: <span>HTML</span>;
}
```

<details>
<summary>Solution</summary>

You cannot use HTML tags in the content property. It only accepts text, URLs, or attribute values.

</details>

## Quiz
1. What does `attr()` do in content property?
2. Can you use images in content property?
3. Why is empty content useful?

<details>
<summary>Quiz Answers</summary>

1. `attr()` pulls the value of an HTML attribute and uses it as content
2. Yes, you can use `content: url('image.png')` for images
3. Empty content is useful for clearfix, clearing floats, or creating pseudo-elements for layout purposes

</details>

---

### Section 6: Border Radius

## Explain
The `border-radius` property controls the rounded corners of elements:

- Single value - All corners equal
- Multiple values - Different corners
- Can create circles (50%) and pills
- Works on any element with borders

## Example
```css
.rounded {
    border-radius: 10px;
}

.circle {
    border-radius: 50%;
    width: 100px;
    height: 100px;
}

.pill {
    border-radius: 50px;
    padding: 10px 20px;
}
```

## Try it yourself
Create elements with different border radius.

```html
<div class="rounded-box">Rounded</div>
<div class="circle-box">Circle</div>
<div class="pill-box">Pill</div>
```

```css
.rounded-box {
    border-radius: 10px;
    /* Add basic styling */
}

.circle-box {
    /* Make this a circle */
}

.pill-box {
    /* Make this pill-shaped */
}
```

## Predict
What will happen with this code?

```css
.oval {
    border-radius: 50%;
    width: 200px;
    height: 100px;
}
```

## Experiment
1. What happens with different corner values?
2. Can you create asymmetric shapes?
3. Does border-radius work on images?

## Challenge
**Task:** Create a card with:
- Top corners rounded, bottom corners sharp
- Different radius for each corner
- Responsive radius changes
- Time: 10 minutes
- Hint: Use shorthand with 4 values: TL TR BR BL

<details>
<summary>Solution</summary>

```html
<div class="custom-card">
    <h3>Custom Radius Card</h3>
    <p>This card has custom rounded corners</p>
</div>
```

```css
.custom-card {
    background: white;
    padding: 20px;
    border-radius: 20px 20px 0 0; /* TL, TR, BR, BL */
    box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

@media (max-width: 480px) {
    .custom-card {
        border-radius: 10px; /* Simpler on mobile */
    }
}
```

</details>

## Bug Hunting
Find the bug in this code:

```css
.buggy {
    border-radius: 50%;
    width: 200px;
    /* Not a perfect circle */
}
```

<details>
<summary>Solution</summary>

Missing equal height. Perfect circles need equal width and height with `border-radius: 50%`.

</details>

## Quiz
1. What creates a perfect circle?
2. How do you set different radius for each corner?
3. Can border-radius exceed 50%?

<details>
<summary>Quiz Answers</summary>

1. `border-radius: 50%` with equal width and height creates a perfect circle
2. Use 4 values: `border-radius: TL TR BR BL` (top-left, top-right, bottom-right, bottom-left)
3. Yes, values above 50% are valid but won't change the shape beyond a circle

</details>

---

### Section 7: Box Shadow

## Explain
The `box-shadow` property adds shadow effects around elements:

- Creates depth and dimension
- Can add multiple shadows
- Supports inner shadows with `inset`
- Syntax: `offset-x offset-y blur-radius spread-radius color`

## Example
```css
.shadow {
    box-shadow: 2px 2px 4px rgba(0, 0, 0, 0.2);
}

.multi-shadow {
    box-shadow: 
        0 2px 4px rgba(0, 0, 0, 0.1),
        0 4px 8px rgba(0, 0, 0, 0.1);
}

.inner-shadow {
    box-shadow: inset 0 2px 4px rgba(0, 0, 0, 0.2);
}
```

## Try it yourself
Create elements with different shadow effects.

```html
<div class="shadow-box">Basic Shadow</div>
<div class="multi-shadow-box">Multi Shadow</div>
<div class="inner-shadow-box">Inner Shadow</div>
```

```css
.shadow-box {
    /* Add basic shadow */
}

.multi-shadow-box {
    /* Add multiple shadows */
}

.inner-shadow-box {
    /* Add inner shadow */
}
```

## Predict
What will happen with this code?

```css
.mystery {
    box-shadow: 0 0 0 10px rgba(0, 0, 0, 0.5);
}
```

## Experiment
1. What happens when you increase the blur radius?
2. Try negative offset values
3. What's the difference between spread and blur?

## Challenge
**Task:** Create a card with:
- Multi-layered shadow for depth
- Colored shadow effect
- Hover effect that enhances shadow
- Time: 10 minutes
- Hint: Use multiple shadows with different blur values

<details>
<summary>Solution</summary>

```html
<div class="shadow-card">
    <h3>Shadow Card</h3>
    <p>Hover to see enhanced shadow effect</p>
</div>
```

```css
.shadow-card {
    background: white;
    padding: 20px;
    border-radius: 8px;
    box-shadow: 
        0 2px 4px rgba(0, 0, 0, 0.1),
        0 8px 16px rgba(0, 0, 0, 0.1);
    transition: box-shadow 0.3s;
}

.shadow-card:hover {
    box-shadow: 
        0 4px 8px rgba(0, 0, 0, 0.15),
        0 12px 24px rgba(0, 0, 0, 0.15),
        0 0 0 4px rgba(37, 99, 235, 0.2);
}
```

</details>

## Bug Hunting
Find the bug in this code:

```css
.buggy-shadow {
    box-shadow: 2px 2px 4px;
    /* The shadow isn't visible */
}
```

<details>
<summary>Solution</summary>

Missing the color value. Box shadow requires a color parameter.

</details>

## Quiz
1. What does the `inset` keyword do in box-shadow?
2. Can you add multiple shadows to one element?
3. What is the order of values in box-shadow syntax?

<details>
<summary>Quiz Answers</summary>

1. `inset` creates an inner shadow instead of the default outer shadow
2. Yes, you can add multiple shadows by separating them with commas
3. offset-x, offset-y, blur-radius, spread-radius, color

</details>

---

### Section 8: Forms Basic Styling

## Explain
Form styling makes input elements user-friendly and visually consistent:

- Inputs need proper padding and borders
- Focus states improve accessibility
- Buttons need hover and active states
- Form groups organize related elements

## Example
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

button {
    padding: 12px 24px;
    background-color: #2563eb;
    color: white;
    border: none;
    border-radius: 6px;
    cursor: pointer;
    transition: all 0.3s;
}

button:hover {
    background-color: #1d4ed8;
    transform: translateY(-2px);
}
```

## Try it yourself
Create a styled form input.

```html
<div class="form-group">
    <label for="email">Email</label>
    <input type="email" id="email" placeholder="Enter your email">
</div>
```

```css
.form-group {
    margin-bottom: 20px;
}

.form-group label {
    display: block;
    margin-bottom: 8px;
    font-weight: 500;
}

/* Style the input */
```

## Predict
What will happen with this code?

```css
input:focus {
    border-color: red;
    outline: none;
}
```

## Experiment
1. What happens when you remove `outline: none` from focus state?
2. Try different border widths on focus
3. How does padding affect the input appearance?

## Challenge
**Task:** Create a complete form with:
- Styled text and email inputs
- Textarea with proper sizing
- Submit button with hover/active states
- Focus states for all inputs
- Time: 12 minutes
- Hint: Use form-group class for organization

<details>
<summary>Solution</summary>

```html
<form class="styled-form">
    <div class="form-group">
        <label for="name">Name</label>
        <input type="text" id="name" placeholder="Your name">
    </div>
    <div class="form-group">
        <label for="email">Email</label>
        <input type="email" id="email" placeholder="your@email.com">
    </div>
    <div class="form-group">
        <label for="message">Message</label>
        <textarea id="message" rows="4" placeholder="Your message"></textarea>
    </div>
    <button type="submit">Send</button>
</form>
```

```css
.styled-form {
    max-width: 400px;
    margin: 0 auto;
}

.form-group {
    margin-bottom: 20px;
}

.form-group label {
    display: block;
    margin-bottom: 8px;
    font-weight: 500;
}

input, textarea {
    width: 100%;
    padding: 12px;
    border: 2px solid #e5e7eb;
    border-radius: 6px;
    font-size: 16px;
    transition: all 0.3s;
}

input:focus, textarea:focus {
    outline: none;
    border-color: #2563eb;
    box-shadow: 0 0 0 3px rgba(37, 99, 235, 0.1);
}

button {
    width: 100%;
    padding: 12px;
    background: #2563eb;
    color: white;
    border: none;
    border-radius: 6px;
    cursor: pointer;
    transition: all 0.3s;
}

button:hover {
    background: #1d4ed8;
    transform: translateY(-2px);
}

button:active {
    transform: translateY(0);
}
```

</details>

## Bug Hunting
Find the bug in this code:

```css
.buggy-input {
    padding: 10px;
    border: 1px solid #ddd;
    /* The focus state doesn't work */
}

.buggy-input:focus {
    border-color: blue;
}
```

<details>
<summary>Solution</summary>

The focus state might be overridden by browser default styles. Add `outline: none` to ensure custom focus styles are visible.

</details>

## Quiz
1. Why is it important to style focus states?
2. What pseudo-class is used for focus state?
3. How do you remove the default browser focus outline?

<details>
<summary>Quiz Answers</summary>

1. Focus states improve accessibility for keyboard navigation and provide visual feedback
2. The `:focus` pseudo-class is used for focus states
3. Use `outline: none` to remove the default browser focus outline

</details>

---

### Section 9: CSS Specificity & Cascade

## Explain
CSS specificity determines which styles are applied when multiple rules target the same element:

- Specificity hierarchy: Inline > ID > Class > Element > Universal
- Calculated as (ID, Class, Element) - e.g., (1,0,0) for one ID
- Higher specificity wins
- Same specificity: later rule wins

## Example
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
```

## Try it yourself
Determine which color will be applied.

```html
<p id="main" class="text">What color am I?</p>
```

```css
p { color: blue; }
.text { color: red; }
#main { color: green; }
p.text { color: purple; }
```

## Predict
What will happen with this code?

```css
.button {
    background: blue;
}

.btn-primary {
    background: red;
}

<button class="button btn-primary">Click me</button>
```

## Experiment
1. What happens when you add an ID to a class selector?
2. Try combining multiple classes
3. What's the specificity of `div p` vs `.text`?

## Challenge
**Task:** Create a specificity demonstration where:
- Show different specificity levels
- Demonstrate override behavior
- Include a case where order matters
- Time: 10 minutes
- Hint: Use the same element with different selectors

<details>
<summary>Solution</summary>

```html
<div class="container">
    <p id="special" class="highlight">What color am I?</p>
    <p class="highlight">What color am I?</p>
    <p>What color am I?</p>
</div>
```

```css
/* Low specificity */
p {
    color: blue;
}

/* Medium specificity */
.highlight {
    color: green;
}

/* High specificity */
#special {
    color: red;
}

/* Combined specificity */
p.highlight {
    color: purple;
}

/* Order matters (same specificity) */
.highlight {
    color: orange; /* This wins over green */
}
```

</details>

## Bug Hunting
Find the bug in this code:

```css
.button {
    background: blue !important;
}

.btn-primary {
    background: red;
}
```

<details>
<summary>Solution</summary>

The `!important` flag will override the `.btn-primary` rule regardless of specificity. This makes debugging difficult and should be avoided.

</details>

## Quiz
1. What is the specificity of an ID selector?
2. Which has higher specificity: class or element?
3. When does order matter in CSS?

<details>
<summary>Quiz Answers</summary>

1. ID selector has specificity of (1,0,0) or 100 points
2. Class selector has higher specificity than element selector
3. Order matters when selectors have the same specificity

</details>

---

### Section 10: !important

## Explain
The `!important` flag overrides all other CSS rules:

- Overrides specificity completely
- Should be used sparingly
- Makes debugging difficult
- Creates maintenance issues

## Example
```css
.button {
    background-color: #2563eb !important;
}

/* This will be overridden */
.btn-primary {
    background-color: #1d4ed8;
}
```

## Try it yourself
Use !important to override a style.

```html
<div class="box">
    <p class="text">Styled text</p>
</div>
```

```css
.text {
    color: red;
}

.box .text {
    color: blue !important; /* Override the red */
}
```

## Predict
What will happen with this code?

```css
.button {
    background: blue !important;
}

#submit {
    background: red;
}

<button id="submit" class="button">Click</button>
```

## Experiment
1. Can you override !important with another !important?
2. What happens when you use !important on a low-specificity selector?
3. Try to avoid !important and use specificity instead

## Challenge
**Task:** Create a demonstration showing:
- When !important is necessary
- When it creates problems
- How to avoid it with better specificity
- Time: 8 minutes
- Hint: Try overriding third-party styles vs normal styling

<details>
<summary>Solution</summary>

```html
<div class="example">
    <p class="override-me">This text is red due to !important</p>
    <p class="better-way">This text uses better specificity</p>
</div>
```

```css
/* Problematic use of !important */
.override-me {
    color: blue;
}

.override-me {
    color: red !important; /* Overrides the blue */
}

/* Better way - use specificity */
.better-way {
    color: blue;
}

.example .better-way {
    color: red; /* Higher specificity, no !important needed */
}
```

</details>

## Bug Hunting
Find the bug in this code:

```css
.buggy {
    color: red !important;
    color: blue; /* This won't work */
}
```

<details>
<summary>Solution</summary>

The `!important` flag will make the first declaration always win, regardless of order. The blue color will never be applied.

</details>

## Quiz
1. When should you use !important?
2. Does !important override inline styles?
3. Why is !important generally discouraged?

<details>
<summary>Quiz Answers</summary>

1. Only for emergency fixes, overriding third-party styles, or utility classes
2. No, inline styles have higher specificity than !important
3. It creates maintenance issues, makes debugging difficult, and breaks the natural cascade

</details>

---

### Section 11: Margin Collapse

## Explain
Margin collapse occurs when vertical margins of adjacent elements combine:

- Only vertical margins collapse (not horizontal)
- The larger margin wins
- Happens between siblings and parent/child
- Can be prevented with certain CSS properties

## Example
```css
.box1 {
    margin-bottom: 20px;
}

.box2 {
    margin-top: 30px; /* Result: 30px, not 50px */
}
```

## Try it yourself
Create elements that demonstrate margin collapse.

```html
<div class="box1">Box 1</div>
<div class="box2">Box 2</div>
```

```css
.box1 {
    margin-bottom: 20px;
    background: blue;
}

.box2 {
    margin-top: 30px;
    background: red;
}
```

## Predict
What will happen with this code?

```css
.parent {
    margin-top: 20px;
}

.child {
    margin-top: 30px;
}
```

## Experiment
1. What happens with horizontal margins?
2. Try using `overflow: hidden` on parent
3. Does padding prevent margin collapse?

## Challenge
**Task:** Create a demonstration showing:
- Normal margin collapse behavior
- How to prevent margin collapse
- When margin collapse is beneficial
- Time: 10 minutes
- Hint: Use overflow or display properties to prevent collapse

<details>
<summary>Solution</summary>

```html
<div class="example">
    <div class="collapsed">
        <div class="box">Box 1</div>
        <div class="box">Box 2</div>
    </div>
    
    <div class="prevented">
        <div class="box">Box 1</div>
        <div class="box">Box 2</div>
    </div>
</div>
```

```css
.collapsed .box {
    margin: 20px 0;
    /* Margins will collapse */
}

.prevented {
    overflow: hidden; /* Prevents collapse */
}

.prevented .box {
    margin: 20px 0;
    /* Margins won't collapse */
}
```

</details>

## Bug Hunting
Find the bug in this code:

```css
.parent {
    margin-top: 20px;
}

.child {
    margin-top: 30px;
}
/* Expected spacing: 50px, actual: 30px */
```

<details>
<summary>Solution</summary>

The margins are collapsing. Use `overflow: hidden` on parent or `padding-top` on parent to prevent collapse.

</details>

## Quiz
1. Do horizontal margins collapse?
2. What property can prevent margin collapse?
3. Why does margin collapse exist?

<details>
<summary>Quiz Answers</summary>

1. No, only vertical margins collapse
2. Properties like `overflow: hidden`, `display: flex`, or `display: grid` prevent collapse
3. Margin collapse exists to create consistent spacing in document flow and avoid excessive spacing

</details>

---

### Section 12: LVHA Order

## Explain
LVHA (Link-Visited-Hover-Active) is the correct order for link pseudo-classes:

- Order matters because styles cascade
- Later rules override earlier ones
- Ensures proper link interaction states
- Affects user experience significantly

## Example
```css
/* Correct LVHA order */
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

## Try it yourself
Create links with proper LVHA styling.

```html
<a href="#" class="styled-link">Click me</a>
```

```css
/* Add proper LVHA styling */
```

## Predict
What will happen with this wrong order?

```css
a:hover {
    color: red;
}

a:link {
    color: blue;
}
```

## Experiment
1. What happens if you change the order?
2. Try styling visited links differently
3. Does LVHA order affect other elements?

## Challenge
**Task:** Create navigation links with:
- Proper LVHA order
- Different colors for each state
- Underline effects
- Time: 8 minutes
- Hint: Remember the exact order: Link, Visited, Hover, Active

<details>
<summary>Solution</summary>

```html
<nav class="navigation">
    <a href="#" class="nav-link">Home</a>
    <a href="#" class="nav-link">About</a>
    <a href="#" class="nav-link">Contact</a>
</nav>
```

```css
.nav-link:link {
    color: #2563eb;
    text-decoration: none;
}

.nav-link:visited {
    color: #7c3aed;
}

.nav-link:hover {
    color: #1d4ed8;
    text-decoration: underline;
}

.nav-link:active {
    color: #f59e0b;
}
```

</details>

## Bug Hunting
Find the bug in this code:

```css
a:active {
    color: red;
}

a:hover {
    color: blue;
}

a:link {
    color: green;
}
```

<details>
<summary>Solution</summary>

Wrong order - should be LVHA (Link, Visited, Hover, Active). The hover state might not work properly.

</details>

## Quiz
1. What does LVHA stand for?
2. Why is the order important?
3. Which state should come last?

<details>
<summary>Quiz Answers</summary>

1. Link, Visited, Hover, Active
2. Order is important because CSS cascades and later rules override earlier ones
3. Active should come last as it's the most specific user interaction state

</details>

---

### Section 13: Float and Clear

## Explain
Float is a historical CSS property for layout, now largely replaced by Flexbox and Grid:

- `float: left/right` - Elements float to left or right
- `clear: both` - Prevents elements from wrapping around floats
- Mainly for legacy support
- Modern: Use Flexbox or Grid instead

## Example
```css
.float-left {
    float: left;
    margin-right: 20px;
}

.float-right {
    float: right;
    margin-left: 20px;
}

.clearfix::after {
    content: "";
    display: table;
    clear: both;
}
```

## Try it yourself
Create a simple float layout.

```html
<div class="container">
    <div class="float-left">Left</div>
    <div class="float-right">Right</div>
    <div class="clearfix"></div>
</div>
```

```css
.float-left {
    /* Add float left styling */
}

.float-right {
    /* Add float right styling */
}
```

## Predict
What will happen with this code?

```css
.image {
    float: left;
}

.text {
    /* No clear property */
}
```

## Experiment
1. What happens without clearfix?
2. Try floating multiple elements
3. How does float affect document flow?

## Challenge
**Task:** Create a simple layout using float:
- Image on left, text on right
- Proper clearfix to contain floats
- Responsive behavior
- Time: 10 minutes
- Hint: Use the clearfix pattern with ::after

<details>
<summary>Solution</summary>

```html
<div class="float-container">
    <div class="float-image">Image</div>
    <div class="float-text">Text content that wraps around the floated image</div>
</div>
```

```css
.float-container::after {
    content: "";
    display: table;
    clear: both;
}

.float-image {
    float: left;
    width: 100px;
    height: 100px;
    background: #2563eb;
    margin-right: 20px;
}

.float-text {
    /* Text will wrap around the floated image */
}
```

</details>

## Bug Hunting
Find the bug in this code:

```css
.parent {
    /* No clearfix */
}

.child {
    float: left;
}
```

<details>
<summary>Solution</summary>

The parent won't contain the floated child. Add clearfix with `::after` pseudo-element.

</details>

## Quiz
1. What is the modern alternative to float?
2. Why do we need clearfix?
3. Is float still used in modern web development?

<details>
<summary>Quiz Answers</summary>

1. Flexbox and Grid are modern alternatives to float
2. Clearfix is needed to contain floated elements within their parent
3. Float is rarely used in modern development, mainly for legacy support or specific text wrapping

</details>

---

### Section 14: Vendor Prefixes

## Explain
Vendor prefixes are used for CSS features that are experimental or browser-specific:

- `-webkit-` for Chrome/Safari
- `-moz-` for Firefox
- `-ms-` for Internet Explorer
- `-o-` for Opera (older versions)
- Modern browsers rarely need prefixes

## Example
```css
.element {
    -webkit-transform: rotate(10deg);
    -moz-transform: rotate(10deg);
    -ms-transform: rotate(10deg);
    -o-transform: rotate(10deg);
    transform: rotate(10deg);
}
```

## Try it yourself
Add vendor prefixes to a CSS property.

```css
.my-element {
    transform: rotate(10deg);
    /* Add vendor prefixes */
}
```

## Predict
What will happen with this code?

```css
.box {
    -webkit-border-radius: 10px;
    border-radius: 10px;
}
```

## Experiment
1. Do modern browsers still need vendor prefixes?
2. What happens if you only use the standard property?
3. Try using Autoprefixer (conceptually)

## Challenge
**Task:** Create a demonstration showing:
- CSS property with full vendor prefixes
- Modern version without prefixes
- When prefixes are actually needed
- Time: 8 minutes
- Hint: Focus on newer CSS features vs established ones

<details>
<summary>Solution</summary>

```css
/* Modern approach - no prefixes needed for established features */
.modern-card {
    border-radius: 10px;
    box-shadow: 0 2px 4px rgba(0,0,0,0.1);
    transform: translateY(-2px);
}

/* Legacy approach with prefixes (rarely needed now) */
.legacy-card {
    -webkit-border-radius: 10px;
    -moz-border-radius: 10px;
    border-radius: 10px;
    
    -webkit-box-shadow: 0 2px 4px rgba(0,0,0,0.1);
    box-shadow: 0 2px 4px rgba(0,0,0,0.1);
    
    -webkit-transform: translateY(-2px);
    -moz-transform: translateY(-2px);
    -ms-transform: translateY(-2px);
    transform: translateY(-2px);
}
```

</details>

## Bug Hunting
Find the bug in this code:

```css
.element {
    -webkit-transform: rotate(10deg);
    /* Missing standard property */
}
```

<details>
<summary>Solution</summary>

Always include the standard property without prefix after vendor prefixes for forward compatibility.

</details>

## Quiz
1. What does `-webkit-` prefix indicate?
2. Are vendor prefixes still commonly needed?
3. What tool can automatically add vendor prefixes?

<details>
<summary>Quiz Answers</summary>

1. `-webkit-` indicates the property is for WebKit-based browsers (Chrome, Safari)
2. No, most modern CSS features don't need vendor prefixes
3. Tools like Autoprefixer can automatically add vendor prefixes

</details>

---

## Part 2: Comprehensive Project (20 minutes)

### Project: Interactive Dashboard Component

**Task:** Create a sophisticated dashboard component that demonstrates all advanced CSS concepts:

**Requirements:**
1. **List & Table Styling** - Custom lists and responsive data table
2. **Pseudo-classes** - Interactive buttons, hover states, focus states
3. **Pseudo-elements** - Decorative elements, icons, drop caps
4. **Border Radius & Box Shadow** - Modern card styling with depth
5. **Forms** - Styled inputs with proper focus states
6. **Specificity** - Proper CSS organization without !important
7. **LVHA Order** - Proper link styling order

**Time:** 20 minutes
**Hint:** Build a user management dashboard with user cards, data table, and search form

<details>
<summary>Solution</summary>

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Interactive Dashboard</title>
    <link rel="stylesheet" href="dashboard.css">
</head>
<body>
    <div class="dashboard">
        <header class="dashboard-header">
            <h1>User Management Dashboard</h1>
            <nav class="dashboard-nav">
                <a href="#" class="nav-link">Dashboard</a>
                <a href="#" class="nav-link">Users</a>
                <a href="#" class="nav-link">Settings</a>
            </nav>
        </header>

        <div class="search-section">
            <form class="search-form">
                <input type="text" class="search-input" placeholder="Search users...">
                <button type="submit" class="search-button">Search</button>
            </form>
        </div>

        <div class="stats-section">
            <div class="stat-card">
                <div class="stat-icon">👥</div>
                <div class="stat-info">
                    <div class="stat-number">1,234</div>
                    <div class="stat-label">Total Users</div>
                </div>
            </div>
            <div class="stat-card">
                <div class="stat-icon">📊</div>
                <div class="stat-info">
                    <div class="stat-number">567</div>
                    <div class="stat-label">Active</div>
                </div>
            </div>
            <div class="stat-card">
                <div class="stat-icon">⭐</div>
                <div class="stat-info">
                    <div class="stat-number">89</div>
                    <div class="stat-label">Premium</div>
                </div>
            </div>
        </div>

        <div class="table-section">
            <h2>User Data</h2>
            <div class="responsive-table">
                <table class="data-table">
                    <thead>
                        <tr>
                            <th>Name</th>
                            <th>Email</th>
                            <th>Role</th>
                            <th>Status</th>
                            <th>Actions</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr>
                            <td>John Doe</td>
                            <td>john@example.com</td>
                            <td>Admin</td>
                            <td><span class="status active">Active</span></td>
                            <td><button class="action-button">Edit</button></td>
                        </tr>
                        <tr>
                            <td>Jane Smith</td>
                            <td>jane@example.com</td>
                            <td>User</td>
                            <td><span class="status inactive">Inactive</span></td>
                            <td><button class="action-button">Edit</button></td>
                        </tr>
                        <tr>
                            <td>Bob Johnson</td>
                            <td>bob@example.com</td>
                            <td>Moderator</td>
                            <td><span class="status active">Active</span></td>
                            <td><button class="action-button">Edit</button></td>
                        </tr>
                    </tbody>
                </table>
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
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    min-height: 100vh;
    padding: 20px;
}

/* Dashboard Container */
.dashboard {
    max-width: 1200px;
    margin: 0 auto;
    background: white;
    border-radius: 16px;
    box-shadow: 0 10px 40px rgba(0, 0, 0, 0.2);
    overflow: hidden;
}

/* Header */
.dashboard-header {
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    color: white;
    padding: 30px;
    display: flex;
    justify-content: space-between;
    align-items: center;
}

.dashboard-header h1 {
    font-size: 24px;
    font-weight: bold;
}

.dashboard-nav {
    display: flex;
    gap: 20px;
}

/* LVHA Order for Links */
.nav-link:link {
    color: rgba(255, 255, 255, 0.8);
    text-decoration: none;
    font-weight: 500;
    transition: color 0.3s;
}

.nav-link:visited {
    color: rgba(255, 255, 255, 0.8);
}

.nav-link:hover {
    color: white;
    text-decoration: underline;
}

.nav-link:active {
    color: rgba(255, 255, 255, 0.6);
}

/* Search Section */
.search-section {
    padding: 30px;
    background: #f8f9fa;
    border-bottom: 1px solid #e5e7eb;
}

.search-form {
    display: flex;
    gap: 10px;
    max-width: 500px;
}

.search-input {
    flex: 1;
    padding: 12px 16px;
    border: 2px solid #e5e7eb;
    border-radius: 8px;
    font-size: 16px;
    transition: all 0.3s;
}

.search-input:focus {
    outline: none;
    border-color: #667eea;
    box-shadow: 0 0 0 3px rgba(102, 126, 234, 0.1);
}

.search-button {
    padding: 12px 24px;
    background: #667eea;
    color: white;
    border: none;
    border-radius: 8px;
    font-size: 16px;
    font-weight: 600;
    cursor: pointer;
    transition: all 0.3s;
}

.search-button:hover {
    background: #5568d3;
    transform: translateY(-2px);
    box-shadow: 0 4px 8px rgba(102, 126, 234, 0.3);
}

.search-button:active {
    transform: translateY(0);
}

/* Stats Section */
.stats-section {
    display: flex;
    gap: 20px;
    padding: 30px;
}

.stat-card {
    flex: 1;
    background: white;
    border: 1px solid #e5e7eb;
    border-radius: 12px;
    padding: 20px;
    display: flex;
    align-items: center;
    gap: 15px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    transition: all 0.3s;
}

.stat-card:hover {
    transform: translateY(-5px);
    box-shadow: 0 8px 16px rgba(0, 0, 0, 0.15);
}

.stat-icon {
    font-size: 32px;
}

.stat-number {
    font-size: 24px;
    font-weight: bold;
    color: #1f2937;
}

.stat-label {
    color: #6b7280;
    font-size: 14px;
}

/* Table Section */
.table-section {
    padding: 30px;
}

.table-section h2 {
    color: #1f2937;
    margin-bottom: 20px;
}

.responsive-table {
    overflow-x: auto;
}

.data-table {
    width: 100%;
    border-collapse: collapse;
    background: white;
    border-radius: 8px;
    overflow: hidden;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.data-table th,
.data-table td {
    padding: 15px;
    text-align: left;
    border-bottom: 1px solid #e5e7eb;
}

.data-table th {
    background: #667eea;
    color: white;
    font-weight: 600;
    text-transform: uppercase;
    font-size: 12px;
    letter-spacing: 0.5px;
}

.data-table tr:hover {
    background: #f8f9fa;
}

.data-table tr:last-child td {
    border-bottom: none;
}

/* Status badges */
.status {
    display: inline-block;
    padding: 4px 12px;
    border-radius: 20px;
    font-size: 12px;
    font-weight: 600;
}

.status.active {
    background: #dcfce7;
    color: #166534;
}

.status.inactive {
    background: #fee2e2;
    color: #991b1b;
}

/* Action Buttons */
.action-button {
    padding: 6px 12px;
    background: #667eea;
    color: white;
    border: none;
    border-radius: 6px;
    font-size: 12px;
    font-weight: 500;
    cursor: pointer;
    transition: all 0.3s;
}

.action-button:hover {
    background: #5568d3;
    transform: scale(1.05);
}

.action-button:active {
    transform: scale(0.95);
}

/* Responsive */
@media (max-width: 768px) {
    .stats-section {
        flex-direction: column;
    }
    
    .dashboard-header {
        flex-direction: column;
        gap: 15px;
        text-align: center;
    }
    
    .dashboard-nav {
        flex-direction: column;
        gap: 10px;
    }
}
```

</details>

---

## Part 3: Review & Assessment (10 minutes)

### Quick Review Quiz

**Question 1:** What is the correct order for link pseudo-classes?
<details>
<summary>Answer</summary>

LVHA - Link, Visited, Hover, Active

</details>

**Question 2:** What does specificity determine in CSS?
<details>
<summary>Answer</summary>

Which CSS rule takes precedence when multiple rules target the same element

</details>

**Question 3:** When should you use `!important`?
<details>
<summary>Answer</summary>

Only in emergencies, to override third-party styles, or for utility classes

</details>

**Question 4:** What is margin collapse?
<details>
<summary>Answer</summary>

When vertical margins of adjacent elements combine into a single margin

</details>

**Question 5:** What does `:nth-child(even)` select?
<details>
<summary>Answer</summary>

All even-numbered children of their parent

</details>

### Final Challenge

**Task:** Create a tooltip component that uses ALL concepts from this session:
- Pseudo-classes for hover/focus states
- Pseudo-elements for tooltip content
- Border radius and box shadow for styling
- Proper specificity without !important
- LVHA order for any links

**Time:** 10 minutes
**Goal:** Build an accessible, interactive tooltip

<details>
<summary>Solution Hint</summary>

Use `::before` or `::after` for tooltip content, `:hover` and `:focus` for visibility, and proper positioning

</details>

---

## Homework Assignment

**Task:** Create a complete interactive component using pseudo-classes and pseudo-elements.

**Requirements:**
- Interactive buttons with hover/active/focus states
- Styled form with custom checkboxes using `:checked`
- Cards with decorative pseudo-elements (`::before`, `::after`)
- Styled table with hover effects using `:hover`
- Use specificity correctly without !important
- Include proper LVHA order for links
- Demonstrate margin collapse prevention

**Due Date:** Next session

---

## End of Session 4

**Summary:** In this session, you learned about list/table styling, pseudo-classes (user action, structural, form, link), pseudo-elements (::before, ::after, ::first-letter, ::first-line, ::selection), content property, border radius, box shadow, forms styling, CSS specificity & cascade, !important, margin collapse, LVHA order, float and clear, and vendor prefixes. ALL concepts (Sections 1-14) were practiced through interactive exercises (Explain → Example → Try it yourself → Predict → Experiment → Challenge → Bug Hunting → Quiz), followed by a comprehensive dashboard project that integrated all concepts.

**Next Session:** CSS Variables + Flexbox (CSS Variables creation and usage, Flexbox parent and child properties, Navbar and Cards implementation)
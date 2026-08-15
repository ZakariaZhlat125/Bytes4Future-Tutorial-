# CSS Complete Course - Lesson 2
## Inheritance, Typography, Positioning & Advanced Styling

---

# Table of Contents

1. [CSS Inheritance](#1-css-inheritance)
2. [Typography](#2-typography)
3. [Font Family](#3-font-family)
4. [Font Size](#4-font-size)
5. [CSS Units](#5-css-units)
6. [Font Style](#6-font-style)
7. [Font Variant](#7-font-variant)
8. [Font Weight](#8-font-weight)
9. [Mouse Cursor](#9-mouse-cursor)
10. [CSS Calculations](#10-css-calculations)
11. [Opacity](#11-opacity)
12. [Position](#12-position)
13. [Static](#13-static)
14. [Relative Position](#14-relative-position)
15. [Absolute Position](#15-absolute-position)
16. [Fixed Position](#16-fixed-position)
17. [Sticky Position](#17-sticky-position)
18. [Z-Index](#18-z-index)
19. [List Styling](#19-list-styling)
20. [Table Styling](#20-table-styling)
21. [Pseudo-Classes](#21-pseudo-classes)
22. [Pseudo-Elements](#22-pseudo-elements)
23. [First Letter](#23-first-letter)
24. [First Line](#24-first-line)
25. [Before and After](#25-before-and-after)
26. [Content Property](#26-content-property)
27. [CSS Vendor Prefixes](#27-css-vendor-prefixes)
28. [Border Radius](#28-border-radius)
29. [Box Shadow](#29-box-shadow)
30. [Box Sizing](#30-box-sizing)
31. [CSS Transition](#31-css-transition)
32. [!important](#32-important)
33. [Margin Collapse](#33-margin-collapse)
34. [CSS Specificity & Cascade](#34-css-specificity--cascade)
35. [Link States Order (LVHA)](#35-link-states-order-lvha)
36. [Float and Clear](#36-float-and-clear)
37. [Forms (Basic Styling)](#37-forms-basic-styling)

---

# 1. CSS Inheritance

CSS inheritance means that some CSS properties are automatically inherited from a parent element by its children.

Example:

```html
<div class="parent">

    <p>Hello World</p>

</div>
```

```css
.parent {
    color: red;
}
```

The paragraph inherits the `color` from the parent.

The result:

```text
Hello World
```

The text will be red.

---

## Properties That Commonly Inherit

Examples:

```css
color
font-family
font-size
font-style
font-weight
line-height
text-align
```

---

## Properties That Usually Do Not Inherit

Examples:

```css
margin
padding
border
width
height
background
```

---

## Explicit Inheritance

You can force a property to inherit:

```css
.child {
    color: inherit;
}
```

---

## Initial

Reset the property to its initial CSS value:

```css
.child {
    color: initial;
}
```

---

## Unset

```css
.child {
    color: unset;
}
```

`unset` behaves like:

- `inherit` for inherited properties
- `initial` for non-inherited properties

---

# 2. Typography

Typography is the design and arrangement of text.

Good typography improves:

- Readability
- User experience
- Visual hierarchy
- Accessibility
- Professional appearance

Important typography properties include:

```css
font-family
font-size
font-style
font-weight
font-variant
line-height
letter-spacing
word-spacing
```

---

# 3. Font Family

`font-family` controls which font is used.

Example:

```css
body {
    font-family: Arial;
}
```

---

## Font Stack

Always provide fallback fonts.

```css
body {
    font-family: Arial, Helvetica, sans-serif;
}
```

The browser tries:

1. Arial
2. Helvetica
3. Any available sans-serif font

---

## Generic Font Families

Common generic families:

```css
serif
sans-serif
monospace
cursive
fantasy
system-ui
```

Example:

```css
body {
    font-family: system-ui, sans-serif;
}
```

---

## Using a Font Name With Spaces

Use quotes:

```css
body {
    font-family: "Times New Roman", serif;
}
```

---

# 4. Font Size

Controls the size of text.

```css
p {
    font-size: 20px;
}
```

You can use different CSS units.

```css
font-size: 1rem;
```

```css
font-size: 1.5em;
```

```css
font-size: 5vw;
```

---

# 5. CSS Units

CSS units are used to define sizes and distances.

There are two main categories:

- Absolute units
- Relative units

---

## Absolute Units

The most common absolute unit is:

```css
px
```

Example:

```css
width: 300px;
```

Other absolute units include:

```text
cm
mm
in
pt
pc
```

For web development, `px` is the most commonly used absolute unit.

---

# Relative Units

Common relative units:

```text
%
em
rem
vw
vh
vmin
vmax
```

---

## Percentage %

Percentage is relative to another dimension, usually the containing block.

```css
.container {
    width: 80%;
}
```

---

## em

`em` is relative to the font size of the current element or its inherited context.

Example:

```css
.parent {
    font-size: 20px;
}

.child {
    font-size: 2em;
}
```

The child will be approximately:

```text
40px
```

---

## rem

`rem` is relative to the root element's font size.

The root element is usually:

```html
<html>
```

Example:

```css
html {
    font-size: 16px;
}

h1 {
    font-size: 2rem;
}
```

Result:

```text
2 × 16px = 32px
```

`rem` is commonly used for scalable typography and spacing.

---

## vw

`vw` means viewport width.

```css
width: 50vw;
```

50vw means 50% of the viewport width.

---

## vh

`vh` means viewport height.

```css
height: 100vh;
```

100vh represents the viewport height.

---

## vmin

Uses the smaller viewport dimension.

```css
font-size: 5vmin;
```

---

## vmax

Uses the larger viewport dimension.

```css
font-size: 5vmax;
```

---

# 6. Font Style

Controls whether text is normal, italic, or oblique.

```css
font-style: normal;
```

```css
font-style: italic;
```

```css
font-style: oblique;
```

Example:

```css
p {
    font-style: italic;
}
```

---

# 7. Font Variant

Controls alternative font rendering features.

Basic example:

```css
p {
    font-variant: small-caps;
}
```

The text may appear as small capital letters.

Example:

```css
font-variant: normal;
```

```css
font-variant: small-caps;
```

For advanced typography, modern CSS also provides properties such as:

```css
font-variant-caps
font-variant-numeric
font-variant-ligatures
```

---

# 8. Font Weight

Controls how thick the text appears.

Common values:

```css
font-weight: normal;
```

```css
font-weight: bold;
```

Numeric values:

```css
font-weight: 100;
font-weight: 200;
font-weight: 300;
font-weight: 400;
font-weight: 500;
font-weight: 600;
font-weight: 700;
font-weight: 800;
font-weight: 900;
```

Typical usage:

```text
400 → Normal
500 → Medium
600 → Semi Bold
700 → Bold
800 → Extra Bold
```

Example:

```css
h1 {
    font-weight: 700;
}
```

---

# 9. Mouse Cursor

The `cursor` property controls the mouse cursor.

Example:

```css
button {
    cursor: pointer;
}
```

Common values:

```css
default
pointer
text
move
not-allowed
wait
help
crosshair
grab
grabbing
zoom-in
zoom-out
```

---

## Example

```css
.delete-button {
    cursor: not-allowed;
}
```

Use `cursor: pointer` for elements that behave like interactive controls.

For example:

```css
button {
    cursor: pointer;
}
```

---

# 10. CSS Calculations

CSS provides the `calc()` function for mathematical calculations.

Syntax:

```css
property: calc(expression);
```

Example:

```css
.container {
    width: calc(100% - 40px);
}
```

This means:

```text
100% - 40px
```

---

## Different Units

One of the most useful features of `calc()` is combining different units.

```css
width: calc(100vw - 300px);
```

Example:

```css
.main {
    width: calc(100% - 250px);
}
```

Useful for layouts containing:

- Sidebar
- Main content
- Header
- Navigation

---

## Spacing Example

```css
.card {
    margin: calc(10px + 1vw);
}
```

---

# 11. Opacity

`opacity` controls how transparent an element is.

Range:

```text
0 → Completely transparent

1 → Completely visible
```

Example:

```css
.box {
    opacity: 0.5;
}
```

The element becomes 50% visible.

---

## Examples

```css
opacity: 1;
```

Fully visible.

```css
opacity: 0.5;
```

50% visible.

```css
opacity: 0;
```

Invisible.

Important:

```css
opacity: 0;
```

does not remove the element from the page.

The element still exists and may still receive pointer events.

---

# 12. Position

The `position` property controls how an element is positioned.

Main values:

```css
static
relative
absolute
fixed
sticky
```

---

# 13. Static

Default value:

```css
position: static;
```

The element follows the normal document flow.

Properties such as:

```css
top
right
bottom
left
```

do not normally reposition a static element.

---

# 14. Relative Position

```css
position: relative;
```

The element remains in the normal document flow.

You can move it using:

```css
top
right
bottom
left
```

Example:

```css
.box {
    position: relative;
    top: 20px;
    left: 30px;
}
```

The element moves visually.

Its original space is still preserved.

---

# 15. Absolute Position

```css
position: absolute;
```

An absolutely positioned element is removed from the normal document flow.

It is positioned relative to its nearest positioned ancestor.

Example:

```html
<div class="parent">

    <div class="child">
        Hello
    </div>

</div>
```

```css
.parent {
    position: relative;
    width: 400px;
    height: 300px;
}

.child {
    position: absolute;
    top: 20px;
    right: 20px;
}
```

The `.child` is positioned relative to `.parent`.

---

## Important Rule

When using absolute positioning inside a container, the parent commonly uses:

```css
position: relative;
```

Example:

```css
.card {
    position: relative;
}

.badge {
    position: absolute;
    top: 10px;
    right: 10px;
}
```

This is very common for:

- Badges
- Icons
- Close buttons
- Image overlays
- Notification counters

---

# 16. Fixed Position

```css
position: fixed;
```

The element is positioned relative to the viewport.

It stays in place while scrolling.

Example:

```css
.chat-button {
    position: fixed;
    right: 20px;
    bottom: 20px;
}
```

Common use cases:

- Floating buttons
- Chat buttons
- Fixed navigation
- Cookie notifications

---

# 17. Sticky Position

```css
position: sticky;
```

An element behaves like a normal element until a specified scrolling position is reached.

Example:

```css
header {
    position: sticky;
    top: 0;
}
```

Useful for:

- Sticky headers
- Sticky navigation
- Table headers

---

# 18. Z-Index

`z-index` controls the stacking order of positioned elements and other stacking contexts.

Example:

```css
.box-one {
    position: absolute;
    z-index: 1;
}

.box-two {
    position: absolute;
    z-index: 2;
}
```

The element with:

```css
z-index: 2;
```

appears above the element with:

```css
z-index: 1;
```

---

## Common Use Case

```css
.modal {
    position: fixed;
    z-index: 1000;
}
```

This helps place the modal above other interface elements.

---

# 19. List Styling

CSS can style ordered and unordered lists.

HTML:

```html
<ul>
    <li>HTML</li>
    <li>CSS</li>
    <li>JavaScript</li>
</ul>
```

---

## List Style Type

```css
ul {
    list-style-type: square;
}
```

Common values:

```css
disc
circle
square
none
```

For ordered lists:

```css
ol {
    list-style-type: decimal;
}
```

Other examples:

```css
lower-alpha
upper-alpha
lower-roman
upper-roman
```

---

## Remove List Marker

Common in navigation menus:

```css
ul {
    list-style: none;
    padding: 0;
    margin: 0;
}
```

---

## List Style Position

```css
list-style-position: inside;
```

or:

```css
list-style-position: outside;
```

---

# 20. Table Styling

HTML:

```html
<table>

    <thead>
        <tr>
            <th>Name</th>
            <th>Age</th>
        </tr>
    </thead>

    <tbody>
        <tr>
            <td>Ali</td>
            <td>25</td>
        </tr>

        <tr>
            <td>Sara</td>
            <td>28</td>
        </tr>
    </tbody>

</table>
```

---

## Border

```css
table,
th,
td {
    border: 1px solid black;
}
```

---

## Border Collapse

```css
table {
    border-collapse: collapse;
}
```

This removes the space between table borders.

---

## Cell Padding

```css
th,
td {
    padding: 12px;
}
```

---

## Text Alignment

```css
th {
    text-align: left;
}
```

---

## Full Example

```css
table {
    width: 100%;
    border-collapse: collapse;
}

th,
td {
    border: 1px solid #ddd;
    padding: 12px;
    text-align: left;
}

th {
    background: #f5f5f5;
}
```

---

# 21. Pseudo-Classes

Pseudo-classes select an element based on its state or position.

Syntax:

```css
selector:pseudo-class {
    property: value;
}
```

---

## :hover

Applies when the mouse is over an element.

```css
button:hover {
    background: black;
    color: white;
}
```

---

## :focus

Applies when an element receives focus.

```css
input:focus {
    border-color: blue;
}
```

Very important for forms and keyboard accessibility.

---

## :active

Applies while an element is being activated.

```css
button:active {
    transform: scale(0.98);
}
```

---

## :visited

Used for links that the user has already visited.

```css
a:visited {
    color: purple;
}
```

---

## :link

Selects unvisited links.

```css
a:link {
    color: blue;
}
```

---

## :first-child

Selects an element if it is the first child of its parent.

```css
li:first-child {
    color: red;
}
```

---

## :last-child

```css
li:last-child {
    color: blue;
}
```

---

## :nth-child()

Selects elements based on their position.

```css
li:nth-child(2) {
    color: red;
}
```

Selects the second `li`.

---

## Odd and Even

```css
li:nth-child(odd) {
    background: #f5f5f5;
}
```

```css
li:nth-child(even) {
    background: #ddd;
}
```

Very useful for tables.

---

## :not()

Select elements that do not match a selector.

```css
button:not(.primary) {
    background: gray;
}
```

---

## :checked

Used with checkboxes and radio buttons.

```css
input:checked {
    accent-color: green;
}
```

---

## :disabled

```css
button:disabled {
    opacity: 0.5;
    cursor: not-allowed;
}
```

---

## :required

```css
input:required {
    border-color: red;
}
```

---

# 22. Pseudo-Elements

Pseudo-elements allow you to style a specific part of an element or create generated content.

Syntax:

```css
selector::pseudo-element {
    property: value;
}
```

---

# 23. First Letter

```css
p::first-letter {
    font-size: 40px;
    font-weight: bold;
}
```

Useful for:

- Magazine designs
- Articles
- Editorial pages

---

# 24. First Line

```css
p::first-line {
    font-weight: bold;
}
```

Styles the first line of a paragraph.

---

# 25. Before and After

Common pseudo-elements:

```css
::before
::after
```

Example:

```css
.title::before {
    content: "★ ";
}
```

HTML:

```html
<h2 class="title">
    CSS Course
</h2>
```

Result:

```text
★ CSS Course
```

---

# 26. Content Property

`content` is commonly used with:

```css
::before
::after
```

Example:

```css
.title::after {
    content: " →";
}
```

Important:

```css
content: "";
```

is still required when using `::before` or `::after` for generated decorative content.

Example:

```css
.box::before {
    content: "";
    display: block;
    width: 20px;
    height: 20px;
    background: red;
}
```

---

## Practical Example

Create a line before a heading:

```css
.title::before {
    content: "";
    display: inline-block;
    width: 30px;
    height: 3px;
    background: red;
    margin-right: 10px;
}
```

---

## Important Note

Do not use `::before` or `::after` for important content that users must access.

For example, don't put an important warning only inside:

```css
::before
```

because generated content is not a replacement for semantic HTML content.

---

# 27. CSS Vendor Prefixes

Vendor prefixes were historically used to provide experimental or browser-specific CSS features.

Examples:

```css
-webkit-
-moz-
-ms-
-o-
```

Example:

```css
.example {
    -webkit-user-select: none;
    -moz-user-select: none;
    user-select: none;
}
```

Common prefixes:

```text
-webkit- → Chrome, Safari and related engines
-moz-    → Firefox
-ms-     → Older Microsoft browsers
-o-      → Older Opera
```

---

## Should You Always Use Vendor Prefixes?

No.

Modern browsers support most standard CSS features without manual prefixes.

Use browser compatibility tools such as:

- Can I Use
- Autoprefixer
- Your build tool

when necessary.

---

# 28. Border Radius

`border-radius` creates rounded corners.

Example:

```css
.box {
    border-radius: 10px;
}
```

---

## All Corners

```css
border-radius: 20px;
```

---

## Individual Corners

```css
border-top-left-radius: 10px;

border-top-right-radius: 10px;

border-bottom-right-radius: 10px;

border-bottom-left-radius: 10px;
```

---

## Circle

For a square element:

```css
width: 100px;
height: 100px;
border-radius: 50%;
```

This creates a circle.

Common use cases:

- Profile images
- Avatars
- Buttons
- Cards
- Badges

---

# 29. Box Shadow

`box-shadow` adds a shadow around an element.

Basic syntax:

```css
box-shadow: horizontal vertical blur color;
```

Example:

```css
box-shadow: 5px 5px 10px gray;
```

---

## Parameters

```text
Horizontal Offset
Vertical Offset
Blur Radius
Color
```

Example:

```css
box-shadow: 10px 10px 20px rgba(0, 0, 0, 0.2);
```

---

## Negative Values

```css
box-shadow: -5px -5px 10px gray;
```

Negative values move the shadow to the opposite direction.

---

## Inset

`inset` creates an inner shadow.

```css
box-shadow: inset 0 0 10px gray;
```

---

## Multiple Shadows

You can add multiple shadows.

```css
box-shadow:
    0 2px 5px rgba(0, 0, 0, 0.1),
    0 10px 20px rgba(0, 0, 0, 0.1);
```

---

## Card Example

```css
.card {
    padding: 20px;
    border-radius: 12px;

    box-shadow:
        0 4px 10px rgba(0, 0, 0, 0.1);
}
```

---

# 30. Box Sizing

`box-sizing` controls how the width and height of an element are calculated.

Default:

```css
box-sizing: content-box;
```

---

## Content Box

With:

```css
box-sizing: content-box;
```

the declared width applies to the content only.

Example:

```css
.box {
    width: 300px;
    padding: 20px;
    border: 5px solid black;
}
```

The total width becomes:

```text
300 + 40 + 10 = 350px
```

---

# Border Box

```css
box-sizing: border-box;
```

Now the declared width includes:

- Content
- Padding
- Border

Example:

```css
.box {
    width: 300px;
    padding: 20px;
    border: 5px solid black;
    box-sizing: border-box;
}
```

The total width remains:

```text
300px
```

---

## Recommended Global Rule

A very common CSS reset is:

```css
* {
    box-sizing: border-box;
}
```

This makes layouts easier to control.

---

# 31. CSS Transition

Transitions create smooth changes between CSS property values.

Syntax:

```css
transition: property duration timing-function;
```

Example:

```css
button {
    background: blue;
    transition: background 0.3s ease;
}

button:hover {
    background: red;
}
```

The color changes smoothly.

---

## Transition All

```css
transition: all 0.3s ease;
```

This is convenient but can sometimes animate properties you did not intend to animate.

Prefer specifying the properties when possible.

```css
transition:
    transform 0.3s ease,
    opacity 0.3s ease;
```

---

## Timing Functions

Common values:

```css
ease
linear
ease-in
ease-out
ease-in-out
```

Example:

```css
transition: transform 0.3s ease-in-out;
```

---

# 32. !important

`!important` increases the priority of a CSS declaration.

Example:

```css
.title {
    color: red !important;
}
```

It can override normal declarations.

---

## Why Should You Avoid It?

Using too much `!important` creates difficult CSS.

Bad:

```css
.title {
    color: red !important;
}

.title {
    color: blue !important;
}

.title {
    color: green !important;
}
```

This makes the CSS difficult to maintain.

---

## When Can It Be Useful?

Use it carefully when:

- Overriding third-party CSS
- Utility classes require a deliberate priority
- You need to override an existing declaration that cannot reasonably be changed

Avoid using it as a solution to selector problems.

---

# 33. Margin Collapse

Margin collapse happens when vertical margins of block elements combine instead of adding together.

Example:

```html
<div class="one"></div>
<div class="two"></div>
```

```css
.one {
    margin-bottom: 30px;
}

.two {
    margin-top: 20px;
}
```

You might expect:

```text
30 + 20 = 50px
```

But in a normal block formatting context, the margins can collapse.

The resulting space can be:

```text
30px
```

The larger margin wins.

---

## Parent and Child Margin Collapse

Example:

```html
<div class="parent">

    <h1>Hello</h1>

</div>
```

```css
.parent {
    margin-top: 20px;
}

h1 {
    margin-top: 30px;
}
```

The margins may collapse through the parent.

---

## How to Prevent Margin Collapse

Several techniques can create separation, depending on the situation.

### Use Padding

```css
.parent {
    padding-top: 1px;
}
```

---

### Use Border

```css
.parent {
    border-top: 1px solid transparent;
}
```

---

### Create a New Formatting Context

For example:

```css
.parent {
    display: flow-root;
}
```

---

### Use Flexbox

```css
.parent {
    display: flex;
    flex-direction: column;
}
```

Flex containers do not have normal margin collapsing between their flex items.

---

# 34. Position Example

A very common real-world example is an image card with a badge.

HTML:

```html
<div class="card">

    <img src="product.jpg" alt="Product">

    <span class="badge">
        New
    </span>

</div>
```

CSS:

```css
.card {
    position: relative;
    width: 300px;
}

.card img {
    width: 100%;
    display: block;
}

.badge {
    position: absolute;

    top: 10px;
    right: 10px;

    padding: 5px 10px;

    color: white;
    background: red;

    border-radius: 6px;
}
```

The parent:

```css
.card {
    position: relative;
}
```

creates the positioning context.

The badge:

```css
.badge {
    position: absolute;
}
```

is positioned relative to the card.

---

# 35. Pseudo-Class Example

HTML:

```html
<button class="button">
    Submit
</button>
```

CSS:

```css
.button {
    padding: 10px 20px;
    background: blue;
    color: white;
    border: none;

    cursor: pointer;

    transition:
        background 0.3s ease,
        transform 0.2s ease;
}

.button:hover {
    background: darkblue;
}

.button:active {
    transform: scale(0.97);
}
```

This creates:

- Normal state
- Hover state
- Active state
- Smooth transition

---

# 36. Practice Exercises

## Exercise 1 - Typography

Create a page containing:

- H1
- H2
- Paragraph

Apply:

- Different font sizes
- Different font weights
- Different font styles
- Different line heights

---

## Exercise 2 - Position

Create a card.

Requirements:

- Card width: 300px
- Image
- Badge in the top-right corner
- Badge must use `position: absolute`
- Card must use `position: relative`

---

## Exercise 3 - Pseudo-Class

Create a button with:

- Normal state
- Hover state
- Active state
- Disabled state
- Smooth transition

---

## Exercise 4 - Pseudo-Element

Create a heading with a decorative line before it.

Use:

```css
::before
```

and:

```css
content: "";
```

---

## Exercise 5 - Table

Create a student table.

Requirements:

- Border
- Padding
- Header background
- Alternating row colors
- Border collapse
- Hover effect

Hint:

```css
tr:nth-child(even) {
    background: #f5f5f5;
}

tr:hover {
    background: #eee;
}
```

---

## Exercise 6 - Text Overflow

Create a card with a long title.

Make the title display:

```text
This is a very long...
```

Use:

```css
overflow: hidden;
white-space: nowrap;
text-overflow: ellipsis;
```

---

# 37. Mini Project

## Product Card

Build a product card containing:

- Product image
- Product name
- Description
- Price
- Discount badge
- Buy button

Requirements:

### Card

```css
position: relative;
```

### Badge

```css
position: absolute;
```

### Button

Use:

```css
:hover
:active
```

### Image

Use:

```css
border-radius
```

### Card

Use:

```css
box-shadow
```

### Button

Use:

```css
transition
```

### Layout

Use:

```css
box-sizing: border-box;
```

---

# 34. CSS Specificity & Cascade

CSS specificity determines which styles are applied when multiple rules target the same element.

## The Cascade

CSS stands for "Cascading Style Sheets". The cascade is the algorithm that determines which property values get applied to an element.

**Cascade Order (highest to lowest priority):**
1. `!important` (overrides everything)
2. Inline styles (in HTML)
3. ID selectors
4. Class selectors, attribute selectors, pseudo-classes
5. Element selectors, pseudo-elements
6. Universal selector

---

## Specificity Calculation

Each selector type has a specificity value:

| Selector Type | Specificity Value |
|---------------|-------------------|
| Inline style | 1000 |
| ID | 100 |
| Class, pseudo-class, attribute | 10 |
| Element, pseudo-element | 1 |
| Universal selector (*) | 0 |

**Example:**
```css
/* Specificity: 0,0,1 (1 element selector) */
div {
  color: red;
}

/* Specificity: 0,1,0 (1 class selector) */
.box {
  color: blue;
}

/* Specificity: 1,0,0 (1 ID selector) */
#container {
  color: green;
}
```

In the example above, `#container` wins because ID has higher specificity than class.

---

## Specificity Examples

```css
/* Specificity: 0,0,2 (2 element selectors) */
div p {
  color: red;
}

/* Specificity: 0,1,1 (1 class + 1 element) */
.box p {
  color: blue;
}

/* Specificity: 0,2,0 (2 classes) */
.box.text {
  color: green;
}

/* Specificity: 1,0,1 (1 ID + 1 element) */
#container p {
  color: purple;
}
```

The last rule wins because it has the highest specificity.

---

## Common Specificity Mistakes

### ❌ Overusing IDs
```css
/* Bad - Too specific */
#header #nav #menu .item {
  color: red;
}

/* Good - Use classes */
.nav-item {
  color: red;
}
```

### ❌ Over-qualifying selectors
```css
/* Bad - Unnecessary specificity */
div.container p.text {
  color: blue;
}

/* Good - Just the class */
.text {
  color: blue;
}
```

### ❌ Using !important
```css
/* Bad - Avoid !important */
.text {
  color: red !important;
}

/* Good - Use more specific selector */
.container .text {
  color: red;
}
```

---

## Best Practices

✅ Use classes for styling, IDs for JavaScript hooks

✅ Keep specificity low and flat

✅ Avoid `!important` unless absolutely necessary

✅ Use meaningful class names

✅ Group related styles together

---

## Practice

Which rule wins?

```css
/* Rule A: 0,1,0 */
.button {
  color: blue;
}

/* Rule B: 0,0,1 */
button {
  color: red;
}
```

**Answer:** Rule A (class selector has higher specificity than element selector)

---

# 35. Link States Order (LVHA)

When styling links, the order of pseudo-classes matters. The correct order is **LVHA**:

- **L** - `:link` (unvisited links)
- **V** - `:visited` (visited links)
- **H** - `:hover` (mouse over)
- **A** - `:active` (being clicked)

## Why Order Matters

CSS applies styles in order. If you put `:hover` before `:link`, the link style will override the hover style.

---

## Correct Order (LVHA)

```css
/* 1. Link - Unvisited */
a:link {
  color: blue;
}

/* 2. Visited */
a:visited {
  color: purple;
}

/* 3. Hover - Mouse over */
a:hover {
  color: red;
  text-decoration: underline;
}

/* 4. Active - Being clicked */
a:active {
  color: orange;
}
```

---

## Common Mistake

```css
/* ❌ Wrong order - hover won't work properly */
a:hover {
  color: red;
}

a:link {
  color: blue;
}
```

The `:link` rule comes after `:hover`, so it overrides the hover style.

---

## Shorthand for All Links

```css
/* Style all link states at once */
a {
  color: blue;
  text-decoration: none;
}

/* Then override specific states */
a:hover {
  color: red;
  text-decoration: underline;
}
```

This is often simpler than using all four pseudo-classes.

---

## Focus State

Don't forget the `:focus` state for accessibility:

```css
a:focus {
  outline: 2px solid blue;
  outline-offset: 2px;
}
```

---

# 36. Float and Clear

Before Flexbox and Grid, `float` was used for layout. It's still useful for text wrapping around images.

## Float Property

The `float` property moves an element to the left or right, allowing text to wrap around it.

```css
.float-left {
  float: left;
}

.float-right {
  float: right;
}

.float-none {
  float: none;
}
```

---

## Float Example

```html
<div class="container">
  <img src="image.jpg" class="float-left" alt="Image">
  <p>This text will wrap around the floated image. The image is floated to the left, so the text flows around it on the right side.</p>
</div>
```

```css
.float-left {
  float: left;
  margin-right: 20px;
  margin-bottom: 10px;
}
```

---

## Clear Property

The `clear` property prevents elements from wrapping around floated elements.

```css
.clear-left {
  clear: left;
}

.clear-right {
  clear: right;
}

.clear-both {
  clear: both;
}

.clear-none {
  clear: none;
}
```

---

## Clearfix

When you float elements, the parent container collapses. Use clearfix to fix this:

```css
.clearfix::after {
  content: "";
  display: table;
  clear: both;
}
```

**Usage:**
```html
<div class="container clearfix">
  <div class="float-left">Left</div>
  <div class="float-right">Right</div>
</div>
```

---

## Modern Alternative

For layout, use Flexbox or Grid instead of float:

```css
/* Modern approach */
.container {
  display: flex;
  justify-content: space-between;
}
```

---

## When to Use Float

✅ Text wrapping around images

✅ Magazine-style layouts

❌ Page layout (use Flexbox/Grid instead)

---

# 37. Forms (Basic Styling)

Forms are essential for user interaction. Here's how to style them.

## Input Styling

```css
input[type="text"],
input[type="email"],
input[type="password"],
textarea {
  width: 100%;
  padding: 12px;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 16px;
  box-sizing: border-box;
}
```

---

## Focus State

Style inputs when they're focused:

```css
input:focus,
textarea:focus {
  outline: none;
  border-color: #3498db;
  box-shadow: 0 0 0 3px rgba(52, 152, 219, 0.2);
}
```

---

## Placeholder Styling

Style the placeholder text:

```css
::placeholder {
  color: #999;
  font-style: italic;
}
```

---

## Button Styling

```css
button,
input[type="submit"],
input[type="button"] {
  padding: 12px 24px;
  background-color: #3498db;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 16px;
  transition: background-color 0.3s;
}

button:hover,
input[type="submit"]:hover {
  background-color: #2980b9;
}

button:active {
  transform: scale(0.98);
}
```

---

## Disabled State

Style disabled inputs:

```css
input:disabled,
button:disabled {
  background-color: #f0f0f0;
  color: #999;
  cursor: not-allowed;
}
```

---

## Form Layout Example

```html
<form class="contact-form">
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
  
  <button type="submit">Send Message</button>
</form>
```

```css
.contact-form {
  max-width: 400px;
  margin: 0 auto;
}

.form-group {
  margin-bottom: 20px;
}

label {
  display: block;
  margin-bottom: 8px;
  font-weight: bold;
}

input,
textarea {
  width: 100%;
  padding: 12px;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 16px;
  box-sizing: border-box;
}

button {
  width: 100%;
  padding: 12px;
  background-color: #3498db;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 16px;
}
```

---

## Checkbox and Radio Styling

```css
input[type="checkbox"],
input[type="radio"] {
  width: 18px;
  height: 18px;
  cursor: pointer;
}
```

---

## Select Dropdown

```css
select {
  width: 100%;
  padding: 12px;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 16px;
  background-color: white;
  cursor: pointer;
}
```

# CSS Complete Course - Lesson 3

## CSS Variables, Flexbox, Filters, Gradients & CSS Grid

---

# Table of Contents

1. [CSS Variables](#1-css-variables)
2. [Why Use CSS Variables?](#2-why-use-css-variables)
3. [Creating CSS Variables](#3-creating-css-variables)
4. [Using CSS Variables](#4-using-css-variables)
5. [Variable Fallback](#5-variable-fallback)
6. [Local Variables](#6-local-variables)
7. [Flexbox Introduction](#7-flexbox-introduction)
8. [Flexbox Parent Properties](#8-flexbox-parent-properties)
9. [Flex Direction](#9-flex-direction)
10. [Flex Wrap](#10-flex-wrap)
11. [Flex Flow](#11-flex-flow)
12. [Justify Content](#12-justify-content)
13. [Align Items](#13-align-items)
14. [Align Content](#14-align-content)
15. [Flexbox Child Properties](#15-flexbox-child-properties)
16. [Flex Grow](#16-flex-grow)
17. [Flex Shrink](#17-flex-shrink)
18. [Order](#18-order)
19. [Flex Basis](#19-flex-basis)
20. [Flex Shorthand](#20-flex-shorthand)
21. [Align Self](#21-align-self)
22. [Flexbox Game](#22-flexbox-game)
23. [Flexbox Practice Task](#23-flexbox-practice-task)
24. [CSS Filters](#24-css-filters)
25. [Grayscale](#25-grayscale)
26. [Blur](#26-blur)
27. [Invert](#27-invert)
28. [Combining Filters](#28-combining-filters)
29. [Gradients](#29-gradients)
30. [Linear Gradient](#30-linear-gradient)
31. [Pointer Events](#31-pointer-events)
32. [Caret Color](#32-caret-color)
33. [CSS Grid](#33-css-grid)
34. [Grid Template Columns](#34-grid-template-columns)
35. [Repeat()](#35-repeat)
36. [Fraction Unit](#36-fraction-unit)
37. [Grid Template Rows](#37-grid-template-rows)
38. [Grid Gap](#38-grid-gap)
39. [Grid Alignment](#39-grid-alignment)
40. [Justify Content](#40-justify-content)
41. [Align Content](#41-align-content)
42. [Grid Template Areas](#42-grid-template-areas)
43. [Complete Grid Layout Example](#43-complete-grid-layout-example)
44. [Grid Child Properties](#44-grid-child-properties)
45. [Grid Column](#45-grid-column)
46. [Grid Row](#46-grid-row)
47. [Grid Area](#47-grid-area)
48. [Minmax()](#48-minmax)
49. [Auto Fill](#49-auto-fill)
50. [Responsive Card Grid](#50-responsive-card-grid)
51. [CSS Grid Garden](#51-css-grid-garden)
52. [Flexbox vs Grid](#52-flexbox-vs-grid)
53. [Practice Task - Flexbox](#53-practice-task---flexbox)
54. [Practice Task - Flex Cards](#54-practice-task---flex-cards)
55. [Practice Task - Grid](#55-practice-task---grid)
56. [Practice Task - Dashboard](#56-practice-task---dashboard)
57. [Mini Project](#57-mini-project)
58. [Best Practices](#58-best-practices)
59. [Important Properties Learned](#59-important-properties-learned)
60. [Summary](#60-summary)

---

# 1. CSS Variables

CSS Variables are reusable values that can be stored and used throughout your CSS.

They are also called:

> **Custom Properties**

Example:

```css
:root {
    --primary-color: #2563eb;
    --text-color: #222;
    --spacing: 20px;
}
```

Now these values can be reused.

```css
button {
    background-color: var(--primary-color);
    color: white;
    padding: var(--spacing);
}
```

---

# 2. Why Use CSS Variables?

Without variables:

```css
button {
    background: #2563eb;
}

a {
    color: #2563eb;
}

.title {
    color: #2563eb;
}
```

If you want to change the primary color, you must change it in multiple places.

With variables:

```css
:root {
    --primary-color: #2563eb;
}
```

Use:

```css
button {
    background: var(--primary-color);
}

a {
    color: var(--primary-color);
}

.title {
    color: var(--primary-color);
}
```

Now you only need to change one value.

---

# 3. Creating CSS Variables

CSS variables usually start with:

```css
--
```

Example:

```css
:root {
    --primary-color: blue;
    --secondary-color: orange;
    --border-radius: 10px;
    --spacing: 20px;
}
```

---

# 4. Using CSS Variables

Use the `var()` function.

```css
button {
    background-color: var(--primary-color);
}
```

Another example:

```css
.card {
    padding: var(--spacing);
    border-radius: var(--border-radius);
}
```

---

# 5. Variable Fallback

You can provide a fallback value.

```css
color: var(--text-color, black);
```

If `--text-color` does not exist, `black` will be used.

---

# 6. Local Variables

Variables do not have to be global.

```css
.card {
    --card-padding: 20px;

    padding: var(--card-padding);
}
```

The variable is available inside the `.card` element and its descendants.

---

# 7. Flexbox Introduction

Flexbox is a CSS layout system designed to arrange elements in one dimension.

It is excellent for:

* Navigation bars
* Buttons
* Cards
* Centering elements
* Rows
* Columns
* Small and medium-sized layouts

Enable Flexbox:

```css
.container {
    display: flex;
}
```

Example:

```html
<div class="container">

    <div>One</div>
    <div>Two</div>
    <div>Three</div>

</div>
```

```css
.container {
    display: flex;
}
```

The children will be arranged in a row by default.

---

# 8. Flexbox Parent Properties

The main Flexbox parent properties are:

```css
flex-direction
flex-wrap
flex-flow
justify-content
align-items
align-content
gap
```

---

# 9. Flex Direction

Controls the main axis direction.

```css
flex-direction: row;
```

Default:

```text
→ → →
```

---

## Row Reverse

```css
flex-direction: row-reverse;
```

```text
← ← ←
```

---

## Column

```css
flex-direction: column;
```

```text
↓
↓
↓
```

---

## Column Reverse

```css
flex-direction: column-reverse;
```

```text
↑
↑
↑
```

---

# 10. Flex Wrap

By default, Flexbox tries to keep items on one line.

```css
flex-wrap: nowrap;
```

---

## Wrap

```css
flex-wrap: wrap;
```

Items move to a new line when there is not enough space.

This is useful for:

* Product cards
* Tags
* Responsive layouts

---

## Wrap Reverse

```css
flex-wrap: wrap-reverse;
```

---

# 11. Flex Flow

`flex-flow` is shorthand for:

```css
flex-direction
flex-wrap
```

Instead of:

```css
.container {
    flex-direction: row;
    flex-wrap: wrap;
}
```

You can write:

```css
.container {
    flex-flow: row wrap;
}
```

Example:

```css
flex-flow: column nowrap;
```

---

# 12. Justify Content

`justify-content` controls how items are distributed along the **main axis**.

Example:

```css
.container {
    display: flex;
    justify-content: center;
}
```

---

## Values

### Start

```css
justify-content: flex-start;
```

Items start at the beginning.

---

### End

```css
justify-content: flex-end;
```

Items move to the end.

---

### Center

```css
justify-content: center;
```

Items are centered.

---

### Space Between

```css
justify-content: space-between;
```

Equal space between items.

No extra space at the edges.

---

### Space Around

```css
justify-content: space-around;
```

Space is distributed around each item.

---

### Space Evenly

```css
justify-content: space-evenly;
```

Equal space between items and edges.

---

# 13. Align Items

`align-items` controls alignment along the **cross axis**.

Example:

```css
.container {
    display: flex;
    align-items: center;
}
```

Common values:

```css
flex-start
flex-end
center
stretch
baseline
```

---

## Center an Element

A common technique:

```css
.container {
    display: flex;
    justify-content: center;
    align-items: center;
}
```

This centers the children on both axes.

---

# 14. Align Content

`align-content` controls the distribution of **multiple flex lines**.

It only becomes useful when:

```css
flex-wrap: wrap;
```

is active and there are multiple lines.

Example:

```css
.container {
    display: flex;
    flex-wrap: wrap;
    align-content: center;
}
```

Common values:

```css
flex-start
flex-end
center
space-between
space-around
space-evenly
stretch
```

Important:

`align-items` aligns items within a line.

`align-content` aligns multiple lines.

---

# 15. Gap

`gap` creates consistent space between flex items.

```css
.container {
    display: flex;
    gap: 20px;
}
```

You can also use:

```css
row-gap: 20px;
column-gap: 30px;
```

---

# 16. Flexbox Child Properties

The main child properties are:

```css
flex-grow
flex-shrink
flex-basis
flex
order
align-self
```

---

# 17. Flex Grow

Controls how much an item can grow when extra space is available.

```css
.item {
    flex-grow: 1;
}
```

Example:

```css
.item-one {
    flex-grow: 1;
}

.item-two {
    flex-grow: 2;
}
```

The second item receives approximately twice as much of the available extra space as the first.

---

# 18. Flex Shrink

Controls how an item shrinks when there is not enough space.

Default:

```css
flex-shrink: 1;
```

Prevent shrinking:

```css
flex-shrink: 0;
```

Example:

```css
.sidebar {
    flex-shrink: 0;
    width: 250px;
}
```

This is useful when you want to prevent a sidebar from becoming smaller than its intended width.

---

# 19. Order

Controls the visual order of flex items.

HTML:

```html
<div class="one">One</div>
<div class="two">Two</div>
<div class="three">Three</div>
```

CSS:

```css
.one {
    order: 3;
}

.two {
    order: 1;
}

.three {
    order: 2;
}
```

Visual order:

```text
Two
Three
One
```

Default:

```css
order: 0;
```

Important:

Changing visual order does not necessarily change the DOM order or the reading order used by assistive technologies.

Use `order` carefully for accessibility.

---

# 20. Flex Basis

Defines the initial main-axis size of a flex item.

```css
.item {
    flex-basis: 300px;
}
```

For a row:

```text
flex-basis → width-like dimension
```

For a column:

```text
flex-basis → height-like dimension
```

---

# 21. Flex Shorthand

The `flex` property combines:

```css
flex-grow
flex-shrink
flex-basis
```

Example:

```css
.item {
    flex: 1;
}
```

Common:

```css
flex: 1;
```

This is often used to make multiple items share available space.

Another example:

```css
.item {
    flex: 1 1 200px;
}
```

Meaning:

```text
grow: 1
shrink: 1
basis: 200px
```

---

# 22. Align Self

`align-self` overrides the parent's `align-items` for one specific child.

Parent:

```css
.container {
    display: flex;
    align-items: center;
}
```

One child:

```css
.item {
    align-self: flex-start;
}
```

The specific item moves independently along the cross axis.

---

# 23. Flexbox Game

A great way to practice Flexbox:

[Flexbox Froggy](https://flexboxfroggy.com/)

The game helps you practice:

* `justify-content`
* `align-items`
* `flex-direction`
* `flex-wrap`
* `align-content`
* `order`
* `align-self`

Complete the game after learning the basic Flexbox properties.

---

# 24. Flexbox Practice Task

Create:

```text
Header
-------------------------
Logo     Home About Login
-------------------------
```

Requirements:

* Use `display: flex`
* Center items vertically
* Add spacing using `gap`
* Push Login to the right
* Make the layout responsive

---

# 25. CSS Filters

The `filter` property applies visual effects to elements.

Syntax:

```css
filter: function(value);
```

Example:

```css
img {
    filter: grayscale(100%);
}
```

---

# 26. Grayscale

Converts an image to grayscale.

```css
filter: grayscale(100%);
```

Example:

```css
.image {
    filter: grayscale(100%);
}
```

---

## Hover Effect

```css
.image {
    filter: grayscale(100%);
    transition: filter 0.3s ease;
}

.image:hover {
    filter: grayscale(0%);
}
```

Use cases:

* Gallery effects
* Team member images
* Portfolio images

---

# 27. Blur

Creates a blur effect.

```css
filter: blur(5px);
```

Example:

```css
.image {
    filter: blur(5px);
}
```

Common use cases:

* Background effects
* Loading placeholders
* Visual effects

Be careful not to make important content unreadable.

---

# 28. Invert

Inverts the colors.

```css
filter: invert(100%);
```

Example:

```css
img {
    filter: invert(1);
}
```

Useful in some cases for:

* Icons
* Simple monochrome images
* Dark/light transformations

---

# 29. Combining Filters

Multiple filters can be used together.

```css
.image {
    filter:
        grayscale(100%)
        blur(2px)
        brightness(80%);
}
```

Other useful filter functions include:

```css
brightness()
contrast()
saturate()
sepia()
hue-rotate()
drop-shadow()
```

---

# 30. Gradients

A gradient creates a smooth transition between colors.

CSS provides:

```css
linear-gradient()
radial-gradient()
conic-gradient()
```

---

# 31. Linear Gradient

Syntax:

```css
background: linear-gradient(direction, color1, color2);
```

Example:

```css
.box {
    background: linear-gradient(
        to right,
        blue,
        purple
    );
}
```

---

## Direction

```css
to right
```

```css
to left
```

```css
to bottom
```

```css
to top
```

---

## Degrees

```css
background:
    linear-gradient(
        45deg,
        blue,
        purple
    );
```

---

## Multiple Colors

```css
background:
    linear-gradient(
        to right,
        red,
        yellow,
        green
    );
```

---

# 32. Pointer Events

The `pointer-events` property controls whether an element can be the target of pointer interaction.

Example:

```css
.overlay {
    pointer-events: none;
}
```

The pointer can interact with elements underneath the overlay.

---

## Common Use Case

```html
<div class="card">

    <img src="image.jpg" alt="Product">

    <div class="overlay"></div>

</div>
```

```css
.overlay {
    position: absolute;
    inset: 0;
    pointer-events: none;
}
```

The overlay will not block clicks on the content underneath.

---

## Disable Pointer Interaction

```css
button {
    pointer-events: none;
}
```

Be careful with this because the element may become impossible to interact with using a mouse or pointer.

For disabled form controls, prefer the actual HTML:

```html
<button disabled>
    Submit
</button>
```

---

# 33. Caret Color

The `caret-color` property controls the color of the text cursor inside editable fields.

Example:

```css
input {
    caret-color: red;
}
```

For dark interfaces:

```css
input {
    caret-color: white;
}
```

Example:

```css
textarea {
    caret-color: blue;
}
```

---

# 34. CSS Grid

CSS Grid is a two-dimensional layout system.

It works with:

* Rows
* Columns

Grid is excellent for:

* Dashboards
* Galleries
* Page layouts
* Product grids
* Complex responsive layouts

Enable Grid:

```css
.container {
    display: grid;
}
```

---

# 35. Grid Template Columns

Defines the columns.

Example:

```css
.container {
    display: grid;
    grid-template-columns: 200px 200px 200px;
}
```

This creates three columns.

---

## Percentage Columns

```css
grid-template-columns:
    30% 30% 40%;
```

---

# 36. Repeat()

Instead of writing:

```css
grid-template-columns:
    1fr 1fr 1fr;
```

You can write:

```css
grid-template-columns:
    repeat(3, 1fr);
```

Syntax:

```css
repeat(number, value)
```

Example:

```css
grid-template-columns:
    repeat(4, 200px);
```

---

# 37. Fraction Unit

`fr` means a fraction of the available space.

Example:

```css
grid-template-columns:
    1fr 1fr;
```

Two equal columns.

---

Example:

```css
grid-template-columns:
    1fr 2fr;
```

The second column receives twice as much available space as the first.

---

## Three Columns

```css
grid-template-columns:
    1fr 2fr 1fr;
```

The middle column is twice the size of each outer column.

---

# 38. Grid Template Rows

Defines row sizes.

```css
grid-template-rows:
    100px 200px;
```

Example:

```css
.container {
    display: grid;

    grid-template-columns:
        1fr 1fr;

    grid-template-rows:
        100px 300px;
}
```

---

# 39. Grid Gap

Adds space between grid items.

```css
.container {
    display: grid;
    gap: 20px;
}
```

Individual gaps:

```css
row-gap: 20px;

column-gap: 30px;
```

---

# 40. Grid Alignment

Grid provides:

```css
justify-content
align-content
justify-items
align-items
justify-self
align-self
```

---

# 41. Justify Content

Controls the grid as a whole along the inline/horizontal axis.

Example:

```css
.container {
    display: grid;
    justify-content: center;
}
```

Common values:

```css
start
end
center
space-between
space-around
space-evenly
```

---

# 42. Align Content

Controls the grid as a whole along the block/vertical axis when there is extra space.

```css
.container {
    display: grid;
    align-content: center;
}
```

---

# 43. Grid Template Areas

`grid-template-areas` allows you to create named layout areas.

Example:

```css
.container {
    display: grid;

    grid-template-columns:
        200px 1fr;

    grid-template-rows:
        auto 1fr auto;

    grid-template-areas:
        "header header"
        "sidebar main"
        "footer footer";
}
```

Assign areas:

```css
.header {
    grid-area: header;
}

.sidebar {
    grid-area: sidebar;
}

.main {
    grid-area: main;
}

.footer {
    grid-area: footer;
}
```

---

# 44. Complete Grid Layout Example

HTML:

```html
<div class="layout">

    <header class="header">
        Header
    </header>

    <aside class="sidebar">
        Sidebar
    </aside>

    <main class="main">
        Main Content
    </main>

    <footer class="footer">
        Footer
    </footer>

</div>
```

CSS:

```css
.layout {
    min-height: 100vh;

    display: grid;

    grid-template-columns:
        240px 1fr;

    grid-template-rows:
        auto 1fr auto;

    grid-template-areas:
        "header header"
        "sidebar main"
        "footer footer";
}

.header {
    grid-area: header;
}

.sidebar {
    grid-area: sidebar;
}

.main {
    grid-area: main;
}

.footer {
    grid-area: footer;
}
```

This creates:

```text
+-----------------------------+
|           Header            |
+----------+------------------+
| Sidebar  |     Main         |
|          |                  |
+----------+------------------+
|           Footer            |
+-----------------------------+
```

---

# 45. Grid Child Properties

Grid children can control where they are placed.

Important properties:

```css
grid-column
grid-row
grid-area
```

---

# 46. Grid Column

Example:

```css
.item {
    grid-column: 1 / 3;
}
```

The item starts at column line 1 and ends at line 3.

Therefore it occupies two columns.

---

## Span

You can also write:

```css
.item {
    grid-column: span 2;
}
```

This makes the item span two columns.

---

# 47. Grid Row

Example:

```css
.item {
    grid-row: 1 / 3;
}
```

The item spans two grid rows.

Or:

```css
.item {
    grid-row: span 2;
}
```

---

# 48. Grid Area

You can use named areas:

```css
.item {
    grid-area: header;
}
```

Or use grid line numbers:

```css
.item {
    grid-area: 1 / 1 / 3 / 3;
}
```

The order is:

```text
row-start
column-start
row-end
column-end
```

---

# 49. Minmax()

`minmax()` defines a minimum and maximum size.

Syntax:

```css
minmax(minimum, maximum)
```

Example:

```css
grid-template-columns:
    minmax(200px, 1fr);
```

The column will be:

* At least 200px
* Up to 1fr

---

# 50. Auto Fill

`auto-fill` can create as many columns as fit.

Example:

```css
.container {
    display: grid;

    grid-template-columns:
        repeat(
            auto-fill,
            minmax(200px, 1fr)
        );

    gap: 20px;
}
```

This is very useful for responsive card layouts.

---

# 51. Responsive Card Grid

HTML:

```html
<div class="products">

    <div class="card">Product 1</div>
    <div class="card">Product 2</div>
    <div class="card">Product 3</div>
    <div class="card">Product 4</div>

</div>
```

CSS:

```css
.products {
    display: grid;

    grid-template-columns:
        repeat(
            auto-fill,
            minmax(220px, 1fr)
        );

    gap: 20px;
}
```

The number of columns automatically changes based on available space.

---

# 52. CSS Grid Garden

Practice Grid using:

[CSS Grid Garden](https://cssgridgarden.com/)

The game helps you practice:

* `grid-template-columns`
* `grid-template-rows`
* `grid-column`
* `grid-row`
* `grid-area`
* `grid-template-areas`

Complete the game after learning the Grid basics.

---

# 53. Flexbox vs Grid

## Flexbox

Best for:

> One-dimensional layouts

Examples:

```text
Row
Column
Navbar
Button group
Card content
```

---

## Grid

Best for:

> Two-dimensional layouts

Examples:

```text
Dashboard
Page layout
Gallery
Product grid
Complex sections
```

---

## Can They Be Used Together?

Yes!

A real project may use:

```text
Grid
 ├── Page Layout
 │
 └── Flexbox
      ├── Navbar
      ├── Cards
      └── Buttons
```

Flexbox and Grid are complementary technologies, not competitors.

---

# 54. Practice Task - Flexbox

Create a navigation bar:

```text
Logo       Home   About   Services       Login
```

Requirements:

* Use Flexbox.
* Use `justify-content`.
* Use `align-items`.
* Use `gap`.
* Add a hover effect.
* Make the navigation responsive.

---

# 55. Practice Task - Flex Cards

Create three cards:

```text
+---------+  +---------+  +---------+
| Card 1  |  | Card 2  |  | Card 3  |
|         |  |         |  |         |
+---------+  +---------+  +---------+
```

Requirements:

* Use `display: flex`.
* Use `flex-wrap`.
* Use `gap`.
* Make cards responsive.
* Use `flex-grow`.

---

# 56. Practice Task - Grid

Create a product gallery:

```text
+---------+ +---------+ +---------+
| Product | | Product | | Product |
+---------+ +---------+ +---------+

+---------+ +---------+ +---------+
| Product | | Product | | Product |
+---------+ +---------+ +---------+
```

Requirements:

* Use CSS Grid.
* Use `repeat()`.
* Use `1fr`.
* Use `gap`.
* Use `minmax()`.
* Use `auto-fill`.

---

# 57. Practice Task - Dashboard

Create:

```text
+--------------------------------+
|             Header             |
+------------+-------------------+
|            |                   |
|  Sidebar   |      Main         |
|            |                   |
|            |                   |
+------------+-------------------+
|             Footer             |
+--------------------------------+
```

Requirements:

* Use Grid.
* Use `grid-template-areas`.
* Use `grid-template-columns`.
* Use `grid-template-rows`.
* Make it responsive.

---

# 58. Mini Project

## Responsive Product Store

Create a responsive product store.

### Requirements

### Header

Use Flexbox:

```css
display: flex;
align-items: center;
justify-content: space-between;
```

---

### Products

Use Grid:

```css
display: grid;
```

and:

```css
repeat(auto-fill, minmax(220px, 1fr));
```

---

### Product Card

Include:

* Product image
* Product name
* Description
* Price
* Button
* Discount badge

---

### CSS Variables

Create:

```css
:root {
    --primary-color: #2563eb;
    --text-color: #222;
    --spacing: 20px;
    --radius: 10px;
}
```

Use them throughout the project.

---

### Button

Add:

* Hover
* Active
* Transition
* Cursor

---

### Image

Add:

* Border radius
* Grayscale effect
* Hover transition

---

# 59. Best Practices

✅ Use CSS variables for repeated values.

✅ Use Flexbox for one-dimensional layouts.

✅ Use Grid for two-dimensional layouts.

✅ Use `gap` instead of unnecessary margins between flex/grid items.

✅ Use `minmax()` and `auto-fill` for responsive grids.

✅ Use `grid-template-areas` for complex page layouts.

✅ Use `position` only when necessary.

✅ Use semantic HTML with your CSS layouts.

✅ Test layouts on different screen sizes.

✅ Keep accessibility in mind.

✅ Don't rely only on visual order.

---

# 60. Important Properties Learned

## CSS Variables

```css
--variable
var()
```

## Flexbox Parent

```css
display: flex;

flex-direction
flex-wrap
flex-flow

justify-content
align-items
align-content

gap
```

## Flexbox Child

```css
flex-grow
flex-shrink
flex-basis
flex
order
align-self
```

## Filters

```css
filter
grayscale()
blur()
invert()
brightness()
contrast()
saturate()
sepia()
hue-rotate()
drop-shadow()
```

## Gradients

```css
linear-gradient()
radial-gradient()
conic-gradient()
```

## Interaction

```css
pointer-events
caret-color
```

## Grid Parent

```css
display: grid;

grid-template-columns
grid-template-rows
grid-template-areas

gap
row-gap
column-gap

justify-content
align-content
justify-items
align-items
```

## Grid Child

```css
grid-column
grid-row
grid-area
justify-self
align-self
```

## Responsive Grid

```css
repeat()
fr
minmax()
auto-fill
auto-fit
```

---

# 61. Summary

In Lesson 3, you learned two of the most important CSS layout systems.

You learned:

* CSS Variables
* Flexbox
* Flexbox Parent Properties
* Flex Direction
* Flex Wrap
* Flex Flow
* Justify Content
* Align Items
* Align Content
* Gap
* Flex Grow
* Flex Shrink
* Order
* Flex Basis
* Flex Shorthand
* Align Self
* CSS Filters
* Grayscale
* Blur
* Invert
* Gradients
* Linear Gradients
* Pointer Events
* Caret Color
* CSS Grid
* Grid Template Columns
* `repeat()`
* `fr`
* Grid Template Rows
* Grid Gap
* Grid Alignment
* Grid Template Areas
* Grid Column
* Grid Row
* Grid Area
* `minmax()`
* `auto-fill`
* Responsive Grid Layouts

---

# Practice Resources

## Flexbox Froggy

https://flexboxfroggy.com/

Practice Flexbox through a game.

---

## CSS Grid Garden

https://cssgridgarden.com/

Practice CSS Grid through a game.

---

# Final Challenge

Build a responsive dashboard using everything from Lessons 1, 2, and 3.

Your dashboard should contain:

* Header
* Sidebar
* Navigation
* Statistics Cards
* Chart Area
* Recent Orders Table
* User Profile
* Footer

Use:

* CSS Variables
* Flexbox
* CSS Grid
* Grid Areas
* Responsive Grid
* `minmax()`
* `auto-fill`
* Typography
* Borders
* Border Radius
* Box Shadows
* Transitions
* Pseudo-Classes
* Pseudo-Elements
* Filters
* Gradients

The goal is not only to make the dashboard look good.

The goal is to understand **why you choose Flexbox, Grid, or another CSS technique for each part of the layout.**

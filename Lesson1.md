# CSS Complete Course - Lesson 1
## Introduction to CSS & Basic Styling

---

# Table of Contents

1. [What is CSS?](#1-what-is-css)
2. [Why Learn CSS?](#2-why-learn-css)
3. [What Do You Need Before Learning CSS?](#3-what-do-you-need-before-learning-css)
4. [How to Study CSS](#4-how-to-study-css)
5. [Create Your First CSS File](#5-create-your-first-css-file)
6. [Connecting CSS to HTML](#6-connecting-css-to-html)
7. [CSS Syntax](#7-css-syntax)
8. [Selectors](#8-css-selectors)
9. [Comments](#9-comments)
10. [Padding](#10-padding)
11. [Margin](#11-margin)
12. [Border](#12-border)
13. [Width & Height](#13-width--height)
14. [Overflow](#14-overflow)
15. [Text Styling](#15-text-styling)
16. [Text Alignment](#16-text-alignment)
17. [Text Direction](#17-text-direction)
18. [Vertical Alignment](#18-vertical-alignment)
19. [Text Decoration](#19-text-decoration)
20. [Text Transform](#20-text-transform)
21. [Text Spacing](#21-text-spacing)
22. [Word Break](#22-word-break)
23. [Text Overflow](#23-text-overflow)
24. [Display Property](#24-display-property)
25. [Backgrounds](#25-backgrounds)
26. [Colors (Advanced)](#26-colors-advanced)
27. [CSS Reset](#27-css-reset)
28. [Best Practices](#28-best-practices)
29. [Practice Exercises](#29-practice-exercises)

---

# 1. What is CSS?

CSS stands for:

> **Cascading Style Sheets**

CSS is used to style HTML elements and control the appearance of web pages.

Think about building a house:

- **HTML** → The Skeleton 🦴
- **CSS** → The Design 🎨
- **JavaScript** → The Brain 🧠

Without CSS:

- Black text
- White background
- Default browser styles

With CSS:

- Colors
- Layouts
- Animations
- Responsive Design
- Beautiful User Interfaces

---

# 2. Why Learn CSS?

CSS is one of the three core technologies of web development.

Learning CSS allows you to become:

- Front-End Developer
- UI Developer
- Web Designer
- Full Stack Developer
- WordPress Developer
- Email Template Developer

Almost every website uses CSS.

---

# 3. What Do You Need Before Learning CSS?

Before learning CSS you should know:

- HTML Basics
- Visual Studio Code (Recommended)
- Chrome Browser
- Firefox Developer Edition

Good habits:

✔ Practice every day

✔ Always search before asking

✔ Build many small projects

✔ Focus on understanding, not memorizing

✔ Start your own ideas and improve them

---

# 4. How to Study CSS

The best learning method is:

```
Watch

↓

Practice

↓

Build

↓

Repeat
```


Practice is the key to mastering CSS.

---

# 5. Create Your First CSS File

Project Structure

```
MyWebsite/

│

├── index.html

└── css/
      └── style.css
```

Never place CSS files on the Desktop.

Always organize your projects.

---

# 6. Connecting CSS to HTML

Inside the `<head>` section:

```html
<link rel="stylesheet" href="css/style.css">
```

Now your HTML page is connected to your CSS file.

---

# 7. CSS Syntax

CSS consists of:

- Selector
- Property
- Value

Example

```css
p {
    color: red;
    font-size: 40px;
}
```

Explanation

```
p           → Selector

color       → Property

red         → Value
```

Every property ends with a semicolon (`;`).

---

# 8. CSS Selectors

Selectors tell CSS which elements to style.

---

## Element Selector

Styles every matching HTML element.

```css
p {
    color: blue;
}
```

```html
<p>Hello</p>
<p>World</p>
```

Both paragraphs become blue.

---

## Class Selector

A class starts with a dot (`.`).

```css
.my-text {
    color: red;
}
```

```html
<p class="my-text">HTML</p>
```

Multiple elements can share the same class.

---

## ID Selector

An ID starts with `#`.

```css
#title {
    color: green;
}
```

```html
<h1 id="title">Welcome</h1>
```

An ID should be unique on the page.

---

## Which Should You Use?

| Selector | Reusable | Recommended |
|----------|----------|-------------|
| Element | Yes | Small styling |
| Class | Yes | ✅ Most common |
| ID | No | Unique elements only |

---

# 9. Comments

Comments help explain your code.

```css
/* Main Navigation */

nav {
    background: black;
}
```

Comments are ignored by the browser.

---

# 10. Padding

Padding creates space **inside** an element.

```
+-------------------------+
| Border                  |
|  +-------------------+  |
|  |     Padding       |  |
|  |  +-------------+  |  |
|  |  |   Content   |  |  |
|  |  +-------------+  |  |
|  +-------------------+  |
+-------------------------+
```

---

## Same value

```css
padding: 20px;
```

All sides = 20px

---

## Two values

```css
padding: 20px 40px;
```

Top & Bottom = 20px

Left & Right = 40px

---

## Three values

```css
padding: 10px 20px 30px;
```

Top = 10

Left & Right = 20

Bottom = 30

---

## Four values

```css
padding: 10px 20px 30px 40px;
```

Top

Right

Bottom

Left

Clockwise direction ⏰

---

## Individual Sides

```css
padding-top: 20px;

padding-right: 15px;

padding-bottom: 30px;

padding-left: 10px;
```

---

# 11. Margin

Margin creates space **outside** the element.

```css
margin: 20px;
```

---

Individual properties

```css
margin-top:

margin-right:

margin-bottom:

margin-left:
```

---

## Auto Centering

```css
width: 400px;

margin: auto;
```

Centers block elements horizontally.

---

# 12. Border

Syntax

```css
border: 2px solid black;
```

Meaning

```
Size

Style

Color
```

---

## Border Size

```css
border-width: 3px;
```

---

## Border Color

```css
border-color: red;
```

---

## Border Style

```css
border-style: solid;
```

Available styles

```
solid

dashed

dotted

double

groove

ridge

inset

outset

none
```

Example

```css
border: 3px dashed blue;
```

---

# 13. Width & Height

```css
width: 300px;

height: 200px;
```

---

## Percentage

```css
width: 100%;
```

---

## Viewport

```css
width: 100vw;

height: 100vh;
```

---

## Fit Content

```css
width: fit-content;
```

The element becomes only as wide as its content.

---

# 14. Overflow

Overflow controls what happens when content is larger than its container.

---

## Visible (Default)

```css
overflow: visible;
```

Content appears outside the box.

---

## Hidden

```css
overflow: hidden;
```

Extra content is clipped.

---

## Scroll

```css
overflow: scroll;
```

Always shows scrollbars.

---

## Auto

```css
overflow: auto;
```

Scrollbars appear only when needed.

---

## Overflow X

```css
overflow-x: auto;
```

Horizontal scrolling.

---

## Overflow Y

```css
overflow-y: auto;
```

Vertical scrolling.

---

# 15. Text Styling

---

## Color

```css
color: red;
```

Other formats

```css
color: #2563EB;

color: rgb(255,0,0);

color: hsl(200,80%,50%);
```

---

## Text Shadow

```css
text-shadow: 2px 2px 5px gray;
```

Syntax

```
X

Y

Blur

Color
```

Example

```css
text-shadow: 0 0 10px red;
```

---

# 16. Text Alignment

```css
text-align: left;

text-align: center;

text-align: right;

text-align: justify;
```

Use Cases

- Center titles
- Right-align Arabic
- Justify long articles

---

# 17. Text Direction

```css
direction: ltr;
```

Left-to-right

Example

English

---

```css
direction: rtl;
```

Right-to-left

Example

Arabic

---

# 18. Vertical Alignment

Used mainly with:

- Images
- Inline Elements
- Table Cells

Example

```css
vertical-align: middle;
```

Other values

```
top

middle

bottom

baseline

text-top

text-bottom
```

---

# 19. Text Decoration

```css
text-decoration: none;
```

Used to remove link underlines.

---

Other values

```css
underline;

overline;

line-through;
```

Example

```css
a {

text-decoration: none;

}
```

---

# 20. Text Transform

Changes text capitalization.

```css
text-transform: uppercase;
```

Output

HELLO WORLD

---

```css
text-transform: lowercase;
```

Output

hello world

---

```css
text-transform: capitalize;
```

Output

Hello World

---

# 21. Text Spacing

---

## Letter Spacing

```css
letter-spacing: 2px;
```

Adds space between letters.

---

## Word Spacing

```css
word-spacing: 10px;
```

Adds space between words.

---

## Line Height

```css
line-height: 1.8;
```

Controls spacing between lines.

---

## Text Indent

```css
text-indent: 50px;
```

Indents the first line of a paragraph.

---

## White Space

Controls how spaces and line breaks are handled.

### Normal

```css
white-space: normal;
```

Default behavior.

---

### No Wrap

```css
white-space: nowrap;
```

Keeps text on one line.

---

### Pre

```css
white-space: pre;
```

Preserves spaces and line breaks.

---

### Pre-wrap

```css
white-space: pre-wrap;
```

Preserves spaces while allowing wrapping.

---

# 22. Word Break

Controls long words.

```css
word-break: break-all;
```

Breaks anywhere if necessary.

---

```css
word-break: keep-all;
```

Avoids breaking words.

---

# 23. Text Overflow

Used with long text.

Usually requires:

```css
overflow: hidden;

white-space: nowrap;

text-overflow: ellipsis;
```

Example

```css
.card-title{

width:200px;

overflow:hidden;

white-space:nowrap;

text-overflow:ellipsis;

}
```

Output

```
This is a very long title...
```

### Other Values

```css
clip;
```

Cuts the text without showing `...`.

Use Cases

- Product cards
- Blog titles
- User names
- Dashboard tables

---

# 24. Display Property

The `display` property controls how an element is displayed on the page. It's one of the most important CSS properties for layout.

## Common Display Values

### display: block

Block elements:
- Take up the full width available
- Start on a new line
- Can have width, height, margin, padding

**Examples:**
```css
div {
  display: block;
}
```

**Default block elements:** `<div>`, `<p>`, `<h1>`-`<h6>`, `<ul>`, `<li>`, `<section>`, `<article>`

---

### display: inline

Inline elements:
- Only take up as much width as needed
- Don't start on a new line
- Cannot have width, height, top/bottom margin
- Can have left/right margin and padding

**Examples:**
```css
span {
  display: inline;
}
```

**Default inline elements:** `<span>`, `<a>`, `<strong>`, `<em>`, `<img>`, `<input>`

---

### display: inline-block

Inline-block elements:
- Behave like inline elements (flow with text)
- Can have width, height, margin, padding like block elements

**Examples:**
```css
.button {
  display: inline-block;
  width: 150px;
  height: 40px;
}
```

**Use cases:**
- Buttons
- Navigation items
- Card layouts

---

### display: none

Hides the element completely:
- Element is removed from the document flow
- Takes up no space
- Not visible to screen readers

**Examples:**
```css
.hidden {
  display: none;
}
```

**Note:** This is different from `visibility: hidden`, which hides the element but keeps its space.

---

## When to Use Each Display Type

| Display Type | Use Case |
|--------------|----------|
| `block` | Main layout containers, paragraphs, headings |
| `inline` | Text styling, links within text |
| `inline-block` | Buttons, navigation items, small components |
| `none` | Hidden elements, toggleable content |

---

## Example

```html
<div class="container">
  <div class="box">Block Element</div>
  <span class="inline">Inline Element</span>
  <div class="inline-block">Inline-Block Element</div>
  <div class="hidden">Hidden Element</div>
</div>
```

```css
.box {
  display: block;
  background: lightblue;
  padding: 10px;
  margin: 10px 0;
}

.inline {
  display: inline;
  background: lightgreen;
  padding: 5px;
}

.inline-block {
  display: inline-block;
  background: lightcoral;
  padding: 10px;
  width: 150px;
}

.hidden {
  display: none;
}
```

---

# 25. Backgrounds

Background properties control the background of an element.

## Background Color

Sets the background color of an element.

```css
.box {
  background-color: #3498db;
  background-color: rgb(52, 152, 219);
  background-color: hsl(204, 70%, 53%);
  background-color: blue; /* Named color */
}
```

---

## Background Image

Adds an image as the background.

```css
.hero {
  background-image: url('image.jpg');
}
```

---

## Background Repeat

Controls how the background image repeats.

```css
.box {
  background-repeat: repeat;    /* Default - repeats both directions */
  background-repeat: repeat-x;  /* Repeats horizontally only */
  background-repeat: repeat-y;  /* Repeats vertically only */
  background-repeat: no-repeat; /* No repetition */
}
```

---

## Background Position

Sets the starting position of the background image.

```css
.box {
  background-position: center;
  background-position: top right;
  background-position: bottom left;
  background-position: 50% 50%;  /* Horizontal Vertical */
  background-position: 100px 50px;
}
```

---

## Background Size

Controls the size of the background image.

```css
.box {
  background-size: cover;      /* Covers entire area */
  background-size: contain;    /* Fits image within area */
  background-size: 100% 100%;  /* Stretch to fit */
  background-size: 50%;         /* Half size */
}
```

---

## Background Attachment

Controls whether the background scrolls with the page.

```css
.box {
  background-attachment: scroll; /* Default - scrolls with page */
  background-attachment: fixed;  /* Fixed position */
  background-attachment: local;  /* Scrolls with element content */
}
```

---

## Background Shorthand

Combines all background properties in one declaration.

```css
.box {
  background: url('image.jpg') no-repeat center center / cover fixed;
}

/* Order: image | repeat | position | / size | attachment */
```

---

## Example

```html
<div class="hero">
  <h1>Welcome</h1>
</div>
```

```css
.hero {
  background: url('background.jpg') no-repeat center center / cover fixed;
  background-color: #333; /* Fallback color */
  height: 400px;
  display: flex;
  align-items: center;
  justify-content: center;
  color: white;
}
```

---

# 26. Colors (Advanced)

Beyond basic colors, CSS offers advanced color options.

## RGBA Colors

RGBA adds transparency to RGB colors.

```css
.box {
  background-color: rgba(52, 152, 219, 0.5);
  /* Red Green Blue Alpha (transparency) */
  /* Alpha: 0 = transparent, 1 = opaque */
}
```

**Example:**
```css
.overlay {
  background-color: rgba(0, 0, 0, 0.7); /* 70% opaque black */
}
```

---

## HSLA Colors

HSLA adds transparency to HSL colors.

```css
.box {
  background-color: hsla(204, 70%, 53%, 0.5);
  /* Hue Saturation Lightness Alpha */
}
```

**HSL Benefits:**
- Easier to adjust colors (just change hue)
- More intuitive for designers

---

## Named Colors

CSS has 140+ named colors.

```css
.box {
  background-color: tomato;
  background-color: cornflowerblue;
  background-color: mediumseagreen;
  background-color: slategray;
}
```

**Common named colors:**
- `red`, `green`, `blue`
- `black`, `white`, `gray`
- `orange`, `yellow`, `purple`
- `pink`, `brown`, `cyan`

---

## currentColor

The `currentColor` keyword uses the value of the `color` property.

```css
.box {
  color: #3498db;
  border: 2px solid currentColor; /* Uses the same blue */
  background-color: currentColor; /* Uses the same blue */
}
```

**Use cases:**
- Consistent theming
- Icons matching text color
- Hover effects

---

## Color Contrast (Accessibility)

Ensure text is readable against its background.

**Good contrast ratios:**
- Normal text: 4.5:1 minimum
- Large text: 3:1 minimum
- UI components: 3:1 minimum

**Example:**
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

---

# 27. CSS Reset

Different browsers have different default styles. A CSS reset ensures consistency.

## Why Use a Reset?

- Browsers apply different default styles
- Inconsistent margins, padding, fonts
- Cross-browser compatibility issues

---

## Simple CSS Reset

```css
/* Simple Reset */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: Arial, sans-serif;
  line-height: 1.6;
}

img {
  max-width: 100%;
  display: block;
}

a {
  text-decoration: none;
  color: inherit;
}
```

---

## Box Sizing Reset

```css
*, *::before, *::after {
  box-sizing: border-box;
}
```

This ensures padding and border are included in the element's width/height.

---

## CSS Normalize

Unlike a reset, normalize.css preserves useful defaults while fixing inconsistencies.

**Benefits:**
- Better than reset for most projects
- Preserves useful browser defaults
- Fixes cross-browser bugs

**Include via CDN:**
```html
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/normalize/8.0.1/normalize.min.css">
```

---

## When to Use Reset vs Normalize

| Approach | When to Use |
|----------|-------------|
| **CSS Reset** | Complete control, starting from scratch |
| **Normalize** | Most projects, preserve useful defaults |
| **Neither** | Small projects, quick prototypes |

---

# 28. Best Practices

✅ Use classes instead of IDs for styling.

✅ Keep your CSS organized.

✅ Write meaningful class names.

✅ Group related styles together.

✅ Add comments for large sections.

✅ Avoid inline CSS.

✅ Use shorthand properties when appropriate.

✅ Keep consistent spacing and indentation.

---

# 29. Practice Exercises

### Exercise 1

Create a paragraph with:

- Red color
- Font size 24px

---

### Exercise 2

Create a box with:

- Width: 300px
- Height: 150px
- Padding: 20px
- Margin: 30px
- Blue border

---

### Exercise 3

Create a heading with:

- Uppercase text
- Center alignment
- Text shadow

---

### Exercise 4

Create a product card that displays:

- A title
- Long text using `text-overflow: ellipsis`
- A border
- Padding
- Margin

---



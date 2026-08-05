# HTML Complete Course for Beginners
### From Zero to Building Real Web Pages

---

# Table of Contents

1. [What is HTML?](#1-what-is-html)
2. [HTML Versions](#2-html-versions)
3. [Why Learn HTML?](#3-why-learn-html)
4. [What Do You Need Before Learning HTML?](#4-what-do-you-need-before-learning-html)
5. [How to Study HTML Effectively](#5-how-to-study-html)
6. [Explore Existing Websites](#6-explore-existing-websites)
7. [Create Your First Project](#7-create-your-first-project)
8. [HTML Document Structure](#8-html-document-structure)
9. [HTML Tags](#9-html-tags)
10. [Head Section](#10-head-section)
11. [Comments](#11-comments)
12. [HTML5 Doctype](#12-html5-doctype)
13. [Headings](#13-headings)
14. [Paragraphs](#14-paragraph)
15. [Block vs Inline Elements](#15-block-vs-inline)
16. [Attributes](#16-attributes)
17. [Global Attributes](#17-global-attributes)
18. [Text Formatting](#18-text-formatting)
19. [Links](#19-links)
20. [Images](#20-images)
21. [Lists](#21-lists)
22. [Tables](#22-tables)
23. [Span, Br, Hr](#23-span-br-hr)
24. [Div](#24-div)
25. [HTML Entities](#25-html-entities)
26. [Semantic Elements](#26-semantic-elements)
27. [Page Layout (Div vs Semantic)](#27-page-layout-div-vs-semantic)
28. [Audio](#28-audio)
29. [Video](#29-video)
30. [Forms](#30-forms)
31. [Accessibility Basics](#31-accessibility-basics)
32. [Live Server](#32-live-server)
33. [Lighthouse](#33-lighthouse)
34. [ARIA Introduction](#34-aria-introduction)
35. [Best Practices](#35-best-practices)

---

# 1. What is HTML?

HTML stands for:

> **HyperText Markup Language**

HTML is **not a programming language**.

It is a **Markup Language** used to structure web pages.

Think about building a house.

- HTML → Skeleton
- CSS → Design & Colors
- JavaScript → Brain & Interactions

Example

```html
<h1>Hello World</h1>
```

Output

# Hello World

---

# 2. HTML Versions

Old version

- HTML 4

Current version

- HTML5

HTML5 introduced:

- Semantic Elements
- Audio
- Video
- Better Forms
- Canvas
- Local Storage

---

# 3. Why Learn HTML?

Every web developer should know HTML.

Career paths:

- Front-End Developer
- Back-End Developer
- Full Stack Developer
- Mobile Developer (React Native, Flutter Web)
- UI Developer
- Email Template Developer
- CMS Developer
- Dashboard Developer
- Reports & Internal Systems

HTML is the foundation of every website.

---

# 4. What Do You Need Before Learning HTML?

## Text Editor

Examples

- Visual Studio Code (Recommended)
- Cursor
- Atom
- Sublime Text
- Vim
- Notepad++

---

## Browser

Recommended

- Google Chrome
- Firefox Developer Edition
- Microsoft Edge

---

## Important Advice

✔ Always Search

✔ Build Projects

✔ Have a Goal

✔ Focus

✔ Practice Daily

✔ Don't Memorize Everything

---

# 5. How to Study HTML

The best learning method is:

Read ➜ Watch ➜ Practice ➜ Repeat

Never watch tutorials only.

Every lesson should end with writing code.

Spend

- 20% Learning
- 80% Practice

---

# 6. Explore Existing Websites

Open any website.

Right Click

Inspect

Open Developer Tools.

Learn how professionals write HTML.

Try removing elements.

Change text.

Edit colors.

Nothing will happen to the real website.

You are only editing your local browser.

---

# 7. Create Your First Project

Don't save projects on Desktop.

Instead create

```
Web Development
    HTML
        Project01
```

Inside Project01

```
index.html
```

---

## Why is it called index.html?

When the browser opens a folder or a web server, it automatically searches for

```
index.html
```

If found,

it becomes the homepage.

---

# 8. HTML Document Structure

Example

```html
<!DOCTYPE html>
<html>

<head>
    <title>My Website</title>
</head>

<body>

</body>

</html>
```

---

# 9. HTML Tags

Tags usually have:

Opening Tag

```html
<p>
```

Closing Tag

```html
</p>
```

Example

```html
<p>Hello</p>
```

Some tags are Self Closing.

Example

```html
<img />
```

---

# 10. Head Section

The head contains page information.

Example

```html
<head>

</head>
```

---

## Meta Tag

Self-closing element.

Example

```html
<meta charset="UTF-8">
```

---

### Character Encoding

Old

```
ISO-8859-1
```

Recommended

```
UTF-8
```

---

### Description

```html
<meta
name="description"
content="Learning HTML">
```

---

## Style

```html
<style>

h1{
color:red;
}

</style>
```

---

## Script

```html
<script>

console.log("Hello");

</script>
```

---

## Link

```html
<link
rel="stylesheet"
href="style.css">
```

---

# 11. Comments

Single line

```html
<!-- Comment -->
```

Multi-line

```html
<!--
Comment
Comment
-->
```

---

# 12. HTML5 Doctype

Always begin every page with

```html
<!DOCTYPE html>
```

It tells the browser to use HTML5.

---

# 13. Headings

```html
<h1>Main Title</h1>

<h2>Section</h2>

<h3>Subsection</h3>

<h4>Heading</h4>

<h5>Heading</h5>

<h6>Heading</h6>
```

Important

Only one **H1** per page.

---

# 14. Paragraph

```html
<p>
This is a paragraph.
</p>
```

Paragraph is a Block Element.

---

# 15. Block vs Inline

## Block

Starts on a new line.

Examples

- div
- p
- h1
- section

## Inline

Stays inside the same line.

Examples

- span
- a
- strong
- em

---

# 16. Attributes

Attributes give extra information.

Example

```html
<img
src="cat.jpg"
alt="Cat">
```

---

# 17. Global Attributes

Available on almost every element.

Examples

- class
- id
- hidden
- title
- style

---

# 18. Text Formatting

## Bold

```html
<b>Bold</b>
```

Visual only.

---

## Strong

```html
<strong>Important</strong>
```

Has semantic meaning.

Better for SEO.

---

## Italic

```html
<i>Italic</i>
```

---

## Emphasis

```html
<em>Important</em>
```

Meaningful emphasis.

---

## Mark

```html
<mark>Highlight</mark>
```

---

## Underline

```html
<u>Underline</u>
```

---

## Small

```html
<small>Small Text</small>
```

---

## Deleted

```html
<del>Old Price</del>
```

---

## Inserted

```html
<ins>New Text</ins>
```

---

## Subscript

```html
H<sub>2</sub>O
```

---

## Superscript

```html
x<sup>2</sup>
```

---

# 19. Links

```html
<a href="https://google.com">
Google
</a>
```

Important attributes

```
href
target
title
```

Open new tab

```html
target="_blank"
```

Tooltip

```html
title="Visit Google"
```

---

# 20. Images

```html
<img
src="image.jpg"
alt="Mountain">
```

Important

Always write alt.

Avoid width and height in HTML.

Prefer CSS.

---

# 21. Lists

## Unordered

```html
<ul>

<li>HTML</li>

<li>CSS</li>

<li>JS</li>

</ul>
```

---

## Ordered

```html
<ol>

<li>Wake Up</li>

<li>Study</li>

</ol>
```

Attributes

```
start
reversed
type
```

---

## Description List

```html
<dl>

<dt>HTML</dt>

<dd>Markup Language</dd>

</dl>
```

---

# 22. Tables

Structure

```html
<table>

<caption>Students</caption>

<thead>

<tr>

<th>Name</th>

<th>Age</th>

</tr>

</thead>

<tbody>

<tr>

<td>Ali</td>

<td>22</td>

</tr>

</tbody>

<tfoot>

</tfoot>

</table>
```

Merge Cells

```html
colspan="2"
```

---

# 23. Span, Br, Hr

Span

```html
<span>Hello</span>
```

Inline element.

Break line

```html
<br>
```

Horizontal line

```html
<hr>
```

---

# 24. Div

The most common container.

```html
<div>

<h2>Title</h2>

<p>Paragraph</p>

</div>
```

---

# 25. HTML Entities

```
&lt;     <
&gt;     >
&copy;   ©
&asymp;  ≈
&nbsp;   Space
&amp;    &
```

---

# 26. Semantic Elements

HTML5 introduced meaningful tags.

```
<header>

<nav>

<main>

<section>

<article>

<aside>

<footer>
```

All are Block Elements.

---

# 27. Layout Examples

## Old Layout

```html
<div class="header"></div>

<div class="menu"></div>

<div class="content"></div>

<div class="footer"></div>
```

---

## Modern Layout

```html
<header></header>

<nav></nav>

<main>

<section>

<article></article>

</section>

<aside></aside>

</main>

<footer></footer>
```

Semantic HTML is easier to read and better for SEO.

---

# 28. Audio

```html
<audio controls>

<source
src="song.mp3"
type="audio/mpeg">

</audio>
```

Attributes

- controls
- autoplay
- muted
- loop

---

# 29. Video

```html
<video controls width="500">

<source
src="video.mp4"
type="video/mp4">

<track
src="subtitle.vtt"
kind="subtitles"
srclang="en">

</video>
```

Attributes

- controls
- autoplay
- muted
- loop
- poster
- width
- height

---

# 30. Forms

Example

```html
<form
action="save.php"
method="POST">

<label for="name">
Name
</label>

<input
id="name"
name="name"
type="text"
required>

<button>
Submit
</button>

</form>
```

---

## Input Types

- text
- password
- email
- number
- color
- date
- datetime-local
- search
- url
- hidden
- radio
- checkbox
- file
- range

---

## Input Attributes

- required
- placeholder
- value
- readonly
- disabled
- autofocus
- minlength
- maxlength
- name

---

## Select

```html
<select>

<option>HTML</option>

<option>CSS</option>

</select>
```

Attributes

- multiple
- selected

---

## Textarea

```html
<textarea
rows="5"
cols="30">
</textarea>
```

---

## Datalist

```html
<input list="languages">

<datalist id="languages">

<option value="HTML">

<option value="CSS">

<option value="JavaScript">

</datalist>
```

---

## Other Useful Elements

- button
- q
- blockquote
- bdi
- wbr
- iframe
- pre
- code

---

# 31. Accessibility Basics

Accessibility means making websites usable for everyone.

Always

- Use semantic elements
- Add alt to images
- Connect label with input
- Use heading order
- Don't use div for everything

---

# 32. Live Server

Install the Live Server extension in VS Code.

Benefits

- Auto Refresh
- Faster Development
- Better Workflow

---

# 33. Lighthouse

Chrome Developer Tools include Lighthouse.

It checks:

- Performance
- Accessibility
- Best Practices
- SEO

Always test your website before publishing.

---

# 34. ARIA Introduction

ARIA improves accessibility.

Example

```html
<button
aria-label="Close Menu">
X
</button>
```

Useful ARIA attributes

- aria-label
- aria-hidden
- aria-expanded
- aria-live
- aria-describedby

Use ARIA only when native HTML cannot solve the problem.

---

# 35. HTML Best Practices

✅ Use HTML5

✅ Use semantic elements

✅ Indent your code

✅ Write meaningful class names

✅ Always use UTF-8

✅ Always include alt on images

✅ One H1 per page

✅ Use labels with form controls

✅ Avoid inline CSS

✅ Validate your HTML

✅ Organize files into folders

```
project/

│

├── index.html

├── pages/

├── css/

├── js/

├── images/

├── videos/

└── audio/
```

---

# Final Advice

Learning HTML is not about memorizing tags.

It is about understanding how web pages are structured.

Practice every day.

Build many small projects.

Read other people's code.

Use Developer Tools.

Search whenever you don't know something.

The more pages you build, the better you become.

Happy Coding! 🚀
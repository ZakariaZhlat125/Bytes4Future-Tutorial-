# CSS Complete Course - Lesson 4
## 2D Transform, 3D Transform, Animations, Selectors & Responsive Design

---

# Table of Contents

1. [CSS Transform](#1-css-transform)
2. [2D Transform](#2-2d-transform)
3. [Scale](#3-scale)
4. [Rotate](#4-rotate)
5. [Translate](#5-translate)
6. [Skew](#6-skew)
7. [Matrix](#7-matrix)
8. [Transform Origin](#8-transform-origin)
9. [3D Transform](#9-3d-transform)
10. [3D Rotate](#10-3d-rotate)
11. [3D Translate](#11-3d-translate)
12. [Perspective](#12-perspective)
13. [Backface Visibility](#13-backface-visibility)
14. [Preserve 3D](#14-preserve-3d)
15. [3D Flip Card](#15-3d-flip-card)
16. [CSS Animation](#16-css-animation)
17. [@keyframes](#17-keyframes)
18. [Animation Properties](#18-animation-properties)
19. [Animation Direction](#19-animation-direction)
20. [Animation Fill Mode](#20-animation-fill-mode)
21. [Animation Play State](#21-animation-play-state)
22. [Animation Training](#22-animation-training)
23. [CSS Selector Reference](#23-css-selector-reference)
24. [Responsive Design](#24-responsive-design)
25. [Media Queries](#25-media-queries)
26. [Mobile-First Design](#26-mobile-first-design)
27. [Inherit, Unset, Initial, Revert](#27-inherit-unset-initial-revert)
28. [CSS Battle](#28-css-battle)
29. [Practice Resources](#29-practice-resources)
30. [Final Challenge](#30-final-challenge)
31. [Lesson Checklist](#31-lesson-checklist)
32. [Summary](#32-summary)

---

# 1. CSS Transform

The `transform` property allows you to visually transform an element without changing the normal document flow.

You can:

- Move an element
- Scale an element
- Rotate an element
- Skew an element
- Create 3D effects

Basic syntax:

```css
.element {
    transform: function(value);
}
```

Example:

```css
.box {
    transform: scale(1.2);
}
```

---

# 2. 2D Transform

2D transformations work on the X and Y axes.

Common functions:

```css
scale()
rotate()
translate()
skew()
matrix()
```

Example:

```css
.box {
    transform:
        translate(20px, 10px)
        rotate(10deg)
        scale(1.1);
}
```

The order of transformations matters.

---

# 3. Scale

`scale()` changes the visual size of an element.

```css
.box {
    transform: scale(1.5);
}
```

## Scale X

```css
transform: scaleX(2);
```

## Scale Y

```css
transform: scaleY(2);
```

## Scale X and Y

```css
transform: scale(2, 1.5);
```

Example:

```css
.card {
    transition: transform 0.3s ease;
}

.card:hover {
    transform: scale(1.05);
}
```

---

# 4. Rotate

`rotate()` rotates an element.

```css
.box {
    transform: rotate(45deg);
}
```

Negative values rotate in the opposite direction:

```css
.box {
    transform: rotate(-45deg);
}
```

## Degrees

```css
rotate(90deg);
```

## Turns

```css
rotate(1turn);
```

`1turn` = `360deg`

Examples:

```css
rotate(0.5turn);  /* 180deg */
rotate(0.25turn); /* 90deg */
```

---

# 5. Translate

`translate()` moves an element visually.

```css
.box {
    transform: translate(50px, 20px);
}
```

Meaning:

```text
X = 50px
Y = 20px
```

## Translate X

```css
transform: translateX(50px);
```

## Translate Y

```css
transform: translateY(20px);
```

Negative values are also possible:

```css
transform: translateX(-50px);
```

---

## Centering With Translate

A common technique:

```css
.element {
    position: absolute;

    left: 50%;
    top: 50%;

    transform: translate(-50%, -50%);
}
```

---

# 6. Skew

`skew()` tilts an element.

```css
.box {
    transform: skew(20deg);
}
```

## Skew X

```css
transform: skewX(20deg);
```

## Skew Y

```css
transform: skewY(20deg);
```

## Skew X and Y

```css
transform: skew(20deg, 10deg);
```

---

# 7. Matrix

`matrix()` is a lower-level way of combining 2D transformations.

Syntax:

```css
transform:
    matrix(
        scaleX,
        skewY,
        skewX,
        scaleY,
        translateX,
        translateY
    );
```

Example:

```css
.box {
    transform:
        matrix(
            1,
            0,
            0,
            1,
            50,
            20
        );
}
```

For most projects, `translate()`, `scale()`, `rotate()`, and `skew()` are easier to understand.

---

# 8. Transform Origin

`transform-origin` controls the point around which a transformation occurs.

Default:

```css
transform-origin: center;
```

Example:

```css
.box {
    transform-origin: top left;
    transform: rotate(45deg);
}
```

Common values:

```css
center
top
bottom
left
right
top left
top right
bottom left
bottom right
```

Percentage example:

```css
transform-origin: 0% 0%;
```

---

# 9. 3D Transform

CSS supports three-dimensional transformations.

3D uses:

```text
X axis → horizontal
Y axis → vertical
Z axis → depth
```

Common functions:

```css
rotateX()
rotateY()
rotateZ()
translate3d()
translateZ()
perspective()
```

---

# 10. 3D Rotate

## Rotate X

```css
.box {
    transform: rotateX(60deg);
}
```

## Rotate Y

```css
.box {
    transform: rotateY(60deg);
}
```

## Rotate Z

```css
.box {
    transform: rotateZ(45deg);
}
```

`rotateZ()` behaves similarly to a normal 2D rotation.

---

# 11. 3D Translate

Move an element on the Z axis:

```css
.box {
    transform: translateZ(100px);
}
```

Full 3D translation:

```css
.box {
    transform:
        translate3d(
            50px,
            20px,
            100px
        );
}
```

The values represent:

```text
X
Y
Z
```

---

# 12. Perspective

Perspective creates the visual effect of depth.

Example:

```css
.scene {
    perspective: 1000px;
}
```

A smaller value creates a stronger perspective effect:

```css
perspective: 500px;
```

A larger value creates a more subtle effect:

```css
perspective: 2000px;
```

Example:

```html
<div class="scene">
    <div class="box">
        Hello
    </div>
</div>
```

```css
.scene {
    perspective: 800px;
}

.box {
    transform: rotateY(45deg);
}
```

---

# 13. Backface Visibility

`backface-visibility` controls whether the back side of a transformed element is visible.

Values:

```css
visible
hidden
```

Example:

```css
.card {
    backface-visibility: hidden;
}
```

This is especially important for flip cards.

---

# 14. Preserve 3D

`transform-style` controls whether an element's children remain in 3D space.

```css
.card {
    transform-style: preserve-3d;
}
```

This is useful when creating 3D objects with multiple transformed children.

---

# 15. 3D Flip Card

A common real-world example is a product card that flips.

HTML:

```html
<div class="scene">

    <div class="card">

        <div class="card-front">
            Product
        </div>

        <div class="card-back">
            Details
        </div>

    </div>

</div>
```

CSS:

```css
.scene {
    width: 300px;
    height: 400px;

    perspective: 1000px;
}

.card {
    width: 100%;
    height: 100%;

    position: relative;

    transform-style: preserve-3d;

    transition: transform 0.7s ease;
}

.scene:hover .card {
    transform: rotateY(180deg);
}

.card-front,
.card-back {
    position: absolute;
    inset: 0;

    display: flex;
    align-items: center;
    justify-content: center;

    backface-visibility: hidden;
}

.card-back {
    transform: rotateY(180deg);
}
```

Structure:

```text
Scene
  ↓
Perspective
  ↓
Card
  ↓
Front + Back
```

---

# 16. CSS Animation

CSS animations allow elements to change between different styles over time.

Animations use:

```css
@keyframes
```

and:

```css
animation
```

---

# 17. @keyframes

`@keyframes` defines the stages of an animation.

Example:

```css
@keyframes move {
    from {
        transform: translateX(0);
    }

    to {
        transform: translateX(200px);
    }
}
```

Apply it:

```css
.box {
    animation: move 2s;
}
```

---

## Keyframes With Percentages

```css
@keyframes move {
    0% {
        transform: translateX(0);
    }

    50% {
        transform: translateX(200px);
    }

    100% {
        transform: translateX(0);
    }
}
```

---

# 18. Animation Properties

## Animation Name

```css
.box {
    animation-name: move;
}
```

## Animation Duration

```css
.box {
    animation-duration: 2s;
}
```

Common units:

```css
500ms
1s
2s
```

## Animation Iteration Count

One time:

```css
animation-iteration-count: 1;
```

Three times:

```css
animation-iteration-count: 3;
```

Infinite:

```css
animation-iteration-count: infinite;
```

---

## Animation Timing Function

Controls the speed curve.

Common values:

```css
linear
ease
ease-in
ease-out
ease-in-out
```

Example:

```css
.box {
    animation-timing-function: ease-in-out;
}
```

---

# 19. Animation Direction

Controls the direction of an animation.

Values:

```css
normal
reverse
alternate
alternate-reverse
```

## Normal

```css
animation-direction: normal;
```

Runs:

```text
0 → 100
```

## Reverse

```css
animation-direction: reverse;
```

Runs:

```text
100 → 0
```

## Alternate

```css
animation-direction: alternate;
```

Runs:

```text
0 → 100 → 0 → 100
```

---

# 20. Animation Delay

Controls how long the browser waits before starting the animation.

```css
.box {
    animation-delay: 1s;
}
```

Example:

```css
.box {
    animation-name: move;
    animation-duration: 2s;
    animation-delay: 1s;
}
```

---

# 21. Animation Fill Mode

`animation-fill-mode` controls styles before and after an animation.

Values:

```css
none
forwards
backwards
both
```

## None

```css
animation-fill-mode: none;
```

Default behavior.

## Forwards

```css
animation-fill-mode: forwards;
```

Keeps the styles from the final keyframe after the animation ends.

## Backwards

```css
animation-fill-mode: backwards;
```

Applies the first relevant keyframe during the animation delay.

## Both

```css
animation-fill-mode: both;
```

Combines `backwards` and `forwards`.

---

# 22. Animation Play State

Controls whether an animation is running or paused.

Values:

```css
running
paused
```

Example:

```css
.box {
    animation: move 2s infinite;
}

.box:hover {
    animation-play-state: paused;
}
```

---

# 23. Animation Shorthand

Instead of:

```css
.box {
    animation-name: move;
    animation-duration: 2s;
    animation-timing-function: ease;
    animation-delay: 1s;
    animation-iteration-count: infinite;
    animation-direction: alternate;
    animation-fill-mode: both;
}
```

You can use:

```css
.box {
    animation:
        move
        2s
        ease
        1s
        infinite
        alternate
        both;
}
```

Understand the individual properties before relying on shorthand.

---

# 24. Up and Down Animation

A common UI animation:

```css
@keyframes up-down {

    0% {
        transform: translateY(0);
    }

    50% {
        transform: translateY(-20px);
    }

    100% {
        transform: translateY(0);
    }

}

.icon {
    animation:
        up-down 1.5s ease-in-out infinite;
}
```

---

# 25. Loading Animation

```css
@keyframes loading {

    0% {
        transform: rotate(0deg);
    }

    100% {
        transform: rotate(360deg);
    }

}

.loader {
    width: 50px;
    height: 50px;

    border: 5px solid #ddd;
    border-top-color: blue;

    border-radius: 50%;

    animation:
        loading 1s linear infinite;
}
```

---

# 26. Fade In Animation

```css
@keyframes fade-in {

    from {
        opacity: 0;
    }

    to {
        opacity: 1;
    }

}

.title {
    animation:
        fade-in 1s ease;
}
```

---

# 27. Slide In Animation

```css
@keyframes slide-in {

    from {
        opacity: 0;
        transform: translateX(-50px);
    }

    to {
        opacity: 1;
        transform: translateX(0);
    }

}

.card {
    animation:
        slide-in 0.7s ease;
}
```

---

# 28. Animation Training

## Exercise 1

Create a ball that:

- Moves up
- Moves down
- Repeats forever

Use:

```css
@keyframes
translateY()
infinite
alternate
```

## Exercise 2

Create a loading spinner.

Requirements:

- Circle
- Border
- One different border color
- Continuous rotation

Use:

```css
rotate()
animation
linear
infinite
```

## Exercise 3

Create a button that:

- Scales slightly
- Rotates
- Returns to the original position

Use:

```css
@keyframes
scale()
rotate()
```

---

# 29. CSS Selector Reference

Selectors determine which HTML elements CSS will style.

## Element Selector

```css
p {
    color: red;
}
```

## Class Selector

```css
.card {
    padding: 20px;
}
```

## ID Selector

```css
#header {
    background: black;
}
```

IDs should generally be unique in a document.

## Universal Selector

```css
* {
    box-sizing: border-box;
}
```

## Group Selector

```css
h1,
h2,
h3 {
    font-family: Arial;
}
```

---

# 30. Relationship Selectors

## Descendant Selector

```css
.card p {
    color: gray;
}
```

Selects all paragraphs anywhere inside `.card`.

## Child Selector

```css
.card > p {
    color: red;
}
```

Selects only direct child paragraphs.

## Adjacent Sibling

```css
h2 + p {
    margin-top: 0;
}
```

Selects the first `<p>` immediately after an `<h2>`.

## General Sibling

```css
h2 ~ p {
    color: gray;
}
```

Selects paragraphs that are later siblings of the heading.

---

# 31. Attribute Selector

Example:

```css
input[type="text"] {
    border: 1px solid gray;
}
```

Select elements that have an attribute:

```css
input[required] {
    border-color: red;
}
```

---

# 32. Pseudo-Class Selector

Example:

```css
button:hover {
    background: black;
}
```

Other common pseudo-classes:

```css
:hover
:focus
:active
:visited
:first-child
:last-child
:nth-child()
:checked
:disabled
```

---

# 33. Pseudo-Element Selector

Pseudo-elements style a specific part of an element.

Example:

```css
p::first-letter {
    font-size: 40px;
}
```

Common pseudo-elements:

```css
::before
::after
::first-letter
::first-line
::selection
```

Example:

```css
.card::before {
    content: "";
}
```

`::before` and `::after` normally require the `content` property.

---

# 34. Responsive Design

Responsive design means creating websites that work well on different screen sizes.

A responsive website should work on:

- Mobile
- Tablet
- Laptop
- Desktop
- Large screens

The layout should adapt to the available space.

---

# 35. Viewport Meta Tag

Responsive websites should include:

```html
<meta
    name="viewport"
    content="width=device-width, initial-scale=1.0"
>
```

Place it inside `<head>`.

---

# 36. Media Queries

Media queries allow CSS rules to apply only under certain conditions.

Basic syntax:

```css
@media (max-width: 768px) {

    .container {
        padding: 10px;
    }

}
```

---

# 37. Max Width

Apply styles when the viewport is at or below a specific width.

```css
@media (max-width: 768px) {

    .menu {
        display: none;
    }

}
```

---

# 38. Min Width

Apply styles when the viewport is at or above a specific width.

```css
@media (min-width: 768px) {

    .container {
        max-width: 1200px;
    }

}
```

---

# 39. Responsive Navigation Example

Desktop:

```text
Logo     Home   About   Services   Login
```

Mobile:

```text
Logo                         Menu
```

CSS:

```css
.nav {
    display: flex;
    align-items: center;
    justify-content: space-between;
}

.menu {
    display: flex;
    gap: 20px;
}

@media (max-width: 768px) {

    .menu {
        display: none;
    }

}
```

---

# 40. Mobile-First Design

A common modern approach is mobile-first development.

Start with the smallest layout:

```css
.card {
    width: 100%;
}
```

Then improve the layout for larger screens:

```css
@media (min-width: 768px) {

    .card {
        width: 50%;
    }

}
```

And:

```css
@media (min-width: 1200px) {

    .card {
        width: 33.333%;
    }

}
```

---

# 41. Responsive Grid

CSS Grid can often handle responsiveness automatically.

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

This allows the grid to adapt to different screen sizes.

---

# 42. Responsive Design Standards

## Layout

Avoid fixed widths when they are not necessary.

Useful responsive values and functions include:

```css
%
max-width
min-width
minmax()
fr
rem
em
vw
vh
clamp()
```

## Images

```css
img {
    max-width: 100%;
    height: auto;
}
```

## Text

Use scalable units when appropriate:

```css
font-size: 1rem;
```

For fluid typography:

```css
font-size: clamp(1.5rem, 4vw, 3rem);
```

## Accessibility

Make sure:

- Text is readable
- Contrast is sufficient
- Keyboard navigation works
- Focus states are visible
- Content is not hidden unnecessarily
- Interactive controls are easy to use on touch devices

---

# 43. Inherit

`inherit` tells a property to take the value from its parent.

Example:

```css
.parent {
    color: red;
}

.child {
    color: inherit;
}
```

The child becomes red.

---

# 44. Unset

`unset` depends on whether the property normally inherits.

For an inherited property, it behaves like:

```css
inherit
```

For a non-inherited property, it behaves like:

```css
initial
```

Example:

```css
.child {
    color: unset;
}
```

---

# 45. Initial

`initial` resets a property to its initial CSS value.

Example:

```css
.box {
    display: initial;
}
```

Always check the property's initial value because it is not necessarily the browser's default styling.

---

# 46. Revert

`revert` rolls a property back to a previous cascade origin, often allowing browser/user stylesheet behavior to return.

Example:

```css
button {
    all: revert;
}
```

This can be useful when removing custom styling and restoring more native behavior.

---

# 47. Inherit vs Unset vs Initial vs Revert

| Value | Meaning |
|---|---|
| `inherit` | Use the parent's value |
| `unset` | Inherit if the property is inherited; otherwise use initial |
| `initial` | Use the property's initial CSS value |
| `revert` | Roll back to a previous cascade origin |

Example:

```css
.child {
    color: inherit;
}

.child {
    color: unset;
}

.child {
    color: initial;
}

.child {
    color: revert;
}
```

These values are different and should not be treated as interchangeable.

---

# 48. CSS Battle

CSS Battle is a challenge platform where you reproduce visual targets using CSS.

Website:

https://cssbattle.dev/

It is useful for practicing:

- CSS positioning
- Shapes
- Borders
- Gradients
- Transform
- Pseudo-elements
- Creative CSS
- Problem solving

---

# 49. Practice Resources

## Frontend Mentor

https://www.frontendmentor.io/

Useful for practicing real-world frontend projects.

You can practice:

- HTML
- CSS
- Responsive design
- Layout
- UI implementation

## Elzero Web School

https://elzero.org/

A large collection of Arabic programming and frontend learning resources.

## CodePen

https://codepen.io/

Useful for:

- Testing CSS
- Creating small experiments
- Sharing frontend examples
- Practicing animations
- Building UI components

---

# 50. Practice Challenge

Create a responsive landing page.

Structure:

```text
--------------------------------
             Navbar
--------------------------------

        Hero Section

    Title
    Description
    Button

--------------------------------

        Feature Cards

   Card    Card    Card

--------------------------------

           Footer
--------------------------------
```

Requirements:

### Navbar

Use:

```css
display: flex;
```

### Hero

Use:

```css
linear-gradient()
```

### Button

Use:

```css
:hover
transform
transition
```

### Cards

Use:

```css
display: grid;
```

### Card Hover

Use:

```css
transform: translateY()
box-shadow
```

### Animation

Animate the hero content using:

```css
@keyframes
```

### Responsive

Use:

```css
@media
```

### Mobile

The cards should use fewer columns on small screens.

---

# 51. Final Project

## Animated Responsive Product Page

Create a professional product page.

### Header

Include:

- Logo
- Navigation
- Button

Use Flexbox.

### Hero

Include:

- Product image
- Product title
- Description
- CTA button

Use:

- Gradient
- Transform
- Animation

### Product Card

Create a 3D flip card.

Front:

```text
Product Image
Product Name
Price
```

Back:

```text
Product Details
Rating
Buy Now
```

Use:

```css
perspective
transform-style
rotateY
backface-visibility
```

### Product Gallery

Use CSS Grid:

```css
grid-template-columns:
    repeat(
        auto-fill,
        minmax(220px, 1fr)
    );
```

### Animations

Add:

- Fade in
- Slide in
- Hover scale
- Floating animation

### Responsive Design

The page must work on:

```text
Mobile
Tablet
Desktop
```

---

# 52. Lesson Checklist

## Transform

- [ ] `transform`
- [ ] `scale()`
- [ ] `rotate()`
- [ ] `deg`
- [ ] `turn`
- [ ] `translate()`
- [ ] `skew()`
- [ ] `matrix()`
- [ ] `transform-origin`

## 3D

- [ ] `rotateX()`
- [ ] `rotateY()`
- [ ] `rotateZ()`
- [ ] `translate3d()`
- [ ] `translateZ()`
- [ ] `perspective`
- [ ] `backface-visibility`
- [ ] `transform-style`
- [ ] `preserve-3d`

## Animation

- [ ] `@keyframes`
- [ ] `animation-name`
- [ ] `animation-duration`
- [ ] `animation-iteration-count`
- [ ] `animation-timing-function`
- [ ] `animation-direction`
- [ ] `animation-delay`
- [ ] `animation-fill-mode`
- [ ] `animation-play-state`
- [ ] Animation shorthand

## Selectors

- [ ] Element selector
- [ ] Class selector
- [ ] ID selector
- [ ] Universal selector
- [ ] Group selector
- [ ] Descendant selector
- [ ] Child selector
- [ ] Adjacent sibling
- [ ] General sibling
- [ ] Attribute selector
- [ ] Pseudo-class
- [ ] Pseudo-element

## Responsive Design

- [ ] Viewport meta tag
- [ ] Media queries
- [ ] `min-width`
- [ ] `max-width`
- [ ] Mobile-first design
- [ ] Responsive Grid
- [ ] Responsive Flexbox

## CSS Values

- [ ] `inherit`
- [ ] `unset`
- [ ] `initial`
- [ ] `revert`

---

# 53. Summary

In Lesson 4, you learned how to create more advanced CSS interfaces.

You learned:

- 2D transformations
- Scale
- Rotate
- Translate
- Skew
- Matrix
- Transform origin
- 3D transformations
- 3D rotation
- 3D translation
- Perspective
- Backface visibility
- Preserve 3D
- 3D flip cards
- CSS animations
- Keyframes
- Animation duration
- Animation iteration
- Timing functions
- Animation direction
- Animation delay
- Fill mode
- Play state
- CSS selector types
- Media queries
- Responsive design
- Mobile-first development
- `inherit`
- `unset`
- `initial`
- `revert`

---

# Final Goal

After completing Lessons 1–4, you should be able to build a responsive website using:

```text
HTML
  ↓
CSS Basics
  ↓
Typography
  ↓
Flexbox
  ↓
Grid
  ↓
Responsive Design
  ↓
Transforms
  ↓
Animations 
  ↓
Advanced CSS
```

The most important goal is not memorizing every CSS property.

The goal is to understand:

> **What problem does this CSS property solve, when should I use it, and why?**

# Session 7: Transitions, Transforms, Animations, Filters, Gradients

## Duration Breakdown
- **1 Hour**: Theoretical Explanation + Live Coding
- **1.5 Hours**: Practical Application (Student writes code)
- **0.5 Hours**: Review, Questions, Problem Solving

---

## Part 1: Theoretical Explanation + Live Coding (1 Hour)

### Section 1: CSS Transitions

## What are Transitions
Transitions allow you to change property values smoothly over a given duration.

## Basic Syntax
```css
.element {
    transition: property duration timing-function delay;
}
```

## Transition Properties
```css
.button {
    background-color: #2563eb;
    transition: background-color 0.3s ease;
}

.button:hover {
    background-color: #1d4ed8;
}
```

## Multiple Properties
```css
.button {
    background-color: #2563eb;
    transform: scale(1);
    transition: 
        background-color 0.3s ease,
        transform 0.3s ease;
}

.button:hover {
    background-color: #1d4ed8;
    transform: scale(1.05);
}
```

## All Properties
```css
.element {
    transition: all 0.3s ease;
}
```

## Timing Functions
```css
.linear {
    transition-timing-function: linear;
}

.ease {
    transition-timing-function: ease; /* Default */
}

.ease-in {
    transition-timing-function: ease-in;
}

.ease-out {
    transition-timing-function: ease-out;
}

.ease-in-out {
    transition-timing-function: ease-in-out;
}

.cubic-bezier {
    transition-timing-function: cubic-bezier(0.4, 0, 0.2, 1);
}
```

## Transition Delay
```css
.element {
    transition: all 0.3s ease 0.5s; /* 0.5s delay */
}
```

---

### Section 2: 2D Transforms

## What are Transforms
Transforms allow you to visually transform elements without changing the normal document flow.

## Scale
```css
.scale-up {
    transform: scale(1.2); /* 120% size */
}

.scale-down {
    transform: scale(0.8); /* 80% size */
}

.scale-x {
    transform: scaleX(1.5); /* Horizontal scale */
}

.scale-y {
    transform: scaleY(0.5); /* Vertical scale */
}
```

## Rotate
```css
.rotate {
    transform: rotate(45deg); /* Rotate 45 degrees */
}

.rotate-negative {
    transform: rotate(-45deg); /* Rotate -45 degrees */
}
```

## Translate
```css
.translate {
    transform: translate(50px, 20px); /* X: 50px, Y: 20px */
}

.translate-x {
    transform: translateX(100px); /* Horizontal only */
}

.translate-y {
    transform: translateY(50px); /* Vertical only */
}
```

## Skew
```css
.skew-x {
    transform: skewX(20deg); /* Horizontal skew */
}

.skew-y {
    transform: skewY(10deg); /* Vertical skew */
}

.skew-both {
    transform: skew(10deg, 5deg); /* Both axes */
}
```

## Combining Transforms
```css
.combined {
    transform: translate(50px, 20px) rotate(45deg) scale(1.2);
}
```

**Important:** The order of transforms matters!

---

### Section 3: 3D Transforms

## 3D Rotate
```css
.rotate-x {
    transform: rotateX(45deg);
}

.rotate-y {
    transform: rotateY(45deg);
}

.rotate-z {
    transform: rotateZ(45deg);
}
```

## 3D Translate
```css
.translate-z {
    transform: translateZ(50px);
}
```

## Perspective
```css
.container {
    perspective: 1000px; /* Creates 3D space */
}

.element {
    transform: rotateY(45deg);
}
```

## Backface Visibility
```css
.element {
    backface-visibility: hidden; /* Hide back of element */
}
```

## Preserve 3D
```css
.container {
    transform-style: preserve-3d; /* Preserve 3D for children */
}
```

---

### Section 4: 3D Flip Card

## Basic Flip Card Structure
```css
.card-container {
    perspective: 1000px;
}

.card {
    position: relative;
    transform-style: preserve-3d;
    transition: transform 0.6s;
}

.card:hover {
    transform: rotateY(180deg);
}

.card-front, .card-back {
    position: absolute;
    backface-visibility: hidden;
}

.card-back {
    transform: rotateY(180deg);
}
```

---

### Section 5: CSS Filters

## Common Filters
```css
.grayscale {
    filter: grayscale(100%); /* Black and white */
}

.blur {
    filter: blur(5px); /* Blur effect */
}

.brightness {
    filter: brightness(150%); /* Brighter */
}

.contrast {
    filter: contrast(150%); /* More contrast */
}

.saturate {
    filter: saturate(200%); /* More saturated */
}

.sepia {
    filter: sepia(100%); /* Sepia tone */
}

.invert {
    filter: invert(100%); /* Inverted colors */
}

.hue-rotate {
    filter: hue-rotate(90deg); /* Rotate colors */
}

.drop-shadow {
    filter: drop-shadow(5px 5px 10px rgba(0, 0, 0, 0.5));
}
```

## Combining Filters
```css
.combined {
    filter: grayscale(100%) blur(2px) brightness(80%);
}
```

---

### Section 6: Gradients

## Linear Gradient
```css
.linear {
    background: linear-gradient(to right, blue, purple);
}

.linear-diagonal {
    background: linear-gradient(45deg, blue, purple);
}
```

## Radial Gradient
```css
.radial {
    background: radial-gradient(circle, blue, purple);
}

.radial-ellipse {
    background: radial-gradient(ellipse, blue, purple);
}
```

## Conic Gradient
```css
.conic {
    background: conic-gradient(blue, purple, blue);
}
```

## Multiple Colors
```css
.multi-color {
    background: linear-gradient(to right, red, yellow, green, blue);
}
```

## Color Stops
```css
.color-stops {
    background: linear-gradient(to right, blue 0%, purple 50%, pink 100%);
}
```

---

## Part 2: Practical Application (1.5 Hours)

### Exercise 1: CSS Transitions Practice (20 minutes)

**Task:**
Create interactive elements with various transitions.

**HTML:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>CSS Transitions</title>
    <link rel="stylesheet" href="transitions.css">
</head>
<body>
    <h1>CSS Transitions</h1>
    
    <h2>Color Transition</h2>
    <button class="color-button">Hover Me</button>
    
    <h2>Transform Transition</h2>
    <button class="transform-button">Scale Me</button>
    
    <h2>Multiple Property Transition</h2>
    <button class="multi-button">Complex Hover</button>
    
    <h2>Delayed Transition</h2>
    <button class="delay-button">Delayed Effect</button>
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
    color: #1f2937;
    margin-bottom: 30px;
}

h2 {
    color: #1f2937;
    margin-top: 30px;
    margin-bottom: 15px;
}

button {
    padding: 15px 30px;
    font-size: 16px;
    font-weight: 600;
    border: none;
    border-radius: 8px;
    cursor: pointer;
    margin: 10px;
}

/* Color Transition */
.color-button {
    background-color: #2563eb;
    color: white;
    transition: background-color 0.3s ease;
}

.color-button:hover {
    background-color: #1d4ed8;
}

/* Transform Transition */
.transform-button {
    background-color: #10b981;
    color: white;
    transition: transform 0.3s ease;
}

.transform-button:hover {
    transform: scale(1.1);
}

/* Multiple Property Transition */
.multi-button {
    background-color: #f59e0b;
    color: white;
    transition: 
        background-color 0.3s ease,
        transform 0.3s ease,
        box-shadow 0.3s ease;
}

.multi-button:hover {
    background-color: #d97706;
    transform: translateY(-3px);
    box-shadow: 0 6px 12px rgba(245, 158, 11, 0.3);
}

/* Delayed Transition */
.delay-button {
    background-color: #ef4444;
    color: white;
    transition: all 0.3s ease 0.5s;
}

.delay-button:hover {
    background-color: #dc2626;
    transform: rotate(5deg);
}
```

---

### Exercise 2: 2D Transforms Practice (20 minutes)

**Task:**
Create elements demonstrating various 2D transforms.

**HTML:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>2D Transforms</title>
    <link rel="stylesheet" href="transforms.css">
</head>
<body>
    <h1>2D Transforms</h1>
    
    <h2>Scale</h2>
    <div class="transform-container">
        <div class="box scale-up">Scale Up</div>
        <div class="box scale-down">Scale Down</div>
    </div>
    
    <h2>Rotate</h2>
    <div class="transform-container">
        <div class="box rotate">Rotate 45°</div>
        <div class="box rotate-negative">Rotate -45°</div>
    </div>
    
    <h2>Translate</h2>
    <div class="transform-container">
        <div class="box translate">Translate</div>
        <div class="box translate-x">Translate X</div>
    </div>
    
    <h2>Skew</h2>
    <div class="transform-container">
        <div class="box skew-x">Skew X</div>
        <div class="box skew-y">Skew Y</div>
    </div>
    
    <h2>Combined</h2>
    <div class="transform-container">
        <div class="box combined">Combined Transform</div>
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
    color: #1f2937;
    margin-bottom: 30px;
}

h2 {
    color: #1f2937;
    margin-top: 30px;
    margin-bottom: 15px;
}

.transform-container {
    display: flex;
    gap: 20px;
    margin-bottom: 20px;
    justify-content: center;
}

.box {
    width: 150px;
    height: 150px;
    background-color: #2563eb;
    color: white;
    display: flex;
    align-items: center;
    justify-content: center;
    border-radius: 8px;
    font-weight: bold;
    transition: transform 0.3s ease;
}

/* Scale */
.scale-up:hover {
    transform: scale(1.2);
}

.scale-down:hover {
    transform: scale(0.8);
}

/* Rotate */
.rotate:hover {
    transform: rotate(45deg);
}

.rotate-negative:hover {
    transform: rotate(-45deg);
}

/* Translate */
.translate:hover {
    transform: translate(20px, 10px);
}

.translate-x:hover {
    transform: translateX(30px);
}

/* Skew */
.skew-x:hover {
    transform: skewX(20deg);
}

.skew-y:hover {
    transform: skewY(10deg);
}

/* Combined */
.combined:hover {
    transform: translate(10px, 10px) rotate(15deg) scale(1.1);
}
```

---

### Exercise 3: CSS Animations - @keyframes (25 minutes)

**Task:**
Create elements using CSS animations with @keyframes.

**HTML:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>CSS Animations</title>
    <link rel="stylesheet" href="animations.css">
</head>
<body>
    <h1>CSS Animations</h1>
    
    <h2>Loading Animation</h2>
    <div class="loading-spinner"></div>
    
    <h2>Fade In Animation</h2>
    <div class="fade-in-box">Fade In</div>
    
    <h2>Slide In Animation</h2>
    <div class="slide-in-box">Slide In</div>
    
    <h2>Bounce Animation</h2>
    <div class="bounce-box">Bounce</div>
    
    <h2>Pulse Animation</h2>
    <div class="pulse-box">Pulse</div>
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
    color: #1f2937;
    margin-bottom: 30px;
}

h2 {
    color: #1f2937;
    margin-top: 30px;
    margin-bottom: 15px;
}

/* Loading Spinner */
@keyframes spin {
    0% { transform: rotate(0deg); }
    100% { transform: rotate(360deg); }
}

.loading-spinner {
    width: 50px;
    height: 50px;
    border: 5px solid #e5e7eb;
    border-top-color: #2563eb;
    border-radius: 50%;
    animation: spin 1s linear infinite;
    margin: 20px auto;
}

/* Fade In */
@keyframes fadeIn {
    from {
        opacity: 0;
        transform: translateY(20px);
    }
    to {
        opacity: 1;
        transform: translateY(0);
    }
}

.fade-in-box {
    width: 200px;
    height: 100px;
    background-color: #2563eb;
    color: white;
    display: flex;
    align-items: center;
    justify-content: center;
    border-radius: 8px;
    margin: 20px auto;
    animation: fadeIn 1s ease-out;
}

/* Slide In */
@keyframes slideIn {
    from {
        transform: translateX(-100%);
        opacity: 0;
    }
    to {
        transform: translateX(0);
        opacity: 1;
    }
}

.slide-in-box {
    width: 200px;
    height: 100px;
    background-color: #10b981;
    color: white;
    display: flex;
    align-items: center;
    justify-content: center;
    border-radius: 8px;
    margin: 20px auto;
    animation: slideIn 1s ease-out;
}

/* Bounce */
@keyframes bounce {
    0%, 20%, 50%, 80%, 100% {
        transform: translateY(0);
    }
    40% {
        transform: translateY(-30px);
    }
    60% {
        transform: translateY(-15px);
    }
}

.bounce-box {
    width: 100px;
    height: 100px;
    background-color: #f59e0b;
    color: white;
    display: flex;
    align-items: center;
    justify-content: center;
    border-radius: 50%;
    margin: 20px auto;
    animation: bounce 2s infinite;
}

/* Pulse */
@keyframes pulse {
    0% {
        transform: scale(1);
        box-shadow: 0 0 0 0 rgba(37, 99, 235, 0.7);
    }
    70% {
        transform: scale(1.1);
        box-shadow: 0 0 0 20px rgba(37, 99, 235, 0);
    }
    100% {
        transform: scale(1);
        box-shadow: 0 0 0 0 rgba(37, 99, 235, 0);
    }
}

.pulse-box {
    width: 150px;
    height: 150px;
    background-color: #2563eb;
    color: white;
    display: flex;
    align-items: center;
    justify-content: center;
    border-radius: 8px;
    margin: 20px auto;
    animation: pulse 2s infinite;
}
```

---

### Exercise 4: 3D Flip Card (25 minutes)

**Task:**
Create a 3D flip card with front and back faces.

**HTML:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>3D Flip Card</title>
    <link rel="stylesheet" href="flip-card.css">
</head>
<body>
    <h1>3D Flip Card</h1>
    
    <div class="card-container">
        <div class="card">
            <div class="card-front">
                <h2>Front Side</h2>
                <p>Hover to flip</p>
                <div class="icon">🎨</div>
            </div>
            <div class="card-back">
                <h2>Back Side</h2>
                <p>This is the back of the card</p>
                <button>Learn More</button>
            </div>
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
    min-height: 100vh;
    display: flex;
    flex-direction: column;
    align-items: center;
}

h1 {
    color: #1f2937;
    margin-bottom: 40px;
}

.card-container {
    perspective: 1000px;
    margin-bottom: 40px;
}

.card {
    width: 300px;
    height: 400px;
    position: relative;
    transform-style: preserve-3d;
    transition: transform 0.8s;
    cursor: pointer;
}

.card:hover {
    transform: rotateY(180deg);
}

.card-front, .card-back {
    position: absolute;
    width: 100%;
    height: 100%;
    backface-visibility: hidden;
    border-radius: 16px;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    padding: 30px;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

.card-front {
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    color: white;
}

.card-back {
    background: linear-gradient(135deg, #f093fb 0%, #f5576c 100%);
    color: white;
    transform: rotateY(180deg);
}

.card-front h2, .card-back h2 {
    font-size: 24px;
    margin-bottom: 15px;
}

.card-front p, .card-back p {
    font-size: 16px;
    margin-bottom: 20px;
    text-align: center;
}

.icon {
    font-size: 48px;
    margin-top: 20px;
}

.card-back button {
    padding: 12px 24px;
    background-color: white;
    color: #f5576c;
    border: none;
    border-radius: 8px;
    font-size: 16px;
    font-weight: 600;
    cursor: pointer;
    transition: transform 0.3s;
}

.card-back button:hover {
    transform: scale(1.05);
}
```

---

## Part 3: Review, Questions, Problem Solving (0.5 Hours)

### Animation Properties Summary (10 minutes)

**Essential Animation Properties:**
```css
.element {
    animation-name: myAnimation;
    animation-duration: 2s;
    animation-timing-function: ease;
    animation-delay: 0.5s;
    animation-iteration-count: infinite;
    animation-direction: alternate;
    animation-fill-mode: forwards;
    animation-play-state: running;
}
```

**Shorthand:**
```css
.element {
    animation: myAnimation 2s ease 0.5s infinite alternate forwards;
}
```

---

### Review Questions (10 minutes)

**Question 1:** What is the difference between transition and animation?
**Answer:** Transitions are for simple state changes, animations can create complex keyframe-based movements.

**Question 2:** What does `transform: scale(1.2)` do?
**Answer:** Scales the element to 120% of its original size.

**Question 3:** What is the purpose of `perspective` in 3D transforms?
**Answer:** Creates the 3D space depth effect by defining how far the element is from the viewer.

**Question 4:** What does `backface-visibility: hidden` do?
**Answer:** Hides the back face of an element when it's rotated away from the viewer.

**Question 5:** What is the difference between linear-gradient and radial-gradient?
**Answer:** Linear creates a straight gradient, radial creates a circular/elliptical gradient.

**Question 6:** What does `filter: grayscale(100%)` do?
**Answer:** Converts the element to black and white.

**Question 7:** What is the purpose of `@keyframes`?
**Answer:** Defines the animation sequence with key points and styles.

**Question 8:** What does `animation-fill-mode: forwards` do?
**Answer:** Keeps the element in the final animation state after the animation completes.

---

### Practice Challenges (5 minutes)

**Challenge 1:** Create a button that has a color transition and scale effect on hover.

**Challenge 2:** Create a div that rotates continuously using CSS animation.

**Challenge 3:** Create a card with a gradient background that has a filter effect on hover.

---

### Homework Assignment

**Task:** Create an animated component using transitions, transforms, and animations.

**Requirements:**
- Create an interactive card with hover transitions
- Implement a 3D flip card
- Use @keyframes for at least one animation
- Apply CSS filters for visual effects
- Use gradients for backgrounds
- Include smooth transitions for all interactive elements
- Make it responsive

**Due Date:** Next session

---

## End of Session 7

**Next Session:** Advanced Selectors + Responsive + Final Project (Relationship selectors, attribute selectors, media queries, mobile-first design, debugging, accessibility, final project implementation)
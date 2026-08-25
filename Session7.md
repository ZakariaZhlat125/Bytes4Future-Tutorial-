# Session 7: Transitions, Transforms, Animations, Filters, Gradients

## Duration Breakdown (2.5 Hours Total)
- **1.5 Hours**: Interactive Learning (All Sections 1-6 with Explain → Example → Try it yourself → Predict → Experiment → Challenge → Bug Hunting → Quiz)
- **20 Minutes**: Comprehensive Project (Single component combining all concepts)
- **10 Minutes**: Review & Assessment

---

## Part 1: Interactive Learning (1.5 Hours)

### Section 1: CSS Transitions

## Explain
CSS transitions allow you to change property values smoothly over a given duration:

- Syntax: `transition: property duration timing-function delay`
- Simple state changes (hover, focus, etc.)
- Multiple properties can be transitioned
- Timing functions control animation speed
- Delays can postpone transitions

## Example
```css
.button {
    background-color: #2563eb;
    transition: background-color 0.3s ease;
}

.button:hover {
    background-color: #1d4ed8;
}
```

## Try it yourself
Create elements with transitions.

```html
<button class="transition-button">Hover Me</button>
<div class="transition-box">Box</div>
```

```css
.transition-button {
    /* Add color transition */
}

.transition-box {
    /* Add transform transition */
}
```

## Predict
What will happen with this code?

```css
.element {
    transition: all 0.3s ease 0.5s;
}
```

## Experiment
1. What happens with different timing functions?
2. Try transitioning multiple properties
3. How does delay affect the transition?

## Challenge
**Task:** Create a button with:
- Color transition on hover
- Scale effect on hover
- Box shadow transition
- 0.5s delay before transition starts
- Time: 10 minutes
- Hint: Use multiple properties in transition

<details>
<summary>Solution</summary>

```html
<button class="challenge-button">Hover Me</button>
```

```css
.challenge-button {
    background: #2563eb;
    color: white;
    padding: 15px 30px;
    border: none;
    border-radius: 8px;
    font-size: 16px;
    cursor: pointer;
    transition: 
        background-color 0.3s ease 0.5s,
        transform 0.3s ease 0.5s,
        box-shadow 0.3s ease 0.5s;
}

.challenge-button:hover {
    background: #1d4ed8;
    transform: scale(1.1);
    box-shadow: 0 8px 16px rgba(37, 99, 235, 0.3);
}
```

</details>

## Bug Hunting
Find the bug in this code:

```css
.buggy {
    background: blue;
    transition: background 0.3s;
    /* No transition on hover */
}
```

<details>
<summary>Solution</summary>

Missing the hover state with a different background color. Transitions only work when property values change.

</details>

## Quiz
1. What is the transition syntax?
2. What does `ease` timing function do?
3. Can you transition multiple properties?

<details>
<summary>Quiz Answers</summary>

1. `transition: property duration timing-function delay`
2. `ease` starts slow, speeds up, then slows down (default timing function)
3. Yes, separate multiple properties with commas or use `transition: all`

</details>

---

### Section 2: 2D Transforms

## Explain
2D transforms visually transform elements without changing document flow:

- `scale()` - Resize element
- `rotate()` - Rotate element
- `translate()` - Move element
- `skew()` - Distort element
- Order of transforms matters!

## Example
```css
.transformed {
    transform: translate(50px, 20px) rotate(45deg) scale(1.2);
}
```

## Try it yourself
Create elements with 2D transforms.

```html
<div class="box scale-box">Scale</div>
<div class="box rotate-box">Rotate</div>
<div class="box translate-box">Translate</div>
```

```css
.scale-box:hover {
    /* Add scale transform */
}
```

## Predict
What will happen with this code?

```css
.element {
    transform: scale(2) rotate(45deg);
}
```

## Experiment
1. What happens when you change transform order?
2. Try combining multiple transforms
3. How does skew affect the element?

## Challenge
**Task:** Create a card with:
- Scale up on hover
- Rotate slightly on hover
- Translate up on hover
- Smooth transition
- Time: 10 minutes
- Hint: Combine transforms in the right order

<details>
<summary>Solution</summary>

```html
<div class="transform-card">
    <h3>Hover Me</h3>
</div>
```

```css
.transform-card {
    width: 200px;
    height: 150px;
    background: #2563eb;
    color: white;
    display: flex;
    align-items: center;
    justify-content: center;
    border-radius: 8px;
    transition: transform 0.3s ease;
}

.transform-card:hover {
    transform: translateY(-10px) rotate(5deg) scale(1.05);
}
```

</details>

## Bug Hunting
Find the bug in this code:

```css
.buggy {
    transform: scale 1.2;
    /* Transform not working */
}
```

<details>
<summary>Solution</summary>

Missing parentheses. Should be `transform: scale(1.2);`

</details>

## Quiz
1. What does `translate()` do?
2. Does transform affect document flow?
3. Why does transform order matter?

<details>
<summary>Quiz Answers</summary>

1. `translate()` moves an element along X and Y axes
2. No, transform doesn't affect document flow or other elements
3. Transforms are applied in sequence, so order affects the final result

</details>

---

### Section 3: 3D Transforms

## Explain
3D transforms add depth to transforms:

- `rotateX/Y/Z` - Rotate in 3D space
- `translateZ` - Move along Z axis
- `perspective` - Creates 3D depth
- `backface-visibility` - Hide/show back face
- `transform-style: preserve-3d` - Maintain 3D for children

## Example
```css
.container {
    perspective: 1000px;
}

.element {
    transform: rotateY(45deg);
}
```

## Try it yourself
Create 3D transforms.

```html
<div class="three-d-container">
    <div class="three-d-box">3D Box</div>
</div>
```

```css
.three-d-container {
    /* Add perspective */
}

.three-d-box {
    /* Add 3D rotate */
}
```

## Predict
What will happen with this code?

```css
.element {
    transform: rotateX(90deg);
}
```

## Experiment
1. What does perspective do?
2. Try different 3D rotations
3. How does backface-visibility work?

## Challenge
**Task:** Create a 3D card with:
- Perspective on container
- Rotate on hover
- Preserve 3D for children
- Time: 10 minutes
- Hint: Use `perspective` on parent, `rotateY` on child

<details>
<summary>Solution</summary>

```html
<div class="three-d-container">
    <div class="three-d-card">3D Card</div>
</div>
```

```css
.three-d-container {
    perspective: 1000px;
    width: 200px;
    height: 150px;
}

.three-d-card {
    width: 100%;
    height: 100%;
    background: linear-gradient(135deg, #667eea, #764ba2);
    color: white;
    display: flex;
    align-items: center;
    justify-content: center;
    border-radius: 8px;
    transition: transform 0.5s ease;
}

.three-d-card:hover {
    transform: rotateY(30deg) rotateX(10deg);
}
```

</details>

## Bug Hunting
Find the bug in this code:

```css
.buggy {
    transform: rotateY(45deg);
    /* No 3D effect visible */
}
```

<details>
<summary>Solution</summary>

Missing `perspective` on parent container. 3D transforms need perspective to show depth.

</details>

## Quiz
1. What does `perspective` do?
2. What's the difference between 2D and 3D transforms?
3. What does `backface-visibility: hidden` do?

<details>
<summary>Quiz Answers</summary>

1. `perspective` creates the illusion of 3D depth by defining how far the element is from the viewer
2. 2D transforms work on X/Y axes, 3D transforms add the Z axis for depth
3. Hides the back face of an element when it's rotated away from the viewer

</details>

---

### Section 4: 3D Flip Card

## Explain
3D flip cards have front and back faces that flip on hover:

- `perspective` on container for 3D depth
- `transform-style: preserve-3d` on card
- `backface-visibility: hidden` for both faces
- Back face rotated 180deg initially
- Transition on rotate transform

## Example
```css
.card-container {
    perspective: 1000px;
}

.card {
    transform-style: preserve-3d;
    transition: transform 0.6s;
}

.card:hover {
    transform: rotateY(180deg);
}

.card-front, .card-back {
    backface-visibility: hidden;
}

.card-back {
    transform: rotateY(180deg);
}
```

## Try it yourself
Create a flip card.

```html
<div class="flip-container">
    <div class="flip-card">
        <div class="flip-front">Front</div>
        <div class="flip-back">Back</div>
    </div>
</div>
```

```css
/* Add flip card styling */
```

## Predict
What will happen with this code?

```css
.card:hover {
    transform: rotateX(180deg);
}
```

## Experiment
1. What happens with different rotation axes?
2. Try rotating on X instead of Y
3. How does transition duration affect the flip?

## Challenge
**Task:** Create a flip card with:
- Front with gradient background
- Back with different color
- Icon on front
- Button on back
- Time: 12 minutes
- Hint: Use absolute positioning for front/back faces

<details>
<summary>Solution</summary>

```html
<div class="flip-container">
    <div class="flip-card">
        <div class="flip-front">
            <div class="icon">🎨</div>
            <h3>Front</h3>
        </div>
        <div class="flip-back">
            <h3>Back</h3>
            <p>Additional info</p>
            <button>Action</button>
        </div>
    </div>
</div>
```

```css
.flip-container {
    perspective: 1000px;
    width: 250px;
    height: 350px;
}

.flip-card {
    width: 100%;
    height: 100%;
    position: relative;
    transform-style: preserve-3d;
    transition: transform 0.8s;
}

.flip-card:hover {
    transform: rotateY(180deg);
}

.flip-front, .flip-back {
    position: absolute;
    width: 100%;
    height: 100%;
    backface-visibility: hidden;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    border-radius: 12px;
}

.flip-front {
    background: linear-gradient(135deg, #667eea, #764ba2);
    color: white;
}

.flip-back {
    background: linear-gradient(135deg, #f093fb, #f5576c);
    color: white;
    transform: rotateY(180deg);
}

.icon {
    font-size: 48px;
    margin-bottom: 15px;
}
```

</details>

## Bug Hunting
Find the bug in this code:

```css
.buggy {
    transform-style: preserve-3d;
    /* Back face visible during flip */
}
```

<details>
<summary>Solution</summary>

Missing `backface-visibility: hidden` on front and back faces.

</details>

## Quiz
1. What property creates 3D depth?
2. What does `backface-visibility: hidden` do?
3. Why use `transform-style: preserve-3d`?

<details>
<summary>Quiz Answers</summary>

1. `perspective` creates 3D depth on the container
2. Hides the back face when element is rotated away from viewer
3. `preserve-3d` maintains 3D positioning for child elements

</details>

---

### Section 5: CSS Filters

## Explain
CSS filters apply visual effects to elements:

- `grayscale()` - Black and white
- `blur()` - Blur effect
- `brightness()` - Brightness adjustment
- `contrast()` - Contrast adjustment
- `saturate()` - Saturation adjustment
- `sepia()` - Sepia tone
- `invert()` - Invert colors
- `hue-rotate()` - Rotate colors
- `drop-shadow()` - Shadow effect

## Example
```css
.filtered {
    filter: grayscale(100%) blur(2px);
}
```

## Try it yourself
Apply filters to elements.

```html
<div class="filter-box grayscale">Grayscale</div>
<div class="filter-box blur">Blur</div>
<div class="filter-box sepia">Sepia</div>
```

```css
.filter-box {
    /* Add basic styling */
}

.grayscale {
    /* Add grayscale filter */
}
```

## Predict
What will happen with this code?

```css
.element {
    filter: blur(10px);
}
```

## Experiment
1. What happens with multiple filters?
2. Try different filter values
3. How does drop-shadow differ from box-shadow?

## Challenge
**Task:** Create an image gallery with:
- Grayscale filter on hover
- Blur effect on active
- Brightness increase on focus
- Smooth transitions
- Time: 10 minutes
- Hint: Combine filters and transitions

<details>
<summary>Solution</summary>

```html
<div class="filter-gallery">
    <div class="filter-image">Image 1</div>
    <div class="filter-image">Image 2</div>
    <div class="filter-image">Image 3</div>
</div>
```

```css
.filter-gallery {
    display: flex;
    gap: 20px;
}

.filter-image {
    width: 150px;
    height: 150px;
    background: #2563eb;
    display: flex;
    align-items: center;
    justify-content: center;
    color: white;
    border-radius: 8px;
    transition: filter 0.3s ease;
}

.filter-image:hover {
    filter: grayscale(100%);
}

.filter-image:active {
    filter: blur(5px);
}

.filter-image:focus {
    filter: brightness(150%);
}
```

</details>

## Bug Hunting
Find the bug in this code:

```css
.buggy {
    filter: blur;
    /* Filter not working */
}
```

<details>
<summary>Solution</summary>

Missing value. Should be `filter: blur(5px);`

</details>

## Quiz
1. What does `grayscale(100%)` do?
2. How do you combine multiple filters?
3. What's the difference between drop-shadow and box-shadow?

<details>
<summary>Quiz Answers</summary>

1. Converts element to black and white
2. Separate filters with spaces: `filter: grayscale(100%) blur(2px)`
3. drop-shadow follows the element's shape (including transparent areas), box-shadow is rectangular

</details>

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
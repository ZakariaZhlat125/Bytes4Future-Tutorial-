# Session 6: CSS Grid

## Duration Breakdown
- **1 Hour**: Theoretical Explanation + Live Coding
- **1.5 Hours**: Practical Application (Student writes code)
- **0.5 Hours**: Review, Questions, Problem Solving

---

## Part 1: Theoretical Explanation + Live Coding (1 Hour)

### Section 1: CSS Grid Introduction

## What is CSS Grid
CSS Grid is a two-dimensional layout system that works with both rows and columns simultaneously.

## When to Use Grid
- Complex page layouts
- Dashboards
- Image galleries
- Product grids
- Two-dimensional layouts
- When you need control over both rows and columns

## Enable Grid
```css
.container {
    display: grid;
}
```

---

### Section 2: Grid Parent Properties

## Grid Template Columns
Defines the columns in a grid layout.

```css
.container {
    display: grid;
    grid-template-columns: 200px 200px 200px; /* Three 200px columns */
}
```

## Percentage Columns
```css
.container {
    grid-template-columns: 30% 30% 40%;
}
```

## Fraction Unit (fr)
`fr` means a fraction of the available space.

```css
.container {
    grid-template-columns: 1fr 1fr; /* Two equal columns */
}

.container {
    grid-template-columns: 1fr 2fr; /* Second column is twice as wide */
}
```

## repeat() Function
Instead of repeating values, use `repeat()`.

```css
.container {
    grid-template-columns: repeat(3, 1fr); /* Three equal columns */
}

.container {
    grid-template-columns: repeat(4, 200px); /* Four 200px columns */
}
```

Syntax: `repeat(number, value)`

---

## Grid Template Rows
Defines the rows in a grid layout.

```css
.container {
    grid-template-rows: 100px 200px; /* First row 100px, second 200px */
}

.container {
    grid-template-rows: auto 1fr auto; /* Auto, flexible, auto */
}
```

---

## Grid Gap
Adds space between grid items.

```css
.container {
    gap: 20px; /* Same gap for rows and columns */
}

.container {
    row-gap: 20px; /* Gap between rows */
    column-gap: 30px; /* Gap between columns */
}
```

---

## Grid Template Areas
Allows you to create named layout areas.

```css
.container {
    display: grid;
    grid-template-columns: 200px 1fr;
    grid-template-rows: auto 1fr auto;
    grid-template-areas:
        "header header"
        "sidebar main"
        "footer footer";
}
```

Then assign areas to children:
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

## Grid Alignment

### Justify Content
Controls the grid as a whole along the inline/horizontal axis.

```css
.container {
    justify-content: start;
    justify-content: end;
    justify-content: center;
    justify-content: space-between;
    justify-content: space-around;
    justify-content: space-evenly;
}
```

### Align Content
Controls the grid as a whole along the block/vertical axis.

```css
.container {
    align-content: start;
    align-content: end;
    align-content: center;
    align-content: space-between;
}
```

### Justify Items
Controls alignment of items along the inline axis within their grid cells.

```css
.container {
    justify-items: start;
    justify-items: end;
    justify-items: center;
}
```

### Align Items
Controls alignment of items along the block axis within their grid cells.

```css
.container {
    align-items: start;
    align-items: end;
    align-items: center;
    align-items: stretch;
}
```

---

### Section 3: Grid Child Properties

## Grid Column
Controls which columns an item occupies.

```css
.item {
    grid-column: 1 / 3; /* From column line 1 to line 3 */
}
```

## Span
```css
.item {
    grid-column: span 2; /* Spans 2 columns */
}
```

## Grid Row
Controls which rows an item occupies.

```css
.item {
    grid-row: 1 / 3; /* From row line 1 to line 3 */
}

.item {
    grid-row: span 2; /* Spans 2 rows */
}
```

## Grid Area
Can use named areas or grid line numbers.

```css
.item {
    grid-area: header; /* Named area */
}

.item {
    grid-area: 1 / 1 / 3 / 3; /* row-start / col-start / row-end / col-end */
}
```

## Justify Self
Overrides the parent's `justify-items` for one specific child.

```css
.item {
    justify-self: start;
    justify-self: center;
    justify-self: end;
}
```

## Align Self
Overrides the parent's `align-items` for one specific child.

```css
.item {
    align-self: start;
    align-self: center;
    align-self: end;
}
```

---

### Section 4: Responsive Grid Tools

## minmax()
Defines a minimum and maximum size for grid tracks.

```css
.container {
    grid-template-columns: minmax(200px, 1fr);
}
```

The column will be at least 200px and up to 1fr of available space.

## auto-fill
Creates as many columns as will fit.

```css
.container {
    grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
}
```

## auto-fit
Similar to auto-fill but can collapse empty tracks.

```css
.container {
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
}
```

---

## Part 2: Practical Application (1.5 Hours)

### Exercise 1: Basic Grid Layout (20 minutes)

**Task:**
Create a basic grid layout with defined columns and rows.

**HTML:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Basic Grid Layout</title>
    <link rel="stylesheet" href="basic-grid.css">
</head>
<body>
    <h1>Basic Grid Layout</h1>
    
    <div class="grid-container">
        <div class="item">Item 1</div>
        <div class="item">Item 2</div>
        <div class="item">Item 3</div>
        <div class="item">Item 4</div>
        <div class="item">Item 5</div>
        <div class="item">Item 6</div>
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

.grid-container {
    display: grid;
    grid-template-columns: repeat(3, 1fr); /* Three equal columns */
    grid-template-rows: repeat(2, 150px); /* Two rows of 150px */
    gap: 20px;
    max-width: 800px;
    margin: 0 auto;
}

.item {
    background-color: white;
    border: 2px solid #2563eb;
    border-radius: 8px;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 18px;
    font-weight: bold;
    color: #1f2937;
    transition: transform 0.3s, box-shadow 0.3s;
}

.item:hover {
    transform: translateY(-5px);
    box-shadow: 0 4px 8px rgba(37, 99, 235, 0.3);
}
```

---

### Exercise 2: Grid Template Areas (25 minutes)

**Task:**
Create a page layout using grid template areas.

**HTML:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Grid Template Areas</title>
    <link rel="stylesheet" href="grid-areas.css">
</head>
<body>
    <div class="layout">
        <header class="header">Header</header>
        <aside class="sidebar">Sidebar</aside>
        <main class="main">Main Content</main>
        <footer class="footer">Footer</footer>
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
    background-color: #f3f4f6;
    min-height: 100vh;
}

.layout {
    display: grid;
    min-height: 100vh;
    grid-template-columns: 250px 1fr;
    grid-template-rows: auto 1fr auto;
    grid-template-areas:
        "header header"
        "sidebar main"
        "footer footer";
    gap: 20px;
    padding: 20px;
}

.header {
    grid-area: header;
    background-color: #2563eb;
    color: white;
    padding: 20px;
    border-radius: 8px;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 24px;
    font-weight: bold;
}

.sidebar {
    grid-area: sidebar;
    background-color: white;
    padding: 20px;
    border-radius: 8px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.main {
    grid-area: main;
    background-color: white;
    padding: 20px;
    border-radius: 8px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.footer {
    grid-area: footer;
    background-color: #1f2937;
    color: white;
    padding: 20px;
    border-radius: 8px;
    text-align: center;
}
```

---

### Exercise 3: Product Gallery (25 minutes)

**Task:**
Create a responsive product gallery using Grid.

**HTML:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Product Gallery</title>
    <link rel="stylesheet" href="product-gallery.css">
</head>
<body>
    <h1>Product Gallery</h1>
    
    <div class="gallery">
        <div class="product">
            <div class="product-image">
                <img src="https://via.placeholder.com/200" alt="Product 1">
            </div>
            <h3>Product 1</h3>
            <p class="price">$29.99</p>
            <button>Add to Cart</button>
        </div>
        
        <div class="product">
            <div class="product-image">
                <img src="https://via.placeholder.com/200" alt="Product 2">
            </div>
            <h3>Product 2</h3>
            <p class="price">$49.99</p>
            <button>Add to Cart</button>
        </div>
        
        <div class="product">
            <div class="product-image">
                <img src="https://via.placeholder.com/200" alt="Product 3">
            </div>
            <h3>Product 3</h3>
            <p class="price">$19.99</p>
            <button>Add to Cart</button>
        </div>
        
        <div class="product">
            <div class="product-image">
                <img src="https://via.placeholder.com/200" alt="Product 4">
            </div>
            <h3>Product 4</h3>
            <p class="price">$39.99</p>
            <button>Add to Cart</button>
        </div>
        
        <div class="product">
            <div class="product-image">
                <img src="https://via.placeholder.com/200" alt="Product 5">
            </div>
            <h3>Product 5</h3>
            <p class="price">$59.99</p>
            <button>Add to Cart</button>
        </div>
        
        <div class="product">
            <div class="product-image">
                <img src="https://via.placeholder.com/200" alt="Product 6">
            </div>
            <h3>Product 6</h3>
            <p class="price">$24.99</p>
            <button>Add to Cart</button>
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
}

h1 {
    text-align: center;
    color: #1f2937;
    margin-bottom: 30px;
}

.gallery {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
    gap: 20px;
    max-width: 1200px;
    margin: 0 auto;
}

.product {
    background-color: white;
    border-radius: 12px;
    padding: 20px;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    transition: transform 0.3s, box-shadow 0.3s;
}

.product:hover {
    transform: translateY(-5px);
    box-shadow: 0 8px 12px rgba(0, 0, 0, 0.15);
}

.product-image {
    text-align: center;
    margin-bottom: 15px;
}

.product-image img {
    width: 100%;
    height: 200px;
    object-fit: cover;
    border-radius: 8px;
}

.product h3 {
    color: #1f2937;
    margin-bottom: 10px;
    font-size: 18px;
}

.price {
    color: #2563eb;
    font-size: 24px;
    font-weight: bold;
    margin-bottom: 15px;
}

.product button {
    width: 100%;
    padding: 12px;
    background-color: #2563eb;
    color: white;
    border: none;
    border-radius: 6px;
    font-size: 16px;
    font-weight: 600;
    cursor: pointer;
    transition: background-color 0.3s;
}

.product button:hover {
    background-color: #1d4ed8;
}
```

---

### Exercise 4: Dashboard Layout (20 minutes)

**Task:**
Create a dashboard layout using Grid.

**HTML:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Dashboard Layout</title>
    <link rel="stylesheet" href="dashboard.css">
</head>
<body>
    <div class="dashboard">
        <header class="dashboard-header">
            <h1>Dashboard</h1>
            <div class="user-info">User Profile</div>
        </header>
        
        <aside class="sidebar">
            <nav>
                <ul>
                    <li><a href="#overview">Overview</a></li>
                    <li><a href="#analytics">Analytics</a></li>
                    <li><a href="#reports">Reports</a></li>
                    <li><a href="#settings">Settings</a></li>
                </ul>
            </nav>
        </aside>
        
        <main class="main-content">
            <div class="stats-grid">
                <div class="stat-card">
                    <h3>Total Users</h3>
                    <p class="stat-number">1,234</p>
                </div>
                <div class="stat-card">
                    <h3>Revenue</h3>
                    <p class="stat-number">$45,678</p>
                </div>
                <div class="stat-card">
                    <h3>Orders</h3>
                    <p class="stat-number">789</p>
                </div>
                <div class="stat-card">
                    <h3>Growth</h3>
                    <p class="stat-number">+23%</p>
                </div>
            </div>
            
            <div class="content-area">
                <h2>Recent Activity</h2>
                <div class="activity-list">
                    <div class="activity-item">User John registered</div>
                    <div class="activity-item">Order #1234 completed</div>
                    <div class="activity-item">New review received</div>
                </div>
            </div>
        </main>
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
    background-color: #f3f4f6;
}

.dashboard {
    display: grid;
    min-height: 100vh;
    grid-template-columns: 250px 1fr;
    grid-template-rows: auto 1fr;
    grid-template-areas:
        "header header"
        "sidebar main";
    gap: 20px;
    padding: 20px;
}

.dashboard-header {
    grid-area: header;
    background-color: #2563eb;
    color: white;
    padding: 20px;
    border-radius: 8px;
    display: flex;
    justify-content: space-between;
    align-items: center;
}

.dashboard-header h1 {
    font-size: 24px;
}

.user-info {
    background-color: rgba(255, 255, 255, 0.2);
    padding: 8px 16px;
    border-radius: 4px;
}

.sidebar {
    grid-area: sidebar;
    background-color: white;
    padding: 20px;
    border-radius: 8px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.sidebar nav ul {
    list-style: none;
}

.sidebar nav ul li {
    margin-bottom: 10px;
}

.sidebar nav ul li a {
    color: #1f2937;
    text-decoration: none;
    padding: 10px;
    display: block;
    border-radius: 4px;
    transition: background-color 0.3s;
}

.sidebar nav ul li a:hover {
    background-color: #f3f4f6;
}

.main-content {
    grid-area: main;
    display: flex;
    flex-direction: column;
    gap: 20px;
}

.stats-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
    gap: 20px;
}

.stat-card {
    background-color: white;
    padding: 20px;
    border-radius: 8px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.stat-card h3 {
    color: #6b7280;
    font-size: 14px;
    margin-bottom: 10px;
}

.stat-number {
    color: #1f2937;
    font-size: 28px;
    font-weight: bold;
}

.content-area {
    background-color: white;
    padding: 20px;
    border-radius: 8px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.content-area h2 {
    color: #1f2937;
    margin-bottom: 20px;
}

.activity-list {
    display: flex;
    flex-direction: column;
    gap: 10px;
}

.activity-item {
    padding: 10px;
    background-color: #f3f4f6;
    border-radius: 4px;
    color: #4b5563;
}
```

---

## Part 3: Review, Questions, Problem Solving (0.5 Hours)

### CSS Grid Garden Practice (10 minutes)

**Introduction to CSS Grid Garden:**
CSS Grid Garden is an interactive game that helps you learn CSS Grid through practice.

**How to Access:**
1. Go to https://cssgridgarden.com/
2. Complete the levels to practice Grid properties
3. Each level teaches a different Grid concept

**What You'll Practice:**
- `grid-template-columns`
- `grid-template-rows`
- `grid-column`
- `grid-row`
- `grid-area`
- `grid-template-areas`

---

### When to Use Grid vs Flexbox (10 minutes)

## Use Flexbox When:
- One-dimensional layout (row OR column)
- Navigation bars
- Button groups
- Small components
- Centering elements
- Space distribution in one direction

## Use Grid When:
- Two-dimensional layout (row AND column)
- Complex page layouts
- Dashboards
- Image galleries
- Product grids
- When you need precise control over both axes

## Can They Work Together?
Yes! A common pattern:
- Use Grid for the overall page layout
- Use Flexbox for components within the Grid

```css
.page {
    display: grid; /* Page layout */
}

.navbar {
    display: flex; /* Component layout */
}
```

---

### Review Questions (10 minutes)

**Question 1:** What is the difference between `auto-fill` and `auto-fit`?
**Answer:** `auto-fill` creates as many columns as fit, `auto-fit` does the same but can collapse empty tracks.

**Question 2:** What does `minmax(200px, 1fr)` do?
**Answer:** Creates columns that are at least 200px and up to 1fr of available space.

**Question 3:** What is `grid-template-areas` used for?
**Answer:** To create named layout areas that make the grid structure more readable.

**Question 4:** What is the difference between `grid-column` and `grid-row`?
**Answer:** `grid-column` controls horizontal placement, `grid-row` controls vertical placement.

**Question 5:** What does `fr` unit represent in Grid?
**Answer:** A fraction of the available space in the grid container.

**Question 6:** When should you use Grid instead of Flexbox?
**Answer:** When you need two-dimensional layout control (both rows and columns).

**Question 7:** What does `gap` do in Grid?
**Answer:** Creates space between grid items (both rows and columns).

**Question 8:** How do you make an item span multiple columns?
**Answer:** Use `grid-column: span 2` or `grid-column: 1 / 3`.

---

### Practice Challenges (5 minutes)

**Challenge 1:** Create a grid with 3 columns where the middle column is twice as wide as the outer columns.

**Challenge 2:** Create a responsive card grid that automatically adjusts the number of columns based on screen size.

**Challenge 3:** Create a layout with header, sidebar, main content, and footer using grid template areas.

---

### Homework Assignment

**Task:** Create a complete responsive dashboard using CSS Grid.

**Requirements:**
- Use grid template areas for the main layout
- Create a responsive stats grid using `auto-fill` and `minmax`
- Use Grid child properties for spanning elements
- Implement proper gap and alignment
- Make it responsive for different screen sizes
- Include at least one area with `grid-column: span` or `grid-row: span`

**Due Date:** Next session

---

## End of Session 6

**Next Session:** Transitions, Transforms, Animations (CSS Transitions, 2D/3D Transforms, @keyframes, Animation properties, Filters, Gradients, practical animation examples)
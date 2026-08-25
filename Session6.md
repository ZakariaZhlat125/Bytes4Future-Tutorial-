# Session 6: CSS Grid

## Duration Breakdown (2.5 Hours Total)
- **1.5 Hours**: Interactive Learning (All Sections 1-4 with Explain → Example → Try it yourself → Predict → Experiment → Challenge → Bug Hunting → Quiz)
- **20 Minutes**: Comprehensive Project (Single component combining all concepts)
- **10 Minutes**: Review & Assessment

---

## Part 1: Interactive Learning (1.5 Hours)

### Section 1: CSS Grid Introduction

## Explain
CSS Grid is a two-dimensional layout system that works with both rows and columns simultaneously:

- `display: grid` enables grid layout
- Two-dimensional (rows AND columns)
- Great for complex layouts, dashboards, galleries
- More powerful than Flexbox for 2D layouts
- Can work with Flexbox together

## Example
```css
.container {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 20px;
}
```

## Try it yourself
Create a simple grid container.

```html
<div class="grid-container">
    <div class="item">Item 1</div>
    <div class="item">Item 2</div>
    <div class="item">Item 3</div>
    <div class="item">Item 4</div>
</div>
```

```css
.grid-container {
    /* Enable grid and add basic styling */
}

.item {
    /* Add basic styling */
}
```

## Predict
What will happen with this code?

```css
.container {
    display: grid;
}
```

## Experiment
1. What happens when you enable grid on a container?
2. How does grid differ from flexbox?
3. Try adding different numbers of items

## Challenge
**Task:** Create a simple grid with:
- 3 columns of equal width
- 2 rows
- 20px gap between items
- Time: 10 minutes
- Hint: Use `grid-template-columns: repeat(3, 1fr)`

<details>
<summary>Solution</summary>

```html
<div class="simple-grid">
    <div class="item">1</div>
    <div class="item">2</div>
    <div class="item">3</div>
    <div class="item">4</div>
    <div class="item">5</div>
    <div class="item">6</div>
</div>
```

```css
.simple-grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    grid-template-rows: repeat(2, 100px);
    gap: 20px;
}

.item {
    background: #2563eb;
    color: white;
    display: flex;
    align-items: center;
    justify-content: center;
    border-radius: 8px;
}
```

</details>

## Bug Hunting
Find the bug in this code:

```css
.buggy {
    display: grid;
    /* Items not in grid layout */
}
```

<details>
<summary>Solution</summary>

Missing `grid-template-columns` or `grid-template-rows`. Without defining columns/rows, items stack in a single column.

</details>

## Quiz
1. What property enables CSS Grid?
2. Is Grid one-dimensional or two-dimensional?
3. What's a common use case for Grid?

<details>
<summary>Quiz Answers</summary>

1. `display: grid` enables CSS Grid layout
2. Grid is two-dimensional (works with both rows and columns)
3. Common use cases include complex page layouts, dashboards, and image galleries

</details>

---

### Section 2: Grid Parent Properties

## Explain
Grid parent properties control the overall grid structure:

- `grid-template-columns` - Define column widths
- `grid-template-rows` - Define row heights
- `gap` - Space between items
- `grid-template-areas` - Named layout areas
- Alignment properties - Position items

## Example
```css
.container {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    grid-template-rows: auto;
    gap: 20px;
    justify-content: center;
    align-items: center;
}
```

## Try it yourself
Create a grid with different parent properties.

```html
<div class="grid-parent">
    <div class="item">Item 1</div>
    <div class="item">Item 2</div>
    <div class="item">Item 3</div>
</div>
```

```css
.grid-parent {
    display: grid;
    /* Add different column patterns */
}
```

## Predict
What will happen with this code?

```css
.container {
    grid-template-columns: 1fr 2fr 1fr;
}
```

## Experiment
1. What does `fr` unit represent?
2. How does `repeat()` work?
3. What's the difference between `gap` and margins?

## Challenge
**Task:** Create a layout with:
- Header spanning full width
- Sidebar and main content side by side
- Footer spanning full width
- Using grid-template-areas
- Time: 12 minutes
- Hint: Define the areas visually in grid-template-areas

<details>
<summary>Solution</summary>

```html
<div class="page-layout">
    <header class="header">Header</header>
    <aside class="sidebar">Sidebar</aside>
    <main class="main">Main</main>
    <footer class="footer">Footer</footer>
</div>
```

```css
.page-layout {
    display: grid;
    grid-template-columns: 250px 1fr;
    grid-template-rows: auto 1fr auto;
    grid-template-areas:
        "header header"
        "sidebar main"
        "footer footer";
    gap: 20px;
    min-height: 100vh;
}

.header {
    grid-area: header;
    background: #2563eb;
    color: white;
    padding: 20px;
}

.sidebar {
    grid-area: sidebar;
    background: white;
    padding: 20px;
}

.main {
    grid-area: main;
    background: white;
    padding: 20px;
}

.footer {
    grid-area: footer;
    background: #1f2937;
    color: white;
    padding: 20px;
}
```

</details>

## Bug Hunting
Find the bug in this code:

```css
.buggy {
    display: grid;
    grid-template-areas:
        "header header"
        "sidebar main";
    /* Areas not working */
}
```

<details>
<summary>Solution</summary>

Missing `grid-template-columns` and `grid-template-rows`. Template areas need column and row definitions to work.

</details>

## Quiz
1. What does `fr` unit mean in Grid?
2. What does `repeat(3, 1fr)` create?
3. What's the purpose of grid-template-areas?

<details>
<summary>Quiz Answers</summary>

1. `fr` means "fraction" of available space
2. Creates 3 equal columns that each take 1 fraction of space
3. Grid-template-areas allows you to create named layout areas for more readable grid structures

</details>

---

### Section 3: Grid Child Properties

## Explain
Grid child properties control individual grid item placement:

- `grid-column` - Horizontal placement and spanning
- `grid-row` - Vertical placement and spanning
- `grid-area` - Named area or line-based placement
- `justify-self` - Override horizontal alignment
- `align-self` - Override vertical alignment

## Example
```css
.item {
    grid-column: 1 / 3; /* Span 2 columns */
    grid-row: span 2; /* Span 2 rows */
    justify-self: center;
}
```

## Try it yourself
Create grid items with different child properties.

```html
<div class="grid-container">
    <div class="item item-span">Spanning Item</div>
    <div class="item">Normal Item</div>
    <div class="item">Normal Item</div>
</div>
```

```css
.item-span {
    /* Make this span multiple columns */
}
```

## Predict
What will happen with this code?

```css
.item {
    grid-column: span 2;
}
```

## Experiment
1. What does `grid-column: 1 / 3` mean?
2. How does `span` work?
3. Can you override parent alignment?

## Challenge
**Task:** Create a grid where:
- First item spans 2 columns
- Second item spans 2 rows
- Third item spans both 2 columns and 2 rows
- Time: 12 minutes
- Hint: Use `grid-column: span X` and `grid-row: span Y`

<details>
<summary>Solution</summary>

```html
<div class="spanning-grid">
    <div class="item span-col">Span 2 Columns</div>
    <div class="item">Normal</div>
    <div class="item span-row">Span 2 Rows</div>
    <div class="item">Normal</div>
    <div class="item span-both">Span Both</div>
</div>
```

```css
.spanning-grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    grid-template-rows: repeat(3, 100px);
    gap: 10px;
}

.item {
    background: #2563eb;
    color: white;
    display: flex;
    align-items: center;
    justify-content: center;
    border-radius: 8px;
}

.span-col {
    grid-column: span 2;
    background: #10b981;
}

.span-row {
    grid-row: span 2;
    background: #f59e0b;
}

.span-both {
    grid-column: span 2;
    grid-row: span 2;
    background: #7c3aed;
}
```

</details>

## Bug Hunting
Find the bug in this code:

```css
.buggy {
    grid-column: 5;
    /* Item not spanning correctly */
}
```

<details>
<summary>Solution</summary>

Need to use `span` keyword or line numbers: `grid-column: span 2` or `grid-column: 1 / 3`

</details>

## Quiz
1. What does `grid-column: span 2` do?
2. What's the difference between `grid-column` and `grid-row`?
3. Can child properties override parent alignment?

<details>
<summary>Quiz Answers</summary>

1. Makes the item span 2 columns horizontally
2. `grid-column` controls horizontal placement, `grid-row` controls vertical placement
3. Yes, `justify-self` and `align-self` can override parent `justify-items` and `align-items`

</details>

---

### Section 4: Responsive Grid Tools

## Explain
Responsive grid tools make layouts adaptive without media queries:

- `minmax(min, max)` - Set minimum and maximum sizes
- `auto-fill` - Create as many columns as fit
- `auto-fit` - Like auto-fill but collapses empty tracks
- Best for responsive card galleries

## Example
```css
.container {
    grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
    gap: 20px;
}
```

## Try it yourself
Create a responsive grid.

```html
<div class="responsive-grid">
    <div class="card">Card 1</div>
    <div class="card">Card 2</div>
    <div class="card">Card 3</div>
    <div class="card">Card 4</div>
</div>
```

```css
.responsive-grid {
    display: grid;
    /* Add responsive columns */
}
```

## Predict
What will happen with this code?

```css
.container {
    grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
}
```

## Experiment
1. What's the difference between auto-fill and auto-fit?
2. How does minmax work?
3. Try different minmax values

## Challenge
**Task:** Create a responsive card gallery with:
- Minimum card width of 200px
- Maximum flexible width
- Automatically adjusts columns based on screen
- Time: 12 minutes
- Hint: Use `repeat(auto-fit, minmax(200px, 1fr))`

<details>
<summary>Solution</summary>

```html
<div class="card-gallery">
    <div class="card">Card 1</div>
    <div class="card">Card 2</div>
    <div class="card">Card 3</div>
    <div class="card">Card 4</div>
    <div class="card">Card 5</div>
    <div class="card">Card 6</div>
</div>
```

```css
.card-gallery {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
    gap: 20px;
    max-width: 1200px;
    margin: 0 auto;
}

.card {
    background: white;
    padding: 20px;
    border-radius: 8px;
    box-shadow: 0 2px 4px rgba(0,0,0,0.1);
    min-height: 150px;
    display: flex;
    align-items: center;
    justify-content: center;
}
```

</details>

## Bug Hunting
Find the bug in this code:

```css
.buggy {
    grid-template-columns: repeat(auto-fill, 200px);
    /* Not responsive as expected */
}
```

<details>
<summary>Solution</summary>

Missing `minmax()`. Should be `repeat(auto-fill, minmax(200px, 1fr))` for proper responsive behavior.

</details>

## Quiz
1. What does `minmax(200px, 1fr)` do?
2. What's the difference between auto-fill and auto-fit?
3. Why use responsive grid tools?

<details>
<summary>Quiz Answers</summary>

1. Creates columns that are at least 200px and can grow up to 1fr of available space
2. auto-fill creates as many columns as fit, auto-fit does the same but collapses empty tracks
3. Responsive grid tools create adaptive layouts without needing media queries

</details>

---

## Part 2: Comprehensive Project (20 minutes)

### Project: Complete Grid Dashboard

**Task:** Create a comprehensive dashboard that demonstrates all CSS Grid concepts:

**Requirements:**
1. **Grid Template Areas Layout**
   - Header spanning full width
   - Sidebar and main content side by side
   - Footer spanning full width
   - Named areas for readability

2. **Responsive Grid Components**
   - Stats grid using `auto-fit` and `minmax`
   - Card gallery with responsive columns
   - Feature grid with spanning items

3. **Grid Child Properties**
   - Items spanning multiple columns/rows
   - Custom alignment with `justify-self` and `align-self`
   - Named area assignments

4. **Advanced Grid Features**
   - Fraction units (`fr`) for flexible sizing
   - `repeat()` function for pattern repetition
   - Proper gap and alignment

**Time:** 20 minutes
**Hint:** Combine Grid for page layout with nested Grid for components

<details>
<summary>Solution</summary>

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Grid Dashboard</title>
    <link rel="stylesheet" href="grid-dashboard.css">
</head>
<body>
    <div class="dashboard">
        <header class="dashboard-header">
            <h1>Grid Dashboard</h1>
            <div class="user-profile">User</div>
        </header>
        
        <aside class="sidebar">
            <nav class="sidebar-nav">
                <a href="#" class="nav-item active">Dashboard</a>
                <a href="#" class="nav-item">Analytics</a>
                <a href="#" class="nav-item">Reports</a>
                <a href="#" class="nav-item">Settings</a>
            </nav>
        </aside>
        
        <main class="main-content">
            <section class="stats-section">
                <h2 class="section-title">Statistics</h2>
                <div class="stats-grid">
                    <div class="stat-card stat-primary">
                        <div class="stat-icon">👥</div>
                        <div class="stat-info">
                            <div class="stat-number">1,234</div>
                            <div class="stat-label">Users</div>
                        </div>
                    </div>
                    <div class="stat-card stat-secondary">
                        <div class="stat-icon">💰</div>
                        <div class="stat-info">
                            <div class="stat-number">$45.6K</div>
                            <div class="stat-label">Revenue</div>
                        </div>
                    </div>
                    <div class="stat-card stat-accent">
                        <div class="stat-icon">📊</div>
                        <div class="stat-info">
                            <div class="stat-number">789</div>
                            <div class="stat-label">Orders</div>
                        </div>
                    </div>
                    <div class="stat-card stat-success">
                        <div class="stat-icon">📈</div>
                        <div class="stat-info">
                            <div class="stat-number">+23%</div>
                            <div class="stat-label">Growth</div>
                        </div>
                    </div>
                </div>
            </section>
            
            <section class="features-section">
                <h2 class="section-title">Features</h2>
                <div class="features-grid">
                    <div class="feature-card feature-span">
                        <h3>Analytics</h3>
                        <p>Advanced analytics dashboard with real-time data visualization and insights.</p>
                    </div>
                    <div class="feature-card">
                        <h3>Reports</h3>
                        <p>Generate detailed reports with customizable filters and export options.</p>
                    </div>
                    <div class="feature-card">
                        <h3>Users</h3>
                        <p>Manage users with comprehensive permissions and role-based access.</p>
                    </div>
                    <div class="feature-card feature-span-both">
                        <h3>Integration</h3>
                        <p>Seamless integration with third-party services and APIs for extended functionality.</p>
                    </div>
                </div>
            </section>
            
            <section class="cards-section">
                <h2 class="section-title">Recent Items</h2>
                <div class="cards-grid">
                    <div class="item-card">
                        <div class="card-header">Item 1</div>
                        <div class="card-body">Description here</div>
                    </div>
                    <div class="item-card">
                        <div class="card-header">Item 2</div>
                        <div class="card-body">Description here</div>
                    </div>
                    <div class="item-card">
                        <div class="card-header">Item 3</div>
                        <div class="card-body">Description here</div>
                    </div>
                    <div class="item-card">
                        <div class="card-header">Item 4</div>
                        <div class="card-body">Description here</div>
                    </div>
                    <div class="item-card">
                        <div class="card-header">Item 5</div>
                        <div class="card-body">Description here</div>
                    </div>
                </div>
            </section>
        </main>
        
        <footer class="dashboard-footer">
            <p>&copy; 2024 Grid Dashboard. All rights reserved.</p>
        </footer>
    </div>
</body>
</html>
```

```css
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    background-color: #f3f4f6;
    min-height: 100vh;
}

/* Main Dashboard Layout - Grid Template Areas */
.dashboard {
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

.dashboard-header {
    grid-area: header;
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    color: white;
    padding: 20px 30px;
    border-radius: 12px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

.dashboard-header h1 {
    font-size: 24px;
    font-weight: bold;
}

.user-profile {
    background: rgba(255, 255, 255, 0.2);
    padding: 8px 16px;
    border-radius: 6px;
    font-weight: 500;
}

.sidebar {
    grid-area: sidebar;
    background: white;
    padding: 20px;
    border-radius: 12px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.sidebar-nav {
    display: flex;
    flex-direction: column;
    gap: 10px;
}

.nav-item {
    padding: 12px 16px;
    color: #1f2937;
    text-decoration: none;
    border-radius: 8px;
    transition: all 0.3s;
    font-weight: 500;
}

.nav-item:hover {
    background: #f3f4f6;
    color: #667eea;
}

.nav-item.active {
    background: #667eea;
    color: white;
}

.main-content {
    grid-area: main;
    display: flex;
    flex-direction: column;
    gap: 30px;
}

.section-title {
    color: #1f2937;
    margin-bottom: 20px;
    font-size: 20px;
}

/* Stats Grid - Responsive with auto-fit */
.stats-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
    gap: 20px;
}

.stat-card {
    background: white;
    padding: 20px;
    border-radius: 12px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    display: flex;
    align-items: center;
    gap: 15px;
    transition: transform 0.3s, box-shadow 0.3s;
}

.stat-card:hover {
    transform: translateY(-5px);
    box-shadow: 0 8px 12px rgba(0, 0, 0, 0.15);
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

.stat-primary { border-left: 4px solid #667eea; }
.stat-secondary { border-left: 4px solid #10b981; }
.stat-accent { border-left: 4px solid #f59e0b; }
.stat-success { border-left: 4px solid #dc2626; }

/* Features Grid - With Spanning */
.features-grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    grid-template-rows: repeat(2, auto);
    gap: 20px;
}

.feature-card {
    background: white;
    padding: 25px;
    border-radius: 12px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    transition: all 0.3s;
}

.feature-card:hover {
    transform: translateY(-5px);
    box-shadow: 0 8px 12px rgba(0, 0, 0, 0.15);
}

.feature-card h3 {
    color: #667eea;
    margin-bottom: 10px;
    font-size: 18px;
}

.feature-card p {
    color: #4b5563;
    line-height: 1.6;
}

.feature-span {
    grid-column: span 2;
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
}

.feature-span h3,
.feature-span p {
    color: white;
}

.feature-span-both {
    grid-column: span 3;
    background: linear-gradient(135deg, #10b981 0%, #059669 100%);
}

.feature-span-both h3,
.feature-span-both p {
    color: white;
}

/* Cards Grid - Responsive */
.cards-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
    gap: 20px;
}

.item-card {
    background: white;
    padding: 20px;
    border-radius: 12px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    transition: all 0.3s;
}

.item-card:hover {
    transform: translateY(-5px);
    box-shadow: 0 8px 12px rgba(0, 0, 0, 0.15);
}

.card-header {
    font-weight: bold;
    color: #1f2937;
    margin-bottom: 10px;
    padding-bottom: 10px;
    border-bottom: 2px solid #f3f4f6;
}

.card-body {
    color: #4b5563;
    line-height: 1.6;
}

.dashboard-footer {
    grid-area: footer;
    background: #1f2937;
    color: white;
    padding: 20px;
    border-radius: 12px;
    text-align: center;
}

/* Responsive */
@media (max-width: 768px) {
    .dashboard {
        grid-template-columns: 1fr;
        grid-template-areas:
            "header"
            "main"
            "sidebar"
            "footer";
    }
    
    .features-grid {
        grid-template-columns: 1fr;
    }
    
    .feature-span,
    .feature-span-both {
        grid-column: span 1;
    }
}
```

</details>

---

## Part 3: Review & Assessment (10 minutes)

### Quick Review Quiz

**Question 1:** What is the difference between `auto-fill` and `auto-fit`?
<details>
<summary>Answer</summary>

`auto-fill` creates as many columns as will fit, `auto-fit` does the same but can collapse empty tracks to center content

</details>

**Question 2:** What does `minmax(200px, 1fr)` do?
<details>
<summary>Answer</summary>

Creates columns that are at least 200px and can grow up to 1fr of available space

</details>

**Question 3:** When should you use Grid instead of Flexbox?
<details>
<summary>Answer</summary>

Use Grid when you need two-dimensional layout control (both rows and columns), Flexbox for one-dimensional (row OR column)

</details>

**Question 4:** What does `fr` unit represent in Grid?
<details>
<summary>Answer</summary>

A fraction of the available space in the grid container

</details>

**Question 5:** How do you make an item span multiple columns?
<details>
<summary>Answer</summary>

Use `grid-column: span 2` or `grid-column: 1 / 3` (line numbers)

</details>

### Grid vs Flexbox

**Use Flexbox When:**
- One-dimensional layout (row OR column)
- Navigation bars, button groups
- Small components
- Centering elements

**Use Grid When:**
- Two-dimensional layout (row AND column)
- Complex page layouts, dashboards
- Image galleries, product grids
- Precise control over both axes

**They Can Work Together:**
- Grid for overall page layout
- Flexbox for components within Grid

### Final Challenge

**Task:** Create a responsive layout that uses ALL Grid concepts:
- Grid template areas for page structure
- Responsive columns with auto-fit and minmax
- Items spanning multiple columns/rows
- Named areas for readability
- Time: 10 minutes
- Goal: Build a complete responsive page layout

<details>
<summary>Solution Hint</summary>

Use `grid-template-areas` for the main layout, `repeat(auto-fit, minmax(250px, 1fr))` for responsive components, and `grid-column: span X` for spanning items

</details>

---

## Homework Assignment

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

**Summary:** In this session, you learned about CSS Grid (introduction, parent properties like grid-template-columns, grid-template-rows, grid-template-areas, gap, and alignment, child properties like grid-column, grid-row, grid-area, justify-self, align-self, and responsive tools like minmax, auto-fill, auto-fit). ALL concepts (Sections 1-4) were practiced through interactive exercises (Explain → Example → Try it yourself → Predict → Experiment → Challenge → Bug Hunting → Quiz), followed by a comprehensive grid dashboard project that integrated all Grid concepts for real-world application. The session also covered when to use Grid vs Flexbox and how they can work together.

**Next Session:** Transitions, Transforms, Animations (CSS Transitions, 2D/3D Transforms, @keyframes, Animation properties, Filters, Gradients, practical animation examples)
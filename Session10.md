# Session 10: Advanced Tailwind CSS

## Duration Breakdown
- **1 Hour**: Theoretical Explanation + Live Coding
- **1.5 Hours**: Practical Application (Student writes code)
- **0.5 Hours**: Review, Questions, Problem Solving

---

## Part 1: Theoretical Explanation + Live Coding (1 Hour)

### Section 1: Responsive Prefixes

**What are Responsive Prefixes:**
Tailwind provides responsive prefixes (`sm:`, `md:`, `lg:`, `xl:`, `2xl:`) to apply styles at different breakpoints.

**Default Breakpoints:**
```javascript
// tailwind.config.js
module.exports = {
  theme: {
    screens: {
      'sm': '640px',
      'md': '768px',
      'lg': '1024px',
      'xl': '1280px',
      '2xl': '1536px',
    },
  }
}
```

---

## Basic Usage

```html
<!-- Mobile: 1 column, Tablet: 2 columns, Desktop: 3 columns -->
<div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3">
    <!-- Items -->
</div>
```

---

## Common Responsive Patterns

```html
<!-- Text Size -->
<h1 class="text-2xl md:text-4xl lg:text-5xl">Responsive Heading</h1>

<!-- Padding -->
<div class="p-4 md:p-8 lg:p-12">Responsive Padding</div>

<!-- Margin -->
<div class="m-4 md:m-8 lg:m-12">Responsive Margin</div>

<!-- Grid -->
<div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4">
    <!-- Items -->
</div>

<!-- Flex Direction -->
<div class="flex flex-col md:flex-row">
    <!-- Items -->
</div>

<!-- Show/Hide -->
<div class="hidden md:block">Hidden on mobile, visible on desktop</div>
<div class="block md:hidden">Visible on mobile, hidden on desktop</div>
```

---

### Section 2: State Variants

**What are State Variants:**
State variants allow you to style elements based on their state (hover, focus, active, etc.).

---

## Hover State (hover:)

```html
<button class="bg-blue-600 hover:bg-blue-700 transition">
    Hover Me
</button>

<div class="text-gray-600 hover:text-blue-600 transition">
    Hover to change color
</div>
```

---

## Focus State (focus:)

```html
<input class="border-2 border-gray-300 focus:border-blue-500 focus:ring-2 focus:ring-blue-200 outline-none">
```

---

## Active State (active:)

```html
<button class="bg-blue-600 active:bg-blue-800 active:scale-95 transition">
    Click Me
</button>
```

---

## Disabled State (disabled:)

```html
<button disabled class="bg-gray-400 disabled:opacity-50 disabled:cursor-not-allowed">
    Disabled Button
</button>
```

---

## Group Hover (group & group-hover:)

```html
<div class="group">
    <p class="text-gray-600 group-hover:text-blue-600">
        This text changes when parent is hovered
    </p>
</div>
```

---

## Peer State (peer & peer-hover:)

```html
<div>
    <input class="peer" type="checkbox">
    <label class="peer-checked:text-blue-600 peer-checked:font-bold">
        Label that changes when checkbox is checked
    </label>
</div>
```

---

### Section 3: Dark Mode

**What is Dark Mode:**
Tailwind provides built-in support for dark mode styling.

---

## Enable Dark Mode

**In tailwind.config.js:**
```javascript
module.exports = {
  darkMode: 'class', // or 'media' for system preference
  // ...
}
```

---

## Using Dark Mode Classes

```html
<body class="dark">
    <!-- Your content -->
</body>
```

---

## Dark Mode Utilities

```html
<!-- Background -->
<div class="bg-white dark:bg-gray-900">
    <!-- Light: white, Dark: gray-900 -->
</div>

<!-- Text -->
<p class="text-gray-900 dark:text-gray-100">
    <!-- Light: dark gray, Dark: light gray -->
</p>

<!-- Border -->
<div class="border-gray-200 dark:border-gray-700">
    <!-- Light: light border, Dark: dark border -->
</div>

<!-- Combined -->
<div class="bg-white dark:bg-gray-800 text-gray-900 dark:text-white">
    <!-- Complete dark mode styling -->
</div>
```

---

## Dark Mode with Toggle

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
        tailwind.config = {
            darkMode: 'class',
        }
    </script>
</head>
<body class="bg-gray-100 dark:bg-gray-900 text-gray-900 dark:text-white">
    <button onclick="document.documentElement.classList.toggle('dark')">
        Toggle Dark Mode
    </button>
</body>
</html>
```

---

### Section 4: Customization (tailwind.config.js)

## Custom Colors

```javascript
module.exports = {
  theme: {
    extend: {
      colors: {
        primary: {
          50: '#eff6ff',
          100: '#dbeafe',
          200: '#bfdbfe',
          300: '#93c5fd',
          400: '#60a5fa',
          500: '#3b82f6',
          600: '#2563eb',
          700: '#1d4ed8',
          800: '#1e40af',
          900: '#1e3a8a',
        },
        brand: '#3b82f6',
      },
    },
  },
}
```

**Usage:**
```html
<div class="bg-primary-500 text-brand">
    Custom colors
</div>
```

---

## Custom Fonts

```javascript
module.exports = {
  theme: {
    extend: {
      fontFamily: {
        sans: ['Inter', 'sans-serif'],
        serif: ['Merriweather', 'serif'],
        mono: ['Fira Code', 'monospace'],
      },
    },
  },
}
```

**Usage:**
```html
<p class="font-sans">Sans serif font</p>
<p class="font-serif">Serif font</p>
<p class="font-mono">Monospace font</p>
```

---

## Custom Spacing

```javascript
module.exports = {
  theme: {
    extend: {
      spacing: {
        '128': '32rem',
        '144': '36rem',
      },
    },
  },
}
```

**Usage:**
```html
<div class="p-128">Custom padding</div>
```

---

## Custom Breakpoints

```javascript
module.exports = {
  theme: {
    screens: {
      '3xl': '1600px',
    },
  },
}
```

**Usage:**
```html
<div class="grid-cols-1 3xl:grid-cols-4">
    <!-- Custom breakpoint -->
</div>
```

---

## Custom Box Shadows

```javascript
module.exports = {
  theme: {
    extend: {
      boxShadow: {
        'xl': '0 20px 25px -5px rgba(0, 0, 0, 0.1)',
        '2xl': '0 25px 50px -12px rgba(0, 0, 0, 0.25)',
      },
    },
  },
}
```

---

### Section 5: @apply Directive

**What is @apply:**
The `@apply` directive allows you to extract repeated patterns into component classes.

---

## Basic Usage

```css
/* In your CSS file */
.btn-primary {
    @apply bg-blue-600 text-white px-4 py-2 rounded-lg font-semibold hover:bg-blue-700 transition;
}
```

**HTML:**
```html
<button class="btn-primary">Click Me</button>
```

---

## Complex Component

```css
.card {
    @apply bg-white rounded-xl p-6 shadow-md hover:shadow-lg transition;
}

.card-title {
    @apply text-xl font-semibold text-gray-900 mb-4;
}

.card-description {
    @apply text-gray-600 mb-6;
}
```

**HTML:**
```html
<div class="card">
    <h3 class="card-title">Card Title</h3>
    <p class="card-description">Card description</p>
</div>
```

---

## When to Use @apply

**Use @apply when:**
- You have repeated patterns
- You want cleaner HTML
- Building component libraries

**Don't overuse @apply:**
- It can make CSS harder to read
- Defeats the purpose of utility classes
- Should be used sparingly

---

### Section 6: Plugins Overview

**What are Plugins:**
Plugins extend Tailwind's functionality with additional utilities and components.

---

## Official Plugins

**@tailwindcss/forms:**
```bash
npm install @tailwindcss/forms
```

```javascript
// tailwind.config.js
module.exports = {
  plugins: [
    require('@tailwindcss/forms'),
  ],
}
```

**Features:**
- Form styling utilities
- Reset styles for form elements
- Custom form components

---

**@tailwindcss/typography:**
```bash
npm install @tailwindcss/typography
```

```javascript
// tailwind.config.js
module.exports = {
  plugins: [
    require('@tailwindcss/typography'),
  ],
}
```

**Usage:**
```html
<article class="prose prose-lg">
    <!-- Beautiful typography -->
</article>
```

---

**@tailwindcss/aspect-ratio:**
```bash
npm install @tailwindcss/aspect-ratio
```

**Usage:**
```html
<div class="aspect-w-16 aspect-h-9">
    <!-- Maintains 16:9 aspect ratio -->
</div>
```

---

## Community Plugins

Many community plugins available:
- tailwindcss-animate
- tailwindcss-rtl
- tailwindcss-safe-area
- And many more...

---

## Part 2: Practical Application (1.5 Hours)

### Exercise 1: Responsive Product Store with Tailwind (45 minutes)

**Task:**
Rebuild the product store from Session 8 using Tailwind CSS with full responsiveness.

**HTML:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
        tailwind.config = {
            darkMode: 'class',
            theme: {
                extend: {
                    colors: {
                        primary: {
                            500: '#3b82f6',
                            600: '#2563eb',
                            700: '#1d4ed8',
                        }
                    }
                }
            }
        }
    </script>
</head>
<body class="bg-gray-100 dark:bg-gray-900 text-gray-900 dark:text-white min-h-screen">
    <!-- Navbar -->
    <nav class="bg-white dark:bg-gray-800 shadow-md sticky top-0 z-50">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <div class="flex justify-between items-center h-16">
                <div class="text-2xl font-bold text-primary-600">TechStore</div>
                
                <!-- Desktop Menu -->
                <ul class="hidden md:flex gap-8">
                    <li><a href="#home" class="text-gray-700 dark:text-gray-300 hover:text-primary-600 transition">Home</a></li>
                    <li><a href="#products" class="text-gray-700 dark:text-gray-300 hover:text-primary-600 transition">Products</a></li>
                    <li><a href="#about" class="text-gray-700 dark:text-gray-300 hover:text-primary-600 transition">About</a></li>
                    <li><a href="#contact" class="text-gray-700 dark:text-gray-300 hover:text-primary-600 transition">Contact</a></li>
                </ul>
                
                <div class="flex items-center gap-4">
                    <!-- Dark Mode Toggle -->
                    <button onclick="document.documentElement.classList.toggle('dark')" class="p-2 rounded-full hover:bg-gray-200 dark:hover:bg-gray-700 transition">
                        🌙
                    </button>
                    
                    <button class="bg-primary-600 text-white px-4 py-2 rounded-lg font-semibold hover:bg-primary-700 transition">
                        Cart (0)
                    </button>
                </div>
            </div>
        </div>
    </nav>
    
    <!-- Hero Section -->
    <section class="bg-gradient-to-r from-primary-600 to-purple-600 text-white py-16 md:py-24 px-4 text-center">
        <div class="max-w-4xl mx-auto">
            <h1 class="text-3xl md:text-5xl font-bold mb-6">Welcome to TechStore</h1>
            <p class="text-lg md:text-xl mb-8 opacity-90">Discover the latest technology products</p>
            <button class="bg-white text-primary-600 px-8 py-4 rounded-lg font-semibold text-lg hover:bg-blue-50 transition transform hover:scale-105">
                Shop Now
            </button>
        </div>
    </section>
    
    <!-- Products Section -->
    <section class="max-w-7xl mx-auto py-12 md:py-16 px-4">
        <h2 class="text-2xl md:text-3xl font-bold mb-8">Featured Products</h2>
        
        <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-6">
            <!-- Product 1 -->
            <div class="bg-white dark:bg-gray-800 rounded-xl p-6 shadow-md hover:shadow-xl transition transform hover:-translate-y-2">
                <div class="h-48 bg-gray-200 dark:bg-gray-700 rounded-lg mb-4 flex items-center justify-center">
                    <span class="text-gray-400 dark:text-gray-500">Product Image</span>
                </div>
                <h3 class="text-lg font-semibold mb-2">Laptop Pro</h3>
                <p class="text-2xl font-bold text-primary-600 mb-4">$999.99</p>
                <button class="w-full bg-primary-600 text-white py-3 rounded-lg font-semibold hover:bg-primary-700 transition">
                    Add to Cart
                </button>
            </div>
            
            <!-- Product 2 -->
            <div class="bg-white dark:bg-gray-800 rounded-xl p-6 shadow-md hover:shadow-xl transition transform hover:-translate-y-2">
                <div class="h-48 bg-gray-200 dark:bg-gray-700 rounded-lg mb-4 flex items-center justify-center">
                    <span class="text-gray-400 dark:text-gray-500">Product Image</span>
                </div>
                <h3 class="text-lg font-semibold mb-2">Headphones</h3>
                <p class="text-2xl font-bold text-primary-600 mb-4">$199.99</p>
                <button class="w-full bg-primary-600 text-white py-3 rounded-lg font-semibold hover:bg-primary-700 transition">
                    Add to Cart
                </button>
            </div>
            
            <!-- Product 3 -->
            <div class="bg-white dark:bg-gray-800 rounded-xl p-6 shadow-md hover:shadow-xl transition transform hover:-translate-y-2">
                <div class="h-48 bg-gray-200 dark:bg-gray-700 rounded-lg mb-4 flex items-center justify-center">
                    <span class="text-gray-400 dark:text-gray-500">Product Image</span>
                </div>
                <h3 class="text-lg font-semibold mb-2">Smartphone</h3>
                <p class="text-2xl font-bold text-primary-600 mb-4">$699.99</p>
                <button class="w-full bg-primary-600 text-white py-3 rounded-lg font-semibold hover:bg-primary-700 transition">
                    Add to Cart
                </button>
            </div>
            
            <!-- Product 4 -->
            <div class="bg-white dark:bg-gray-800 rounded-xl p-6 shadow-md hover:shadow-xl transition transform hover:-translate-y-2">
                <div class="h-48 bg-gray-200 dark:bg-gray-700 rounded-lg mb-4 flex items-center justify-center">
                    <span class="text-gray-400 dark:text-gray-500">Product Image</span>
                </div>
                <h3 class="text-lg font-semibold mb-2">Tablet</h3>
                <p class="text-2xl font-bold text-primary-600 mb-4">$449.99</p>
                <button class="w-full bg-primary-600 text-white py-3 rounded-lg font-semibold hover:bg-primary-700 transition">
                    Add to Cart
                </button>
            </div>
        </div>
    </section>
    
    <!-- Footer -->
    <footer class="bg-gray-900 dark:bg-black text-white py-8 text-center">
        <p>&copy; 2024 TechStore. All rights reserved.</p>
    </footer>
</body>
</html>
```

---

### Exercise 2: Advanced Component with @apply (25 minutes)

**Task:**
Create advanced components using @apply directive.

**CSS File (styles.css):**
```css
@tailwind base;
@tailwind components;
@tailwind utilities;

@layer components {
    .btn {
        @apply px-4 py-2 rounded-lg font-semibold transition duration-200 ease-in-out;
    }
    
    .btn-primary {
        @apply bg-blue-600 text-white hover:bg-blue-700 focus:ring-2 focus:ring-blue-300;
    }
    
    .btn-secondary {
        @apply bg-gray-200 text-gray-900 hover:bg-gray-300 focus:ring-2 focus:ring-gray-400;
    }
    
    .card {
        @apply bg-white dark:bg-gray-800 rounded-xl p-6 shadow-md hover:shadow-lg transition;
    }
    
    .input-field {
        @apply w-full px-4 py-2 border-2 border-gray-300 rounded-lg focus:border-blue-500 focus:ring-2 focus:ring-blue-200 outline-none transition;
    }
}
```

**HTML:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link href="styles.css" rel="stylesheet">
</head>
<body class="bg-gray-100 p-8">
    <button class="btn btn-primary">Primary Button</button>
    <button class="btn btn-secondary">Secondary Button</button>
    
    <div class="card mt-8">
        <h3 class="text-xl font-semibold mb-4">Card Title</h3>
        <p class="text-gray-600">Card content goes here.</p>
    </div>
    
    <input type="text" class="input-field mt-4" placeholder="Enter text">
</body>
</html>
```

---

### Exercise 3: Custom Theme Configuration (20 minutes)

**Task:**
Configure a custom Tailwind theme and use it.

**tailwind.config.js:**
```javascript
module.exports = {
  darkMode: 'class',
  content: [
    "./index.html",
    "./src/**/*.{js,ts,jsx,tsx}",
  ],
  theme: {
    extend: {
      colors: {
        brand: {
          50: '#f0f9ff',
          100: '#e0f2fe',
          200: '#bae6fd',
          300: '#7dd3fc',
          400: '#38bdf8',
          500: '#0ea5e9',
          600: '#0284c7',
          700: '#0369a1',
          800: '#075985',
          900: '#0c4a6e',
        },
      },
      fontFamily: {
        heading: ['Playfair Display', 'serif'],
        body: ['Inter', 'sans-serif'],
      },
      spacing: {
        '18': '4.5rem',
        '88': '22rem',
        '128': '32rem',
      },
      boxShadow: {
        'soft': '0 4px 20px rgba(0, 0, 0, 0.08)',
        'glow': '0 0 20px rgba(14, 165, 233, 0.5)',
      },
    },
  },
  plugins: [],
}
```

**HTML:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="tailwind.config.js"></script>
</head>
<body class="bg-gray-100 p-8">
    <h1 class="text-4xl font-heading text-brand-600 mb-8">Custom Theme</h1>
    
    <div class="bg-white p-8 rounded-xl shadow-soft">
        <p class="font-body text-gray-700">Using custom colors, fonts, spacing, and shadows.</p>
    </div>
    
    <button class="mt-4 bg-brand-500 text-white px-6 py-3 rounded-lg shadow-glow hover:shadow-lg transition">
        Custom Button
    </button>
</body>
</html>
```

---

## Part 3: Review, Questions, Problem Solving (0.5 Hours)

### When to Use Tailwind vs Custom CSS (10 minutes)

## Use Tailwind When:
- Rapid development needed
- Consistent design system required
- Team collaboration
- Responsive design focus
- Want to avoid naming conflicts

## Use Custom CSS When:
- Complex animations
- Third-party component integration
- Very specific custom designs
- When Tailwind utilities don't fit
- Legacy codebase

## Best Approach:
Combine both! Use Tailwind for 80-90% of styling, custom CSS for the rest.

---

### JIT/Purge for Performance (10 minutes)

## What is JIT (Just-In-Time):
JIT engine generates CSS on-demand, only including styles you actually use.

## What is Purge:
Purge removes unused CSS from the final bundle.

## Configuration (tailwind.config.js):
```javascript
module.exports = {
  content: [
    "./index.html",
    "./src/**/*.{js,ts,jsx,tsx}",
  ],
  // Tailwind will scan these files and only include used styles
}
```

## Performance Benefits:
- Smaller CSS bundle
- Faster load times
- Better user experience
- Reduced bandwidth

---

### Review Questions (10 minutes)

**Question 1:** What are the default Tailwind breakpoints?
**Answer:** sm (640px), md (768px), lg (1024px), xl (1280px), 2xl (1536px).

**Question 2:** How do you create a hover effect in Tailwind?
**Answer:** Use the `hover:` prefix, like `hover:bg-blue-700`.

**Question 3:** How do you enable dark mode in Tailwind?
**Answer:** Set `darkMode: 'class'` in tailwind.config.js and add `dark` class to HTML.

**Question 4:** What is the purpose of the @apply directive?
**Answer:** To extract repeated utility patterns into reusable component classes.

**Question 5:** How do you customize colors in Tailwind?
**Answer:** Extend the colors object in tailwind.config.js theme.extend.

**Question 6:** What is the difference between JIT and Purge?
**Answer:** JIT generates CSS on-demand, Purge removes unused CSS (JIT replaces Purge in newer versions).

**Question 7:** When should you use custom CSS instead of Tailwind?
**Answer:** For complex animations, third-party components, or very specific custom designs.

**Question 8:** What are some official Tailwind plugins?
**Answer:** @tailwindcss/forms, @tailwindcss/typography, @tailwindcss/aspect-ratio.

---

### Practice Challenges (5 minutes)

**Challenge 1:** Create a responsive navbar that shows hamburger menu on mobile and full menu on desktop.

**Challenge 2:** Create a card component with @apply that includes hover effects and dark mode support.

**Challenge 3:** Configure a custom color palette and use it in a component.

---

### Homework Assignment

**Task:** Build a complete responsive application with Tailwind.

**Requirements:**
- Use npm installation method
- Configure custom theme
- Implement dark mode with toggle
- Make fully responsive (mobile, tablet, desktop)
- Use @apply for repeated patterns
- Include hover and focus states
- Optimize for performance (content configuration)

**Due Date:** End of course

---

## End of Session 10

## 🎉 CSS Complete Course Completion!

**You have successfully completed the entire CSS + Tailwind CSS course!**

**Course Summary:**
- Sessions 1-8: CSS Fundamentals to Advanced
- Session 9: Tailwind CSS Fundamentals
- Session 10: Advanced Tailwind CSS

**Final Achievement:**
You now have comprehensive knowledge of both traditional CSS and modern utility-first CSS with Tailwind!

**Next Steps:**
- Build real-world projects
- Explore JavaScript frameworks (React, Vue, etc.)
- Stay updated with CSS features
- Contribute to open source
- Build your portfolio

**Happy Coding! 🚀**
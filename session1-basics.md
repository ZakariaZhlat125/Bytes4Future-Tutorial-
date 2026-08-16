# Session 1: Introduction to JavaScript

## 📚 Theory (1h)

### What is JavaScript?

JavaScript is a high-level, interpreted programming language primarily used for web development. It's one of the core technologies of the World Wide Web, alongside HTML and CSS.

**Key Characteristics:**
- **Dynamic:** Variables can hold any type and can change types
- **Interpreted:** Code is executed line by line (with JIT compilation in modern engines)
- **Multi-paradigm:** Supports procedural, object-oriented, and functional programming
- **Event-driven:** Responds to user actions and events
- **Cross-platform:** Runs in browsers, servers (Node.js), mobile apps, and more

**Why Learn JavaScript?**
- Essential for web development (frontend and backend)
- Huge ecosystem and community
- High demand in job market
- Versatile - can build anything from websites to mobile apps
- Easy to start, powerful to master

### How to Study This Course

**Learning Strategy:**
1. **Read the theory** - Understand concepts before coding
2. **Practice immediately** - Apply what you learn right away
3. **Experiment** - Try variations and break things intentionally
4. **Review regularly** - Go back to previous sessions
5. **Build projects** - Apply knowledge to real scenarios

**Study Tips:**
- Don't just copy code - understand why it works
- Use Chrome DevTools to inspect and debug
- Take notes on concepts that confuse you
- Practice daily, even if just for 15-30 minutes
- Join JavaScript communities for help and inspiration

### Setting Up Environment & Tools

#### Essential Tools:

**1. Code Editor**
- **VS Code** (Recommended): Free, powerful, great JavaScript support
- Alternative: Sublime Text, Atom, WebStorm (paid)

**2. Web Browser**
- **Google Chrome** (Recommended): Best DevTools
- Alternative: Firefox, Edge, Safari

**3. Node.js** (for running JavaScript outside browser)
- Download from: https://nodejs.org/
- Install latest LTS version

#### Setting Up VS Code:

**Recommended Extensions:**
- ESLint - Code linting and error detection
- Prettier - Code formatting
- JavaScript (ES6) code snippets - Quick code templates
- Live Server - Auto-reload for web development

**VS Code Setup Steps:**
1. Install VS Code from https://code.visualstudio.com/
2. Open VS Code
3. Go to Extensions (Ctrl+Shift+X)
4. Search and install recommended extensions
5. Create a new folder for your JavaScript projects
6. Open the folder in VS Code

#### Creating Your First JavaScript File:

**Option 1: Browser-based (easiest for beginners)**
1. Create an HTML file: `index.html`
2. Add `<script>` tag
3. Open in browser

**Option 2: Node.js (for console applications)**
1. Create a JavaScript file: `app.js`
2. Run with: `node app.js`

### Chrome DevTools

Chrome DevTools is a set of web developer tools built directly into the Google Chrome browser.

**How to Open DevTools:**
- **Windows/Linux:** `F12` or `Ctrl+Shift+I` or `Ctrl+Shift+J` (Console)
- **Mac:** `Cmd+Option+I` or `Cmd+Option+J` (Console)
- **Right-click** on any element → "Inspect"

**Key DevTools Panels:**

**1. Elements Panel**
- Inspect and modify HTML and CSS
- View DOM structure
- Edit styles in real-time

**2. Console Panel**
- Execute JavaScript code
- View logs and errors
- Debug and test snippets

**3. Sources Panel**
- Set breakpoints
- Step through code
- View and edit source files

**4. Network Panel**
- Monitor network requests
- Analyze load times
- Debug API calls

**Console Features:**
```javascript
// Basic logging
console.log("Hello, World!");

// Clear console
console.clear();

// Measure performance
console.time("test");
// ... code ...
console.timeEnd("test");

// Table display
console.table({name: "John", age: 25});

// Group related logs
console.group("User Info");
console.log("Name: John");
console.log("Age: 25");
console.groupEnd();
```

### Where to Put Code

#### 1. Inline JavaScript (Not Recommended)
```html
<button onclick="alert('Hello!')">Click me</button>
```
**Problems:** Hard to maintain, mixes concerns, security risks

#### 2. Internal JavaScript (Script Tag in HTML)
```html
<!DOCTYPE html>
<html>
<head>
    <title>My Page</title>
    <script>
        console.log("This runs when parsed");
    </script>
</head>
<body>
    <h1>Hello World</h1>
    <script>
        console.log("This runs when reached");
    </script>
</body>
</html>
```
**Use case:** Small scripts specific to one page

#### 3. External JavaScript File (Recommended)
```html
<!DOCTYPE html>
<html>
<head>
    <title>My Page</title>
    <script src="script.js"></script>
</head>
<body>
    <h1>Hello World</h1>
</body>
</html>
```
**Benefits:**
- Separation of concerns
- Reusable across pages
- Better caching
- Easier maintenance

#### Script Loading Attributes:

**Normal (blocks rendering):**
```html
<script src="script.js"></script>
```

**`defer` (recommended):**
```html
<script src="script.js" defer></script>
```
- Loads in background
- Executes after DOM is ready
- Maintains script order

**`async`:**
```html
<script src="script.js" async></script>
```
- Loads in background
- Executes as soon as loaded
- No order guarantee

**Best Practice:** Use `defer` for most scripts, `async` for independent scripts like analytics

### Comments & Bad Practices

#### Comments in JavaScript

**Single-line comments:**
```javascript
// This is a single-line comment
let x = 5; // This explains the variable
```

**Multi-line comments:**
```javascript
/*
 This is a multi-line comment
 It can span multiple lines
 Useful for longer explanations
*/
```

**When to Use Comments:**
- Explain "why" not "what"
- Document complex logic
- Provide context for decisions
- Add TODO/FIXME markers

**Comment Examples:**
```javascript
// Good: Explains why
// Using parseInt because we need whole numbers only
let age = parseInt(userInput);

// Bad: Explains what (obvious from code)
// Set age to 25
let age = 25;

// Good: Complex logic explanation
// Calculate discount based on:
// - Customer tier (gold/silver/bronze)
// - Purchase history
// - Current promotions
let discount = calculateDiscount(customer, purchase);

// TODO: Add error handling for invalid inputs
function processUser(input) {
    // implementation
}

// FIXME: This doesn't handle negative numbers correctly
function calculateAbsolute(num) {
    return num; // wrong implementation
}
```

#### Bad Practices to Avoid

**1. Using `var` instead of `let`/`const`:**
```javascript
// Bad
var name = "John";

// Good
let name = "John"; // if value changes
const NAME = "John"; // if constant
```

**2. Missing semicolons (can cause issues):**
```javascript
// Risky
let x = 5
let y = 10

// Better
let x = 5;
let y = 10;
```

**3. Magic numbers/strings:**
```javascript
// Bad
if (status === 1) { ... }

// Good
const STATUS_ACTIVE = 1;
if (status === STATUS_ACTIVE) { ... }
```

**4. Inconsistent naming:**
```javascript
// Bad
let userName = "John";
let user_age = 25;
let UserEmail = "john@example.com";

// Good (camelCase)
let userName = "John";
let userAge = 25;
let userEmail = "john@example.com";
```

**5. Not using strict mode:**
```javascript
// Add at top of file/script
"use strict";

// Prevents common mistakes
// Makes code more secure
// Enables optimizations
```

**6. Global variables:**
```javascript
// Bad - pollutes global scope
var globalVar = "I'm everywhere";

// Good - use functions/modules
function myFunction() {
    let localVar = "I'm local";
}
```

**7. Console logs in production:**
```javascript
// Bad - leaves debug code
console.log("Debug info");
console.error("Error happened");

// Good - remove or use proper logging
if (DEBUG_MODE) {
    console.log("Debug info");
}
```

**8. Not handling errors:**
```javascript
// Bad
let data = JSON.parse(userInput);

// Good
try {
    let data = JSON.parse(userInput);
} catch (error) {
    console.error("Invalid JSON:", error);
}
```

---

## 💻 Practical (1.5h)

### Exercise 1: Setting Up Development Environment

**Step 1: Install VS Code**
1. Download from https://code.visualstudio.com/
2. Install and launch VS Code
3. Install recommended extensions (ESLint, Prettier, Live Server)

**Step 2: Create Project Structure**
```
my-javascript-course/
├── session1/
│   ├── index.html
│   ├── script.js
│   └── style.css (optional)
```

**Step 3: Create HTML File**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Session 1 - JavaScript Basics</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <h1>JavaScript Session 1</h1>
    <div id="output"></div>
    <script src="script.js" defer></script>
</body>
</html>
```

**Step 4: Create JavaScript File**
```javascript
// script.js
console.log("JavaScript is working!");
```

**Step 5: Test Your Setup**
1. Open `index.html` in Chrome
2. Open DevTools (F12)
3. Check Console tab for the message
4. If you see "JavaScript is working!", setup is complete!

### Exercise 2: Console Methods & Styling

**Basic Console Methods:**
```javascript
// Basic logging
console.log("Regular log message");
console.info("Information message");
console.warn("Warning message");
console.error("Error message");

// Clear console
console.clear();

// Assert - only logs if condition is false
console.assert(2 + 2 === 4, "Math is broken!");
console.assert(2 + 2 === 5, "Math is broken!");

// Count occurrences
console.count("click");
console.count("click");
console.count("click");
console.countReset("click");

// Time measurement
console.time("loop");
for (let i = 0; i < 1000; i++) {
    // some operation
}
console.timeEnd("loop");

// Group related logs
console.group("User Details");
console.log("Name: John Doe");
console.log("Age: 30");
console.log("Email: john@example.com");
console.groupEnd();

// Table display
const users = [
    { name: "John", age: 30, email: "john@example.com" },
    { name: "Jane", age: 25, email: "jane@example.com" },
    { name: "Bob", age: 35, email: "bob@example.com" }
];
console.table(users);

// Trace - shows call stack
function functionA() {
    functionB();
}

function functionB() {
    console.trace("Trace from functionB");
}

functionA();
```

**Console Styling:**
```javascript
// Style with CSS
console.log("%cHello World!", "color: blue; font-size: 20px;");
console.log("%cError!", "color: red; font-weight: bold; font-size: 16px;");
console.log("%cSuccess!", "color: green; background: #e0ffe0; padding: 5px;");

// Multiple styles
console.log(
    "%c %c %c Hello %c World! %c ",
    "background: #ff0000; padding: 5px;",
    "background: #00ff00; padding: 5px;",
    "background: #0000ff; padding: 5px; color: white;",
    "background: #ffff00; padding: 5px;",
    "background: #ff00ff; padding: 5px;"
);

// Custom formatting
const styles = [
    "color: #fff",
    "background: #1890ff",
    "padding: 5px 10px",
    "border-radius: 5px",
    "font-weight: bold"
].join(";");

console.log("%c Styled Message ", styles);
```

### Exercise 3: Output to Screen

**Using console.log:**
```javascript
// Different data types
console.log("String");
console.log(42);
console.log(true);
console.log(null);
console.log(undefined);
console.log({ name: "John", age: 30 });
console.log([1, 2, 3, 4, 5]);

// Multiple arguments
console.log("Name:", "John", "Age:", 30);

// String concatenation
console.log("Hello " + "World");

// Template literals
const name = "John";
const age = 30;
console.log(`My name is ${name} and I'm ${age} years old`);
```

**Using DOM Manipulation:**
```javascript
// Get element
const output = document.getElementById("output");

// Text content
output.textContent = "Hello from JavaScript!";

// HTML content
output.innerHTML = "<h2>Welcome</h2><p>This is generated by JavaScript</p>";

// Create elements
const paragraph = document.createElement("p");
paragraph.textContent = "This is a new paragraph";
output.appendChild(paragraph);

// Style elements
output.style.color = "blue";
output.style.fontSize = "20px";
output.style.fontWeight = "bold";
```

**Using alert (not recommended for production):**
```javascript
alert("Hello, World!");
```

**Using prompt (for user input):**
```javascript
const name = prompt("What is your name?");
if (name) {
    console.log("Hello, " + name + "!");
}
```

### Exercise 4: Web API Basics

**Document Object Model (DOM):**
```javascript
// Access document
console.log(document);
console.log(document.title);
console.log(document.URL);

// Get elements
const heading = document.querySelector("h1");
console.log(heading.textContent);

// Modify elements
heading.textContent = "Modified by JavaScript";
heading.style.color = "red";

// Create and append elements
const newParagraph = document.createElement("p");
newParagraph.textContent = "This is a dynamically created paragraph";
document.body.appendChild(newParagraph);
```

**Window Object:**
```javascript
// Window properties
console.log(window.innerWidth);
console.log(window.innerHeight);
console.log(window.location.href);

// Window methods
window.alert("Alert from window");
window.setTimeout(() => {
    console.log("Delayed message");
}, 2000);

window.setInterval(() => {
    console.log("Repeated message");
}, 3000);
```

**Navigator Object:**
```javascript
// Browser information
console.log(navigator.userAgent);
console.log(navigator.platform);
console.log(navigator.language);
```

**LocalStorage:**
```javascript
// Save data
localStorage.setItem("username", "John");

// Retrieve data
const username = localStorage.getItem("username");
console.log("Username:", username);

// Remove data
localStorage.removeItem("username");

// Clear all
localStorage.clear();
```

### Exercise 5: Complete Working Example

**Complete script.js:**
```javascript
// Session 1: JavaScript Basics
// This script demonstrates basic JavaScript concepts

console.log("=== Session 1: JavaScript Basics ===");

// 1. Console methods demonstration
console.log("--- Console Methods ---");
console.log("Information message");
console.warn("Warning message");
console.error("Error message");

// 2. Console styling
console.log("--- Console Styling ---");
console.log("%cStyled Console Output", "color: blue; font-size: 18px; font-weight: bold;");

// 3. Data types
console.log("--- Data Types ---");
console.log("String:", "Hello");
console.log("Number:", 42);
console.log("Boolean:", true);
console.log("Object:", { name: "John", age: 30 });
console.log("Array:", [1, 2, 3, 4, 5]);

// 4. Variables
console.log("--- Variables ---");
let userName = "John Doe";
const userAge = 30;
console.log("User:", userName, "Age:", userAge);

// 5. Template literals
console.log("--- Template Literals ---");
console.log(`User ${userName} is ${userAge} years old`);

// 6. DOM manipulation
console.log("--- DOM Manipulation ---");
const output = document.getElementById("output");
if (output) {
    output.innerHTML = `
        <h2>Welcome to JavaScript!</h2>
        <p>This content was generated dynamically.</p>
        <p>User: <strong>${userName}</strong></p>
        <p>Age: <strong>${userAge}</strong></p>
    `;
}

// 7. Web API
console.log("--- Web API ---");
console.log("Browser:", navigator.userAgent);
console.log("Screen width:", window.innerWidth);
console.log("Screen height:", window.innerHeight);

// 8. Comments example
/*
 This is a multi-line comment
 explaining the purpose of this script
 It demonstrates various JavaScript concepts
*/

console.log("=== Session 1 Complete ===");
```

**Complete index.html:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Session 1 - JavaScript Basics</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            max-width: 800px;
            margin: 0 auto;
            padding: 20px;
            background-color: #f5f5f5;
        }
        h1 {
            color: #333;
            text-align: center;
        }
        #output {
            background: white;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 2px 4px rgba(0,0,0,0.1);
            margin-top: 20px;
        }
        #output h2 {
            color: #1890ff;
            margin-top: 0;
        }
        #output p {
            line-height: 1.6;
        }
    </style>
</head>
<body>
    <h1>JavaScript Session 1</h1>
    <p>Open the browser console (F12) to see the JavaScript output.</p>
    <div id="output">
        <p>JavaScript output will appear here...</p>
    </div>
    <script src="script.js" defer></script>
</body>
</html>
```

---

## 📝 Review Questions

1. **What is JavaScript primarily used for?**
   - [ ] Database management
   - [ ] Web development
   - [ ] System programming
   - [ ] Mobile app development only

2. **What is the recommended way to include JavaScript in HTML?**
   - [ ] Inline JavaScript
   - [ ] Internal script tag
   - [ ] External file with defer
   - [ ] External file with async

3. **How do you open Chrome DevTools?**
   - [ ] F12 or Ctrl+Shift+I
   - [ ] Ctrl+Alt+Delete
   - [ ] Ctrl+S
   - [ ] Alt+F4

4. **What is the purpose of comments in code?**
   - [ ] To make code run faster
   - [ ] To explain "why" not "what"
   - [ ] To increase file size
   - [ ] To replace documentation

5. **Which console method is used for error messages?**
   - [ ] console.log()
   - [ ] console.info()
   - [ ] console.warn()
   - [ ] console.error()

6. **What is a bad practice in JavaScript?**
   - [ ] Using const for constants
   - [ ] Using var instead of let/const
   - [ ] Adding comments
   - [ ] Using console.log for debugging

7. **How do you style console output?**
   - [ ] console.style()
   - [ ] console.log("%c text", "css")
   - [ ] console.format()
   - [ ] console.css()

8. **What does the DOM represent?**
   - [ ] Database structure
   - [ ] Document Object Model
   - [ ] Data Object Management
   - [ ] Digital Output Module

9. **Which is NOT a valid console method?**
   - [ ] console.log()
   - [ ] console.table()
   - [ ] console.style()
   - [ ] console.assert()

10. **What is the benefit of using external JavaScript files?**
    - [ ] Slower loading
    - [ ] Better code organization and caching
    - [ ] More difficult to maintain
    - [ ] Can't be reused

### Correct Answers

1. ✅ Web development
2. ✅ External file with defer
3. ✅ F12 or Ctrl+Shift+I
4. ✅ To explain "why" not "what"
5. ✅ console.error()
6. ✅ Using var instead of let/const
7. ✅ console.log("%c text", "css")
8. ✅ Document Object Model
9. ✅ console.style()
10. ✅ Better code organization and caching

---

## 🎯 Next Steps

1. ✅ Set up your development environment (VS Code + extensions)
2. ✅ Create the project structure
3. ✅ Complete all console exercises
4. ✅ Practice DOM manipulation
5. ✅ Explore Chrome DevTools features
6. ✅ Review bad practices and avoid them in your code

---

## 📚 Additional Resources

- [MDN JavaScript Guide](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide)
- [JavaScript.info](https://javascript.info/)
- [Chrome DevTools Documentation](https://developer.chrome.com/docs/devtools/)
- [VS Code JavaScript Documentation](https://code.visualstudio.com/docs/javascript/javascript-tutorial)

**Remember:** Practice is essential for learning JavaScript. Open your browser console and experiment! 💪
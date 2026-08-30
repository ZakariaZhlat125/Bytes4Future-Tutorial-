# Session 1: Introduction to JavaScript

## 🎯 Session Overview
**Duration:** 2.5 hours  
**Level:** Absolute Beginner  
**Goal:** Set up environment, understand JavaScript basics, and build your first interactive web page

---

## 📋 Session Structure

### Part 1: Environment Setup (20 minutes)
- Problem: We need a place to write and run JavaScript
- Live Coding: Setting up VS Code and Chrome DevTools

### Part 2: Your First JavaScript Code (25 minutes)
- Problem: How do we make the browser do something?
- Interactive console exercises and DOM manipulation

### Part 3: Variables and Data Types (20 minutes)
- Problem: How do we store and use information?
- Live Coding with variables, errors, and challenges

### Part 4: Console Mastery (15 minutes)
- Problem: How do we see what's happening in our code?
- Advanced console techniques and debugging

### Part 5: Bug Hunting Challenge (20 minutes)
- Problem: Find and fix deliberate errors in code
- Individual and group bug hunting

### Part 6: Mini Project (30 minutes)
- Problem: Build a complete interactive feature
- Student-led development with instructor guidance

---

## 🔧 Part 1: Environment Setup (20 minutes)

### 🎯 Problem Statement
**Instructor:** "You want to become a web developer, but you have no tools to write code. Your computer can't understand JavaScript yet. We need to set up your development environment."

### 🤔 What Do You Expect?
**Instructor Question:** "If you write JavaScript code in a text editor like Notepad and try to open it, what do you think will happen?"

**[Let students guess - 1 minute]**

### 💡 Explanation
Your computer needs:
1. **A code editor** - A smart text editor that understands JavaScript syntax
2. **A web browser** - To run and test your JavaScript
3. **Developer tools** - To see what your code is doing

**Why Not Just Use Notepad?**
- No syntax highlighting (code looks plain)
- No error detection
- No autocomplete
- No debugging tools
- Hard to read and maintain

### 🎬 Live Coding: Setting Up VS Code

**Step 1: Install VS Code**
```bash
# Instructor demonstrates:
# 1. Go to https://code.visualstudio.com/
# 2. Download and install
# 3. Launch VS Code
```

**Step 2: Install Essential Extensions**
**Instructor:** "We need to add superpowers to VS Code. These extensions will help us write better code."

**Extensions to install:**
1. **ESLint** - Finds errors in your code
2. **Prettier** - Makes your code look pretty
3. **JavaScript (ES6) code snippets** - Quick code templates
4. **Live Server** - Auto-reloads your web page

**Live Coding:**
```
Instructor: "Watch me install these extensions. I'll go to Extensions (Ctrl+Shift+X), search for each one, and click Install."
```

### 🎬 Live Coding: Creating Your First Project

**Step 1: Create Project Structure**
```bash
# Instructor demonstrates in VS Code:
# 1. Create folder: my-javascript-course
# 2. Create subfolder: session1
# 3. Create files: index.html, script.js, style.css
```

**Step 2: Create index.html**
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

**Instructor Question:** "Why do you think I added `defer` to the script tag? What would happen without it?"

**[Let students guess - 30 seconds]**

**Explanation:** The `defer` attribute makes sure the HTML loads first before JavaScript runs. Without it, JavaScript might try to find elements that don't exist yet.

### 🎬 Live Coding: Opening Chrome DevTools

**Instructor:** "Now let's open the secret control panel for web developers."

**Methods to open DevTools:**
- **Windows/Linux:** `F12` or `Ctrl+Shift+I` or `Ctrl+Shift+J`
- **Mac:** `Cmd+Option+I` or `Cmd+Option+J`
- **Right-click** → "Inspect"

**Live Coding:**
```
Instructor: "I'll open index.html in Chrome, then press F12. Look at all these panels!"
```

**Key Panels to Show:**
1. **Elements** - See and change HTML/CSS
2. **Console** - Where JavaScript talks to us
3. **Sources** - Where we can debug code
4. **Network** - See what files are loading

### 🎯 Challenge 1: Setup Verification (5 minutes)
**Individual Challenge**

**📋 Requirements:**
- Create the exact folder structure shown
- Create the HTML file with the exact code
- Open it in Chrome
- Open DevTools and find the Console tab

**⏱️ Time Limit:** 5 minutes

**💡 Optional Hints:**
- Hint 1: Make sure your file extensions are correct (.html, .js, .css)
- Hint 2: Use the Live Server extension (right-click index.html → "Open with Live Server")

**🏆 Points:** 10 points for successful setup

**👨‍🏫 Instructor Solution:**
```bash
# Folder structure:
my-javascript-course/
└── session1/
    ├── index.html
    ├── script.js
    └── style.css

# Verify by:
# 1. Right-click index.html in VS Code
# 2. Select "Open with Live Server"
# 3. Press F12 in Chrome
# 4. Click Console tab
# 5. You should see an empty console (no errors)
```

---

## 💻 Part 2: Your First JavaScript Code (25 minutes)

### 🎯 Problem Statement
**Instructor:** "We have our environment ready. Now, how do we actually make the browser DO something? We want to display a message, but how?"

### 🤔 What Do You Expect?
**Instructor Question:** "If I write `Hello World` in a JavaScript file, what do you think will appear on the web page?"

**[Let students guess - 1 minute]**

**Answer:** Nothing! JavaScript doesn't automatically print to the web page. We need to tell it WHERE to display the message.

### 💡 Explanation
JavaScript has multiple ways to output information:

1. **console.log()** - Shows in browser console (invisible to normal users)
2. **alert()** - Shows a popup window
3. **DOM manipulation** - Changes the web page itself
4. **document.write()** - Writes directly to the page (old method, not recommended)

### 🎬 Live Coding: First JavaScript Statement

**Step 1: Add code to script.js**
```javascript
console.log("Hello, World!");
```

**Instructor:** "I'm adding this line to script.js. Now watch what happens when I refresh the page."

**Step 2: Refresh and check Console**
```
Instructor: "I'll refresh the page (F5) and look at the Console tab. Do you see 'Hello, World!'?"
```

### 🤔 What Do You Expect?
**Instructor Question:** "What do you think will happen if I write `console.log("Hello")` without the quotes?"

**[Let students guess - 30 seconds]**

**Live Coding:**
```javascript
console.log(Hello);  // No quotes
```

**Result:** Error! `Uncaught ReferenceError: Hello is not defined`

**Explanation:** Without quotes, JavaScript thinks `Hello` is a variable name, not text. We need quotes to tell JavaScript it's a string (text).

### 🎬 Live Coding: Different Output Methods

**Method 1: Console (for debugging)**
```javascript
console.log("This goes to the console");
console.info("This is information");
console.warn("This is a warning");
console.error("This is an error");
```

**Instructor:** "Each of these has a different style and icon in the console."

**Method 2: Alert (popup)**
```javascript
alert("Hello from a popup!");
```

**Instructor Question:** "When would you use alert vs console.log in a real website?"

**[Discuss - 1 minute]**

**Answer:** 
- `console.log()` - For debugging during development
- `alert()` - Almost never in production (annoying for users)

**Method 3: DOM Manipulation (changing the page)**
```javascript
const output = document.getElementById("output");
output.textContent = "Hello from JavaScript!";
```

**Instructor:** "This is how we actually change what users see on the page."

### 🎯 Challenge 2: Output Methods (8 minutes)
**Individual Challenge**

**📋 Requirements:**
1. Write a message using `console.log()`
2. Write a message using `alert()`
3. Write a message using DOM manipulation to the `#output` div
4. Make each message different

**⏱️ Time Limit:** 8 minutes

**💡 Optional Hints:**
- Hint 1: Don't forget to refresh the page after saving script.js
- Hint 2: For DOM manipulation, use `document.getElementById("output")`
- Hint 3: Use `textContent` to change text, `innerHTML` to change HTML

**🏆 Points:** 15 points for all three methods working

**👨‍🏫 Instructor Solution:**
```javascript
// Console output
console.log("Message in console");

// Alert popup
alert("Message in popup");

// DOM manipulation
const output = document.getElementById("output");
output.textContent = "Message on the page";
```

### 🎬 Live Coding: Template Literals

**Problem:** We want to combine text and variables.

**Old way (string concatenation):**
```javascript
let name = "John";
let age = 25;
console.log("My name is " + name + " and I am " + age + " years old");
```

**New way (template literals):**
```javascript
let name = "John";
let age = 25;
console.log(`My name is ${name} and I am ${age} years old`);
```

**Instructor:** "The backticks (`) and ${} make it much easier to read!"

### 🤔 What Do You Expect?
**Instructor Question:** "What do you think happens if I use single quotes instead of backticks with ${}?"

```javascript
let name = "John";
console.log('My name is ${name}');
```

**[Let students guess - 30 seconds]**

**Answer:** It literally prints `${name}` instead of the value. Template literals ONLY work with backticks.

### 🎯 Challenge 3: Personalized Greeting (7 minutes)
**Individual Challenge**

**📋 Requirements:**
- Create variables for your name, age, and favorite color
- Use template literals to create a personalized message
- Display the message using DOM manipulation
- Make it look nice with some HTML styling

**⏱️ Time Limit:** 7 minutes

**💡 Optional Hints:**
- Hint 1: Use `innerHTML` instead of `textContent` to add HTML tags
- Hint 2: You can add HTML like `<strong>`, `<em>`, or `<span style="color: red">`
- Hint 3: Template literals use backticks: `` ` ``

**🏆 Points:** 20 points for working personalized message with styling

**👨‍🏫 Instructor Solution:**
```javascript
let name = "John";
let age = 25;
let favoriteColor = "blue";

const output = document.getElementById("output");
output.innerHTML = `
    <h2>Hello, ${name}!</h2>
    <p>You are ${age} years old.</p>
    <p>Your favorite color is <span style="color: ${favoriteColor}; font-weight: bold;">${favoriteColor}</span>.</p>
`;
```

---

## 📊 Part 3: Variables and Data Types (20 minutes)

### 🎯 Problem Statement
**Instructor:** "We need to store information like user names, ages, scores, and settings. How do we keep track of all this data in our program?"

### 🤔 What Do You Expect?
**Instructor Question:** "If I write `x = 5` in JavaScript, what do you think happens? Can I later write `x = "hello"`?"

**[Let students guess - 1 minute]**

**Answer:** Yes! JavaScript is dynamically typed, meaning variables can change types.

### 💡 Explanation
**Variables** are like containers that hold data.

**Three ways to declare variables:**
1. **`let`** - Can change value (recommended for most cases)
2. **`const`** - Cannot change value (for constants)
3. **`var`** - Old way (avoid using this)

### 🎬 Live Coding: Variable Declaration

**Example 1: let (can change)**
```javascript
let score = 10;
console.log("Score:", score);
score = 20;
console.log("New score:", score);
```

**Example 2: const (cannot change)**
```javascript
const PI = 3.14159;
console.log("PI:", PI);
PI = 3.14;  // This will cause an error!
```

**Instructor:** "Watch what happens when I try to change a const."

**Result:** `TypeError: Assignment to constant variable.`

**Example 3: var (avoid this)**
```javascript
var name = "John";
console.log(name);
```

**Instructor:** "We'll learn why to avoid `var` later. For now, just use `let` and `const`."

### 🎬 Live Coding: Data Types

**Instructor:** "JavaScript can hold different types of data. Let's explore them all."

**Type 1: String (text)**
```javascript
let text = "Hello World";
let text2 = 'Single quotes work too';
let text3 = `Template literals`;
```

**Type 2: Number**
```javascript
let integer = 42;
let decimal = 3.14;
let negative = -10;
let scientific = 1.5e10;  // 15000000000
```

**Type 3: Boolean (true/false)**
```javascript
let isTrue = true;
let isFalse = false;
```

**Type 4: Undefined**
```javascript
let notDefined;
console.log(notDefined);  // undefined
```

**Type 5: Null**
```javascript
let empty = null;
console.log(empty);  // null
```

**Type 6: Object**
```javascript
let person = {
    name: "John",
    age: 30,
    city: "New York"
};
```

**Type 7: Array**
```javascript
let numbers = [1, 2, 3, 4, 5];
let fruits = ["apple", "banana", "orange"];
```

### 🤔 What Do You Expect?
**Instructor Question:** "What's the difference between `undefined` and `null`?"

```javascript
let a;
let b = null;
console.log(a);  // ?
console.log(b);  // ?
```

**[Let students guess - 1 minute]**

**Answer:** 
- `undefined` = variable exists but has no value assigned
- `null` = variable exists and explicitly has no value (intentionally empty)

### 🎬 Live Coding: Checking Data Types

**Instructor:** "We can check what type a variable is using `typeof`."

```javascript
let name = "John";
let age = 25;
let isStudent = true;
let nothing = null;
let notDefined;

console.log(typeof name);        // "string"
console.log(typeof age);         // "number"
console.log(typeof isStudent);   // "boolean"
console.log(typeof nothing);     // "object" (this is a JavaScript bug!)
console.log(typeof notDefined);  // "undefined"
```

**Instructor:** "Notice that `typeof null` returns 'object'. This is actually a bug in JavaScript that has existed since the first version!"

### 🎯 Challenge 4: Variable & Data Type Quiz (10 minutes)
**Individual Challenge**

**📋 Requirements:**
Create a script that:
1. Declares 5 different variables with different data types
2. Uses `let` for variables that might change
3. Uses `const` for variables that won't change
4. Uses `typeof` to check each variable's type
5. Displays all this information on the web page

**⏱️ Time Limit:** 10 minutes

**💡 Optional Hints:**
- Hint 1: Try to use all 7 data types we learned
- Hint 2: Make the output readable with HTML formatting
- Hint 3: Use template literals to display the type checks

**🏆 Points:** 25 points for all requirements met

**👨‍🏫 Instructor Solution:**
```javascript
// Different data types
let userName = "John";              // string
const MAX_SCORE = 100;             // number (constant)
let currentScore = 75;             // number
let isGameOver = false;            // boolean
let inventory = null;              // null
let settings;                      // undefined
let player = {                     // object
    level: 5,
    health: 100
};
let items = ["sword", "shield"];  // array

// Display on page
const output = document.getElementById("output");
output.innerHTML = `
    <h2>Data Type Demonstration</h2>
    <ul>
        <li>userName: ${userName} (type: ${typeof userName})</li>
        <li>MAX_SCORE: ${MAX_SCORE} (type: ${typeof MAX_SCORE})</li>
        <li>currentScore: ${currentScore} (type: ${typeof currentScore})</li>
        <li>isGameOver: ${isGameOver} (type: ${typeof isGameOver})</li>
        <li>inventory: ${inventory} (type: ${typeof inventory})</li>
        <li>settings: ${settings} (type: ${typeof settings})</li>
        <li>player: ${JSON.stringify(player)} (type: ${typeof player})</li>
        <li>items: ${JSON.stringify(items)} (type: ${typeof items})</li>
    </ul>
`;
```

---

## 🔍 Part 4: Console Mastery (15 minutes)

### 🎯 Problem Statement
**Instructor:** "Your code isn't working. You have no idea what's going wrong. How do you figure out what your code is actually doing?"

### 💡 Explanation
The console is your best friend for debugging. It has many powerful features beyond simple `console.log()`.

### 🎬 Live Coding: Advanced Console Methods

**Method 1: console.table()**
```javascript
let users = [
    { name: "John", age: 30, city: "NYC" },
    { name: "Jane", age: 25, city: "LA" },
    { name: "Bob", age: 35, city: "Chicago" }
];
console.table(users);
```

**Instructor:** "This displays data in a nice table format!"

**Method 2: console.group()**
```javascript
console.group("User Information");
console.log("Name: John");
console.log("Age: 30");
console.log("Email: john@example.com");
console.groupEnd();
```

**Instructor:** "This groups related logs together."

**Method 3: console.time() and console.timeEnd()**
```javascript
console.time("My operation");
// Some code that takes time
for (let i = 0; i < 1000; i++) {
    console.log(i);
}
console.timeEnd("My operation");
```

**Instructor:** "This measures how long code takes to run."

**Method 4: console.assert()**
```javascript
console.assert(2 + 2 === 4, "Math is broken!");
console.assert(2 + 2 === 5, "Math is broken!");
```

**Instructor:** "This only logs if the condition is false. The second one will show an error."

**Method 5: console.count()**
```javascript
console.count("Click");
console.count("Click");
console.count("Click");
console.countReset("Click");
```

**Instructor:** "This counts how many times something happens."

### 🎬 Live Coding: Console Styling

**Instructor:** "We can even style our console messages with CSS!"

```javascript
console.log("%cHello World!", "color: blue; font-size: 20px;");
console.log("%cError!", "color: red; font-weight: bold; font-size: 16px;");
console.log("%cSuccess!", "color: green; background: #e0ffe0; padding: 5px;");
```

**Instructor:** "The %c tells the console to apply CSS styling."

### 🤔 What Do You Expect?
**Instructor Question:** "Why would we want to style console messages?"

**[Let students guess - 30 seconds]**

**Answer:** To make important messages stand out during debugging - errors in red, warnings in yellow, success in green.

### 🎯 Challenge 5: Console Detective (8 minutes)
**Individual Challenge**

**📋 Requirements:**
Create a script that uses at least 4 different console methods:
1. Must use `console.table()` for some data
2. Must use `console.group()` for related information
3. Must use `console.time()` to measure something
4. Must use styled console output

**⏱️ Time Limit:** 8 minutes

**💡 Optional Hints:**
- Hint 1: Create an array of objects for the table
- Hint 2: Group information logically (e.g., user details, game stats)
- Hint 3: Time a loop or some operation
- Hint 4: Use different colors for different types of messages

**🏆 Points:** 20 points for all 4 methods used correctly

**👨‍🏫 Instructor Solution:**
```javascript
// Console table
let students = [
    { name: "Alice", score: 95, grade: "A" },
    { name: "Bob", score: 87, grade: "B" },
    { name: "Charlie", score: 92, grade: "A" }
];
console.table(students);

// Console group
console.group("Student Statistics");
console.log("Total students:", students.length);
console.log("Average score:", 91.3);
console.log("Highest grade:", "A");
console.groupEnd();

// Console time
console.time("Sorting operation");
students.sort((a, b) => b.score - a.score);
console.timeEnd("Sorting operation");

// Styled console
console.log("%c✓ Students processed successfully!", "color: green; font-weight: bold; background: #e0ffe0; padding: 5px;");
console.log("%c⚠ Check the data above", "color: orange; font-weight: bold; background: #fff3cd; padding: 5px;");
```

---

## 🐛 Part 5: Bug Hunting Challenge (20 minutes)

### 🎯 Problem Statement
**Instructor:** "I've written some JavaScript code, but it's full of bugs! Your job is to find and fix them."

### 🕵️ Bug Hunt 1: The Silent Error (5 minutes)
**Individual Challenge**

**📋 Buggy Code:**
```javascript
let message = "Hello World
console.log(message);
```

**🎯 Requirements:**
- Find the bug
- Fix it
- Explain what was wrong

**⏱️ Time Limit:** 5 minutes

**💡 Hints:**
- Hint 1: Look at the string carefully
- Hint 2: Strings need matching quotes

**🏆 Points:** 10 points

**👨‍🏫 Instructor Solution:**
```javascript
// BUG: Missing closing quote
let message = "Hello World";  // Added closing quote
console.log(message);
```

### 🕵️ Bug Hunt 2: The Mysterious Variable (5 minutes)
**Individual Challenge**

**📋 Buggy Code:**
```javascript
const userName = "John";
userName = "Jane";
console.log(userName);
```

**🎯 Requirements:**
- Find the bug
- Fix it
- Explain why it's a bug

**⏱️ Time Limit:** 5 minutes

**💡 Hints:**
- Hint 1: Look at how the variable is declared
- Hint 2: Can const variables be changed?

**🏆 Points:** 10 points

**👨‍🏫 Instructor Solution:**
```javascript
// BUG: Trying to reassign a const variable
// Solution 1: Change const to let
let userName = "John";
userName = "Jane";
console.log(userName);

// Solution 2: Use a new variable
const userName = "John";
const newUserName = "Jane";
console.log(newUserName);
```

### 🕵️ Bug Hunt 3: The Disappearing Act (5 minutes)
**Individual Challenge**

**📋 Buggy Code:**
```javascript
let firstName = "John";
let lastName = "Doe";
console.log("Full name: " + firstName + " " + lastName);
```

**Instructor:** "This code actually works! But there's a 'logic bug' - it's not the best way to do it. Can you improve it?"

**🎯 Requirements:**
- Identify the improvement opportunity
- Rewrite using better JavaScript practices
- Explain why your version is better

**⏱️ Time Limit:** 5 minutes

**💡 Hints:**
- Hint 1: Think about template literals
- Hint 2: Think about code readability

**🏆 Points:** 10 points

**👨‍🏫 Instructor Solution:**
```javascript
// IMPROVEMENT: Use template literals for better readability
let firstName = "John";
let lastName = "Doe";
console.log(`Full name: ${firstName} ${lastName}`);
```

### 🕵️ Bug Hunt 4: The Group Challenge (5 minutes)
**Group Challenge**

**📋 Buggy Code:**
```javascript
let scores = [10, 20, 30, 40, 50];
let total = 0;

for (let i = 0; i < scores.length; i++) {
    total = total + scores[i];
}

console.log("Average: " + total / scores.length);
```

**Instructor:** "This code works, but it has a common beginner mistake. Work in groups to find it."

**🎯 Requirements:**
- Find the inefficiency
- Rewrite using more modern JavaScript
- Explain the benefits

**⏱️ Time Limit:** 5 minutes

**💡 Hints:**
- Hint 1: Think about array methods
- Hint 2: Is there a simpler way to sum an array?

**🏆 Points:** 15 points for the group

**👨‍🏫 Instructor Solution:**
```javascript
// IMPROVEMENT: Use reduce() method
let scores = [10, 20, 30, 40, 50];
let total = scores.reduce((sum, score) => sum + score, 0);
console.log("Average: " + total / scores.length);
```

**Note:** This introduces `reduce()` which is advanced - acknowledge this and explain it's a preview of future topics.

---

## 🎨 Part 6: Mini Project (30 minutes)

### 🎯 Problem Statement
**Instructor:** "We've learned about variables, data types, console methods, and DOM manipulation. Now let's build something real!"

### 🏗️ Project: Interactive Student Profile Generator

**Scenario:** You're building a simple web application that lets users create a student profile card.

### 📋 Project Requirements

**Must Include:**
1. **User Input Collection**
   - Name (string)
   - Age (number)
   - Favorite subject (string)
   - Is currently studying (boolean)
   - Skills (array of strings)

2. **Data Validation**
   - Check if name is provided
   - Check if age is a reasonable number
   - Display appropriate error messages

3. **Profile Display**
   - Show all information in a nicely formatted card
   - Use different colors/styles for different data types
   - Include a "profile created" timestamp

4. **Console Logging**
   - Log the raw data using `console.table()`
   - Log profile creation using `console.time()`
   - Use styled console messages for success/errors

5. **Bonus Features** (extra points)
   - Allow updating the profile
   - Add a "clear profile" button
   - Save profile to localStorage

### 🎬 Live Coding: Project Setup

**Step 1: HTML Structure**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Student Profile Generator</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            max-width: 800px;
            margin: 0 auto;
            padding: 20px;
            background-color: #f5f5f5;
        }
        .profile-card {
            background: white;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
            margin-top: 20px;
        }
        .profile-card h2 {
            color: #1890ff;
            margin-top: 0;
        }
        .skill-tag {
            display: inline-block;
            background: #1890ff;
            color: white;
            padding: 5px 10px;
            border-radius: 15px;
            margin: 5px;
            font-size: 14px;
        }
        .error {
            color: red;
            background: #ffe0e0;
            padding: 10px;
            border-radius: 5px;
            margin: 10px 0;
        }
        .success {
            color: green;
            background: #e0ffe0;
            padding: 10px;
            border-radius: 5px;
            margin: 10px 0;
        }
    </style>
</head>
<body>
    <h1>Student Profile Generator</h1>
    
    <div id="input-section">
        <h2>Create Your Profile</h2>
        <input type="text" id="name" placeholder="Your name">
        <input type="number" id="age" placeholder="Your age">
        <input type="text" id="subject" placeholder="Favorite subject">
        <label>
            <input type="checkbox" id="studying"> Currently studying
        </label>
        <input type="text" id="skills" placeholder="Skills (comma separated)">
        <button onclick="createProfile()">Create Profile</button>
    </div>
    
    <div id="profile-output"></div>
    
    <script src="script.js" defer></script>
</body>
</html>
```

**Step 2: JavaScript Skeleton**
```javascript
// script.js

function createProfile() {
    console.log("=== Creating Profile ===");
    console.time("Profile Creation");
    
    // Get user input
    const name = document.getElementById("name").value;
    const age = document.getElementById("age").value;
    const subject = document.getElementById("subject").value;
    const isStudying = document.getElementById("studying").checked;
    const skillsInput = document.getElementById("skills").value;
    
    // Convert skills string to array
    const skills = skillsInput.split(",").map(skill => skill.trim());
    
    // Validation
    if (!name) {
        showError("Name is required!");
        return;
    }
    
    if (!age || age < 5 || age > 100) {
        showError("Please enter a valid age (5-100)");
        return;
    }
    
    // Create profile object
    const profile = {
        name: name,
        age: parseInt(age),
        favoriteSubject: subject,
        isStudying: isStudying,
        skills: skills,
        createdAt: new Date().toLocaleString()
    };
    
    // Log to console
    console.table([profile]);
    console.log("%c✓ Profile created successfully!", "color: green; font-weight: bold;");
    
    // Display on page
    displayProfile(profile);
    
    console.timeEnd("Profile Creation");
}

function displayProfile(profile) {
    const output = document.getElementById("profile-output");
    
    let skillsHTML = profile.skills.map(skill => 
        `<span class="skill-tag">${skill}</span>`
    ).join("");
    
    output.innerHTML = `
        <div class="profile-card">
            <h2>👤 ${profile.name}</h2>
            <p><strong>Age:</strong> ${profile.age}</p>
            <p><strong>Favorite Subject:</strong> ${profile.favoriteSubject}</p>
            <p><strong>Status:</strong> ${profile.isStudying ? "📚 Currently Studying" : "🎓 Not Studying"}</p>
            <p><strong>Skills:</strong></p>
            <div>${skillsHTML}</div>
            <p><strong>Created:</strong> ${profile.createdAt}</p>
        </div>
    `;
    
    showSuccess("Profile displayed successfully!");
}

function showError(message) {
    const output = document.getElementById("profile-output");
    output.innerHTML = `<div class="error">❌ ${message}</div>`;
    console.error(message);
}

function showSuccess(message) {
    const output = document.getElementById("profile-output");
    // Don't overwrite the profile if it exists
    if (!output.innerHTML.includes("profile-card")) {
        output.innerHTML += `<div class="success">✓ ${message}</div>`;
    }
    console.log(`%c✓ ${message}`, "color: green; font-weight: bold;");
}
```

### 🎯 Project Challenge (25 minutes)
**Individual Challenge with Peer Review**

**📋 Requirements:**
1. Implement the profile generator exactly as shown
2. Add at least one additional feature of your choice
3. Test with different inputs (valid and invalid)
4. Have a peer review your code

**⏱️ Time Limit:** 25 minutes

**💡 Optional Feature Ideas:**
- Add a "Clear Profile" button
- Add profile editing capability
- Save profiles to localStorage
- Add a profile picture (URL input)
- Add a graduation year calculator
- Add a "generate random profile" button

**🏆 Points:**
- 50 points for basic implementation
- 20 points for additional feature
- 10 points for peer review completed

**👨‍🏫 Instructor Notes:**
- Walk around and help students
- Encourage peer collaboration
- Show off interesting solutions at the end
- Discuss different approaches students took

---

## 🎬 Live Coding Review (10 minutes)

### 📝 Instructor Questions for Review

**1. Environment Setup:**
- "Why do we need a code editor instead of Notepad?"
- "What does the `defer` attribute do?"
- "How do we open Chrome DevTools?"

**2. JavaScript Basics:**
- "What's the difference between `let`, `const`, and `var`?"
- "Why do we use template literals instead of string concatenation?"
- "What's the difference between `undefined` and `null`?"

**3. Console Methods:**
- "When would you use `console.table()` instead of `console.log()`?"
- "Why do we style console messages?"
- "How can `console.time()` help us?"

**4. Debugging:**
- "What's the most common bug you found today?"
- "How do you approach fixing a bug?"
- "What's your favorite console method for debugging?"

**5. Project:**
- "What was the hardest part of the mini project?"
- "What feature did you add and why?"
- "How would you improve this project?"

---

## 🏆 Session Wrap-up

### ✅ What We Accomplished

1. **Set up a complete development environment**
   - VS Code with extensions
   - Chrome DevTools
   - Project structure

2. **Learned JavaScript fundamentals**
   - Variables (let, const)
   - Data types (string, number, boolean, null, undefined, object, array)
   - Output methods (console, DOM manipulation)

3. **Mastered console debugging**
   - Advanced console methods
   - Console styling
   - Performance measurement

4. **Practiced debugging**
   - Found and fixed common bugs
   - Learned best practices

5. **Built a real project**
   - Student profile generator
   - Form validation
   - DOM manipulation
   - Console integration

### 🎯 Key Takeaways

1. **Always use `let` and `const`** - Avoid `var`
2. **Use template literals** - They're more readable
3. **Master the console** - It's your best debugging tool
4. **Validate user input** - Don't trust what users enter
5. **Practice debugging** - It's a skill like any other

### 📚 Homework

**1. Practice Exercise (30 minutes)**
- Create a "To-Do List" application
- Use all the concepts we learned today
- Add console logging for debugging
- Try to add at least one extra feature

**2. Reading (15 minutes)**
- Read about JavaScript data types on MDN
- Explore Chrome DevTools documentation
- Look up JavaScript best practices

**3. Prepare for Next Session**
- Think about: "What if we need to make decisions in our code?"
- Research: "What are conditional statements in JavaScript?"

### 🌟 Points Leaderboard

**Session 1 Champion:** [To be filled in class]

**Top Performers:**
1. [Name] - [Points]
2. [Name] - [Points]
3. [Name] - [Points]

---

## 📖 Additional Resources

- [MDN JavaScript Guide](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide)
- [JavaScript.info](https://javascript.info/)
- [Chrome DevTools Documentation](https://developer.chrome.com/docs/devtools/)
- [VS Code JavaScript Documentation](https://code.visualstudio.com/docs/javascript/javascript-tutorial)

### 🎮 Practice Sites
- [Codecademy JavaScript](https://www.codecademy.com/learn/introduction-to-javascript)
- [freeCodeCamp JavaScript](https://www.freecodecamp.org/learn/javascript-algorithms-and-data-structures/)
- [JavaScript30](https://javascript30.com/)

---

## 💡 Tips for Success

1. **Code every day** - Even 15 minutes helps
2. **Don't copy-paste** - Type everything yourself
3. **Break things** - See what happens when you change code
4. **Use the console** - Log everything when learning
5. **Ask questions** - There's no such thing as a stupid question
6. **Teach others** - Explaining helps you learn
7. **Build projects** - Theory alone isn't enough

---

**Remember:** Every expert was once a beginner. Keep practicing! 💪

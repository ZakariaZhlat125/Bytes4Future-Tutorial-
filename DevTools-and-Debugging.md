# Session 13: DevTools and Debugging

## Duration Breakdown
- **1 Hour**: Theoretical Explanation + Live Coding
- **1.5 Hours**: Practical Application (Student writes code)
- **0.5 Hours**: Review, Questions, Problem Solving

---

## Part 1: Theoretical Explanation + Live Coding (1 Hour)

### Section 1: What are DevTools?

**Definition:**
DevTools (Developer Tools) are built-in browser tools that help developers debug, test, and optimize web applications.

**Accessing DevTools:**
- **Chrome/Edge**: F12 or Ctrl+Shift+I (Windows), Cmd+Option+I (Mac)
- **Firefox**: F12 or Ctrl+Shift+I (Windows), Cmd+Option+I (Mac)
- **Safari**: Cmd+Option+I (Mac, need to enable in preferences)

**Key Panels:**
- Elements
- Console
- Network
- Sources
- Performance
- Application
- Lighthouse

---

### Section 2: Elements Panel

**What is Elements Panel:**
Allows you to inspect and modify HTML and CSS in real-time.

**Features:**
- View and edit HTML structure
- Inspect CSS styles
- Modify styles temporarily
- View box model
- Check computed styles
- Access DOM nodes

**How to Use:**
1. Right-click element → "Inspect"
2. Or click element selector tool (Ctrl+Shift+C)
3. Click on page element

**Editing HTML:**
- Double-click element to edit
- Right-click → Edit as HTML
- Drag elements to reorder

**Editing CSS:**
- Click on style to edit
- Add new rules
- Toggle properties
- View computed values

**Box Model:**
- View padding, margin, border
- Visual representation
- Edit values directly

---

### Section 3: Console Panel

**What is Console Panel:**
Allows you to execute JavaScript and view logs/errors.

**Console Methods:**

## console.log()
General logging.

```javascript
console.log('Hello World');
console.log('Value:', variable);
console.log('Object:', { name: 'John' });
```

## console.error()
Error logging (red color).

```javascript
console.error('Something went wrong');
```

## console.warn()
Warning logging (yellow color).

```javascript
console.warn('This is a warning');
```

## console.info()
Info logging (blue color).

```javascript
console.info('Information message');
```

## console.table()
Display data as table.

```javascript
console.table([
    { name: 'John', age: 30 },
    { name: 'Jane', age: 25 }
]);
```

## console.group()
Group related logs.

```javascript
console.group('User Info');
console.log('Name: John');
console.log('Age: 30');
console.groupEnd();
```

## console.time() / console.timeEnd()
Measure execution time.

```javascript
console.time('Timer');
// Code to measure
console.timeEnd('Timer');
```

## console.assert()
Assertion logging.

```javascript
console.assert(x > 0, 'x should be positive');
```

---

### Section 4: Network Panel

**What is Network Panel:**
Monitors network requests and responses.

**Features:**
- View all network requests
- Inspect request/response headers
- Analyze load times
- Debug API calls
- View file sizes
- Filter requests

**Common Use Cases:**
- Debug API requests
- Check response data
- Measure performance
- Find failed requests
- Analyze bandwidth

**Request Types:**
- Document
- Stylesheet
- Script
- XHR/Fetch
- Image
- Font
- Other

**Status Codes:**
- 200: OK
- 201: Created
- 301: Moved Permanently
- 400: Bad Request
- 401: Unauthorized
- 403: Forbidden
- 404: Not Found
- 500: Internal Server Error

---

### Section 5: Sources Panel

**What is Sources Panel:**
Allows you to debug JavaScript code.

**Features:**
- View source files
- Set breakpoints
- Step through code
- Watch variables
- Call stack
- Scope variables

**Breakpoints:**
- Line breakpoints
- Conditional breakpoints
- DOM breakpoints
- XHR breakpoints
- Event listener breakpoints

**Setting Breakpoints:**
1. Open Sources panel
2. Select file
3. Click line number
4. Or right-click → Add breakpoint

**Debugging Controls:**
- Resume (F8)
- Step over (F10)
- Step into (F11)
- Step out (Shift+F11)

---

### Section 6: Breakpoints Explained

## Line Breakpoints
Pause execution at specific line.

```javascript
function calculateSum(a, b) {
    debugger; // Or set breakpoint here
    return a + b;
}
```

## Conditional Breakpoints
Pause only when condition is true.

```javascript
// Right-click breakpoint → Edit breakpoint
// Add condition: x > 10
```

## DOM Breakpoints
Pause when DOM changes.

**Types:**
- Subtree modifications
- Attribute modifications
- Node removal

## XHR Breakpoints
Pause when XHR request matches URL pattern.

## Event Listener Breakpoints
Pause when specific event fires.

---

### Section 7: Watch Expressions

**What are Watch Expressions:**
Variables to monitor while debugging.

**Adding Watch:**
1. Open Sources panel
2. In Watch section, click +
3. Enter variable/expression

**Examples:**
```javascript
x
x + y
array.length
object.property
```

---

### Section 8: Call Stack

**What is Call Stack:**
Shows the sequence of function calls that led to current point.

**How to Use:**
- View function call history
- Navigate to different frames
- Understand execution flow

---

### Section 9: Performance Panel

**What is Performance Panel:**
Records and analyzes runtime performance.

**How to Use:**
1. Click Record
2. Interact with page
3. Stop recording
4. Analyze results

**Metrics:**
- FPS (Frames Per Second)
- CPU usage
- Memory usage
- Network activity
- Rendering time

---

### Section 10: Lighthouse

**What is Lighthouse:**
Automated tool for improving web page quality.

**Categories:**
- Performance
- Accessibility
- Best Practices
- SEO
- PWA

**How to Run:**
1. Open DevTools
2. Go to Lighthouse panel
3. Select categories
4. Click "Analyze page load"

**Scores:**
- 0-49: Poor (Red)
- 50-89: Needs Improvement (Orange/Yellow)
- 90-100: Good (Green)

**Common Issues:**
- Large images
- Unoptimized JavaScript
- Missing alt text
- Poor contrast
- Slow server response

---

### Section 11: Application Panel

**What is Application Panel:**
View and manage storage and resources.

**Features:**
- Local Storage
- Session Storage
- Cookies
- IndexedDB
- Cache Storage
- Service Workers
- Manifest

**Use Cases:**
- Debug storage issues
- Clear cache
- View cookies
- Manage service workers
- Inspect PWA manifest

---

### Section 12: Common Debugging Techniques

## 1. console.log Debugging
```javascript
console.log('Variable:', variable);
console.log('Step 1 completed');
```

## 2. Breakpoint Debugging
Set breakpoints and step through code.

## 3. debugger Statement
```javascript
function myFunction() {
    debugger; // Pauses execution here
    // Code
}
```

## 4. Error Boundary (React)
```javascript
class ErrorBoundary extends React.Component {
    componentDidCatch(error, info) {
        console.error(error, info);
    }
}
```

## 5. Try-Catch
```javascript
try {
    // Code that might fail
} catch (error) {
    console.error('Error:', error);
}
```

---

## Part 2: Practical Application (1.5 Hours)

### Exercise 1: Elements Panel Practice (20 minutes)

**Task:**
Use Elements panel to inspect and modify a webpage.

**HTML:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>DevTools Practice</title>
    <style>
        .box {
            width: 200px;
            height: 200px;
            background-color: blue;
            padding: 20px;
            margin: 20px;
            border: 2px solid black;
        }
    </style>
</head>
<body>
    <div class="box">
        <h1>DevTools Practice</h1>
        <p>Inspect and modify this element</p>
    </div>
</body>
</html>
```

**Tasks:**
1. Open file in browser
2. Open DevTools (F12)
3. Inspect the .box element
4. Change background color to red
5. Change padding to 30px
6. Add border-radius: 10px
7. View box model
8. View computed styles
9. Edit HTML text
10. Reset changes (refresh page)

---

### Exercise 2: Console Practice (20 minutes)

**Task:**
Practice using different console methods.

**HTML:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Console Practice</title>
</head>
<body>
    <h1>Console Practice</h1>
    <button onclick="runConsoleMethods()">Run Console Methods</button>
    
    <script>
        function runConsoleMethods() {
            console.log('Log message');
            console.error('Error message');
            console.warn('Warning message');
            console.info('Info message');
            
            console.table([
                { name: 'John', age: 30 },
                { name: 'Jane', age: 25 }
            ]);
            
            console.group('User Group');
            console.log('Name: John');
            console.log('Age: 30');
            console.groupEnd();
            
            console.time('Timer');
            for (let i = 0; i < 1000; i++) {
                // Some work
            }
            console.timeEnd('Timer');
            
            const x = 5;
            console.assert(x > 10, 'x should be greater than 10');
        }
    </script>
</body>
</html>
```

**Tasks:**
1. Open file in browser
2. Open Console panel
3. Click button
4. Observe different console outputs
5. Understand color coding
6. View table format
7. Check timing
8. See assertion failure

---

### Exercise 3: Debugging with Breakpoints (30 minutes)

**Task:**
Debug JavaScript code with intentional bugs.

**HTML:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Debugging Practice</title>
</head>
<body>
    <h1>Debugging Practice</h1>
    <button onclick="calculate()">Calculate</button>
    <p id="result"></p>
    
    <script>
        function calculate() {
            const numbers = [1, 2, 3, 4, 5];
            let sum = 0;
            
            // Bug: Loop doesn't execute correctly
            for (let i = 0; i < numbers.length; i++) {
                sum += numbers[i];
            }
            
            // Bug: Wrong calculation
            const average = sum / numbers.length - 1;
            
            document.getElementById('result').textContent = 
                `Sum: ${sum}, Average: ${average}`;
        }
    </script>
</body>
</html>
```

**Tasks:**
1. Open file in browser
2. Open Sources panel
3. Set breakpoint on `calculate` function
4. Click button
5. Step through code
6. Watch variables
7. Identify bugs
5. Fix bugs in code
6. Test again

**Expected Fix:**
```javascript
// Fix 1: Loop is actually correct
// Fix 2: Average calculation
const average = sum / numbers.length; // Remove -1
```

---

### Exercise 4: Network Panel Practice (20 minutes)

**Task:**
Monitor network requests using Network panel.

**HTML:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Network Practice</title>
</head>
<body>
    <h1>Network Practice</h1>
    <button onclick="fetchData()">Fetch Data</button>
    <img src="https://via.placeholder.com/200" alt="Placeholder">
    
    <script>
        function fetchData() {
            fetch('https://jsonplaceholder.typicode.com/posts/1')
                .then(response => response.json())
                .then(data => console.log(data))
                .catch(error => console.error(error));
        }
    </script>
</body>
</html>
```

**Tasks:**
1. Open file in browser
2. Open Network panel
3. Refresh page
4. Observe all requests
5. Click "Fetch Data" button
6. Observe XHR request
7. Click on request to view details
8. Check response data
9. Check request headers
10. Check status code

---

### Exercise 5: Lighthouse Audit (20 minutes)

**Task:**
Run Lighthouse audit on a webpage.

**HTML:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Lighthouse Practice</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            padding: 20px;
        }
        img {
            max-width: 100%;
            height: auto;
        }
    </style>
</head>
<body>
    <h1>Lighthouse Practice Page</h1>
    <p>This page is for Lighthouse testing.</p>
    <img src="https://via.placeholder.com/1200x600" alt="Large Image">
    <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit.</p>
</body>
</html>
```

**Tasks:**
1. Open file in browser
2. Open DevTools
3. Go to Lighthouse panel
4. Select Performance, Accessibility, Best Practices, SEO
5. Click "Analyze page load"
6. Wait for analysis
7. Review scores
8. Read recommendations
9. Implement improvements
10. Re-run Lighthouse

---

## Part 3: Review, Questions, Problem Solving (0.5 Hours)

### Debugging Checklist (10 minutes)

**Before Debugging:**
- [ ] Reproduce the issue
- [ ] Understand expected behavior
- [ ] Check browser console for errors
- [ ] Verify HTML/CSS is correct

**During Debugging:**
- [ ] Use console.log strategically
- [ ] Set breakpoints at key points
- [ ] Watch important variables
- [ ] Step through code methodically
- [ ] Check data types and values

**After Debugging:**
- [ ] Verify fix works
- [ ] Test edge cases
- [ ] Remove debugging code
- [ ] Document the issue
- [ ] Prevent future occurrences

---

### Common Debugging Scenarios (10 minutes)

**Scenario 1: Element Not Appearing**
1. Check if element exists in DOM
2. Verify CSS display property
3. Check z-index
4. Verify parent elements
5. Check for JavaScript errors

**Scenario 2: Event Not Firing**
1. Check if element exists
2. Verify event listener is attached
3. Check for JavaScript errors
4. Verify event name spelling
5. Check if element is interactive

**Scenario 3: API Request Failing**
1. Check Network panel
4. Verify URL is correct
5. Check request headers
6. Check response status
7. Verify CORS settings

**Scenario 4: State Not Updating**
1. Check if re-render is triggered
2. Verify state mutation
3. Check for asynchronous issues
4. Verify event handlers
5. Check component lifecycle

---

### Review Questions (10 minutes)

**Question 1:** How do you open DevTools?
**Answer:** Press F12 or Ctrl+Shift+I (Windows), Cmd+Option+I (Mac).

**Question 2:** What is the difference between console.log and console.error?
**Answer:** console.log displays general messages, console.error displays error messages in red.

**Question 3:** How do you set a breakpoint?
**Answer:** Click the line number in Sources panel or use debugger statement.

**Question 4:** What does the Network panel show?
**Answer:** All network requests including status codes, headers, and response data.

**Question 5:** What is Lighthouse used for?
**Answer:** Automated auditing of web page quality (performance, accessibility, SEO, etc.).

**Question 6:** How do you edit CSS in DevTools?
**Answer:** Use Elements panel, click on style to edit, changes are temporary.

**Question 7:** What is the Call Stack?
**Answer:** Shows the sequence of function calls that led to the current execution point.

**Question 8:** How do you view localStorage?
**Answer:** Open Application panel → Local Storage.

---

### Practice Challenges (5 minutes)

**Challenge 1:** Use Elements panel to change the color of an element on a webpage.

**Challenge 2:** Add console.log statements to debug a function that's not working correctly.

**Challenge 3:** Run Lighthouse audit on a webpage and implement at least one improvement.

---

### Homework Assignment

**Task:** Debug a JavaScript file with intentional bugs.

**Requirements:**
- Use breakpoints to identify issues
- Use console methods for logging
- Use Network panel for API debugging
- Fix all bugs
- Document the debugging process
- Run Lighthouse and implement improvements

**Due Date:** End of course

---

## End of Session 13

## 🎉 Complete Course Completion!

**You have successfully completed the entire course!**

**Course Summary:**
- Sessions 1-8: CSS Fundamentals to Advanced
- Sessions 9-10: Tailwind CSS Fundamentals & Advanced
- Session 11: Git Fundamentals
- Session 12: Terminal & npm
- Session 13: DevTools and Debugging

**Final Achievement:**
You now have comprehensive knowledge of:
- CSS (Traditional + Tailwind)
- Version Control (Git)
- Development Tools (Terminal, npm, DevTools)

**Next Steps:**
- Build real-world projects
- Learn JavaScript frameworks (React, Vue, etc.)
- Explore backend development
- Stay updated with web technologies
- Contribute to open source
- Build your portfolio

**Happy Coding! 🚀**
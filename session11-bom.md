# Session 11: BOM (Browser Object Model)

## 📚 Theory (1h)

### What is the BOM

The Browser Object Model (BOM) allows JavaScript to interact with the browser. Unlike the DOM, which deals with the document content, the BOM deals with the browser window and its features.

#### BOM Components

- **Window Object**: Represents the browser window
- **Navigator Object**: Provides information about the browser
- **Screen Object**: Provides information about the user's screen
- **History Object**: Manages the browser's session history
- **Location Object**: Provides information about the current URL
- **Document Object**: Part of DOM, but accessible through BOM

#### Window Object

The `window` object is the global object in client-side JavaScript. All global variables and functions become properties of the window object.

```javascript
// Window properties
console.log(window.innerWidth);   // Browser window width
console.log(window.innerHeight);  // Browser window height
console.log(window.outerWidth);  // Total window width
console.log(window.outerHeight); // Total window height

// Window methods
window.alert("Hello!");         // Show alert
window.confirm("Are you sure?"); // Show confirmation
window.prompt("Enter name:");  // Show prompt
```

### alert/confirm/prompt

These are built-in methods that create simple modal dialogs.

#### alert()

Displays an alert box with a message and an OK button.

```javascript
alert("Hello, World!");
alert("This is an alert");
alert("Line 1\nLine 2"); // Supports newlines
```

**Usage:** Simple notifications, debugging, important warnings.

#### confirm()

Displays a dialog with a message, OK, and Cancel buttons. Returns `true` if OK is clicked, `false` if Cancel is clicked.

```javascript
const result = confirm("Are you sure you want to continue?");
if (result) {
    console.log("User clicked OK");
} else {
    console.log("User clicked Cancel");
}
```

**Usage:** Confirmations before destructive actions, user consent.

#### prompt()

Displays a dialog with a message, text input field, OK, and Cancel buttons. Returns the entered text if OK is clicked, or `null` if Cancel is clicked.

```javascript
const name = prompt("Please enter your name:");
if (name) {
    console.log("Hello, " + name);
} else {
    console.log("User cancelled or entered nothing");
}

// With default value
const age = prompt("Enter your age:", "25");
console.log("Age:", age);
```

**Usage:** User input, simple data entry, quick prompts.

### setTimeout/clearTimeout

`setTimeout` executes a function after a specified delay. `clearTimeout` cancels a scheduled timeout.

#### setTimeout()

```javascript
// Basic syntax
setTimeout(function() {
    console.log("Executed after 2 seconds");
}, 2000);

// With named function
function showMessage() {
    console.log("Hello!");
}
setTimeout(showMessage, 1000);

// With arrow function
setTimeout(() => {
    console.log("Arrow function executed");
}, 1500);

// With parameters
setTimeout((name) => {
    console.log("Hello, " + name);
}, 1000, "John");
```

#### clearTimeout()

```javascript
// Schedule a timeout
const timeoutId = setTimeout(() => {
    console.log("This will not execute");
}, 5000);

// Cancel the timeout
clearTimeout(timeoutId);
console.log("Timeout cancelled");
```

**Use Cases:**
- Delayed execution
- Animations
- Debouncing
- API retry logic

### setInterval/clearInterval

`setInterval` executes a function repeatedly at specified intervals. `clearInterval` cancels a scheduled interval.

#### setInterval()

```javascript
// Basic syntax
setInterval(function() {
    console.log("Executed every second");
}, 1000);

// With named function
function tick() {
    console.log("Tick");
}
setInterval(tick, 1000);

// With arrow function
setInterval(() => {
    console.log("Arrow function executed");
}, 500);

// With parameters
setInterval((count) => {
    console.log("Count:", count);
}, 1000, 0); // Note: setInterval doesn't pass parameters like setTimeout
```

#### clearInterval()

```javascript
// Schedule an interval
const intervalId = setInterval(() => {
    console.log("This will stop after 5 seconds");
}, 1000);

// Cancel after 5 seconds
setTimeout(() => {
    clearInterval(intervalId);
    console.log("Interval stopped");
}, 5000);
```

**Use Cases:**
- Real-time updates
- Animations
- Polling
- Countdown timers
- Games

---

## 💻 Practical (1.5h)

### Exercise 1: alert/confirm/prompt

```javascript
// Exercise 1.1: Basic alert
console.log("=== Basic Alert ===");

function showAlert(message) {
    alert(message);
}

showAlert("Hello, World!");
showAlert("This is an alert dialog");

// Exercise 1.2: Confirm dialogs
console.log("\n=== Confirm Dialog ===");

function confirmAction(action) {
    const result = confirm(`Are you sure you want to ${action}?`);
    if (result) {
        console.log(`${action} confirmed`);
        return true;
    } else {
        console.log(`${action} cancelled`);
        return false;
    }
}

confirmAction("delete this file");
confirmAction("submit the form");

// Exercise 1.3: Prompt dialogs
console.log("\n=== Prompt Dialog ===");

function getUserInput(promptText, defaultValue) {
    const input = prompt(promptText, defaultValue);
    if (input !== null) {
        console.log("User entered:", input);
        return input;
    }
    console.log("User cancelled");
    return null;
}

const name = getUserInput("Enter your name:", "John");
const age = getUserInput("Enter your age:", "25");

// Exercise 1.4: Combined usage
console.log("\n=== Combined Usage ===");

function deleteUser() {
    const confirmDelete = confirm("Are you sure you want to delete this item?");
    if (confirmDelete) {
        const confirmAgain = confirm("This action cannot be undone. Continue?");
        if (confirmAgain) {
            const password = prompt("Enter your password to confirm:");
            if (password) {
                console.log("Item deleted with password:", password);
            } else {
                console.log("Deletion cancelled - no password provided");
            }
        } else {
            console.log("Deletion cancelled");
        }
    } else {
        console.log("Deletion cancelled");
    }
}

deleteUser();
```

### Exercise 2: setTimeout/clearTimeout

```javascript
// Exercise 2.1: Basic setTimeout
console.log("=== Basic setTimeout ===");

console.log("Start");
setTimeout(() => {
    console.log("Executed after 2 seconds");
}, 2000);
console.log("End");

// Exercise 2.2: setTimeout with parameters
console.log("\n=== setTimeout with Parameters ===");

function greet(name, greeting) {
    console.log(`${greeting}, ${name}!`);
}

setTimeout(greet, 1000, "John", "Hello");
setTimeout(greet, 1500, "Jane", "Hi");

// Exercise 2.3: Multiple timeouts
console.log("\n=== Multiple Timeouts ===");

const timeouts = [];

for (let i = 1; i <= 5; i++) {
    const timeoutId = setTimeout((num) => {
        console.log(`Timeout ${num} executed`);
    }, i * 1000, i);
    timeouts.push(timeoutId);
}

// Exercise 2.4: clearTimeout
console.log("\n=== clearTimeout ===");

const shortTimeout = setTimeout(() => {
    console.log("This won't execute");
}, 5000);

console.log("Timeout scheduled");

// Cancel after 1 second
setTimeout(() => {
    clearTimeout(shortTimeout);
    console.log("Timeout cancelled");
}, 1000);

// Exercise 2.5: Chained timeouts
console.log("\n=== Chained Timeouts ===");

function runSequence() {
    setTimeout(() => {
        console.log("Step 1");
        setTimeout(() => {
            console.log("Step 2");
            setTimeout(() => {
                console.log("Step 3");
            }, 1000);
        }, 1000);
    }, 1000);
}

runSequence();
```

### Exercise 3: setInterval/clearInterval

```javascript
// Exercise 3.1: Basic setInterval
console.log("=== Basic setInterval ===");

let counter = 0;
const intervalId = setInterval(() => {
    counter++;
    console.log("Counter:", counter);
    
    if (counter >= 5) {
        clearInterval(intervalId);
        console.log("Interval stopped");
    }
}, 1000);

// Exercise 3.2: setInterval with parameters
console.log("\n=== setInterval with Parameters ===");

function displayMessage(message, count) {
    console.log(`${message} (${count})`);
}

let count = 0;
const msgInterval = setInterval(() => {
    count++;
    displayMessage("Hello", count);
}, 500);

// Stop after 3 seconds
setTimeout(() => {
    clearInterval(msgInterval);
}, 3000);

// Exercise 3.3: Digital clock
console.log("\n=== Digital Clock ===");

function startClock() {
    const clockInterval = setInterval(() => {
        const now = new Date();
        console.log(now.toLocaleTimeString());
    }, 1000);
    
    // Return interval ID so it can be stopped
    return clockInterval;
}

const clockId = startClock();

// Stop clock after 10 seconds
setTimeout(() => {
    clearInterval(clockId);
    console.log("Clock stopped");
}, 10000);

// Exercise 3.4: Countdown timer
console.log("\n=== Countdown Timer ===");

function startCountdown(seconds) {
    let remaining = seconds;
    
    const countdownInterval = setInterval(() => {
        console.log(`Time remaining: ${remaining} seconds`);
        remaining--;
        
        if (remaining < 0) {
            clearInterval(countdownInterval);
            console.log("Time's up!");
        }
    }, 1000);
}

startCountdown(5);
```

### Exercise 4: Window Location Object

```javascript
// Exercise 4.1: Location properties
console.log("=== Location Properties ===");

console.log("Full URL:", window.location.href);
console.log("Protocol:", window.location.protocol);
console.log("Host:", window.location.host);
console.log("Hostname:", window.location.hostname);
console.log("Port:", window.location.port);
console.log("Pathname:", window.location.pathname);
console.log("Search:", window.location.search);
console.log("Hash:", window.location.hash);

// Exercise 4.2: Navigation methods
console.log("\n=== Navigation Methods ===");

// Reload page
// window.location.reload();

// Navigate to new URL
// window.location.href = "https://example.com";

// Assign new URL
// window.location.assign("https://example.com");

// Replace current URL (no history entry)
// window.location.replace("https://example.com");

// Exercise 4.3: URL manipulation
console.log("\n=== URL Manipulation ===");

function updateURLParam(param, value) {
    const url = new URL(window.location.href);
    url.searchParams.set(param, value);
    window.history.pushState({}, "", url);
}

// updateURLParam("page", "2");

// Exercise 4.4: URL parsing
console.log("\n=== URL Parsing ===");

function parseURL(url) {
    const urlObj = new URL(url);
    
    return {
        protocol: urlObj.protocol,
        hostname: urlObj.hostname,
        port: urlObj.port,
        pathname: urlObj.pathname,
        search: urlObj.search,
        hash: urlObj.hash,
        params: Object.fromEntries(urlObj.searchParams)
    };
}

const parsed = parseURL("https://example.com:8080/path?param1=value1&param2=value2#section");
console.log("Parsed URL:", parsed);
```

### Exercise 5: Window Open/Close

```javascript
// Exercise 5.1: Opening windows
console.log("=== Opening Windows ===");

// Open new window
const newWindow = window.open("https://example.com", "_blank", "width=600,height=400");

console.log("New window:", newWindow);

// Exercise 5.2: Window features
console.log("\n=== Window Features ===");

const features = "width=800,height=600,menubar=no,toolbar=no,location=no,status=no";
const customWindow = window.open("", "MyWindow", features);

if (customWindow) {
    customWindow.document.write("<h1>Custom Window</h1>");
    customWindow.document.write("<p>This window was opened with custom features</p>");
}

// Exercise 5.3: Closing windows
console.log("\n=== Closing Windows ===");

// Close current window (usually blocked by browsers)
// window.close();

// Close opened window
if (newWindow && !newWindow.closed) {
    setTimeout(() => {
        newWindow.close();
        console.log("Window closed");
    }, 3000);
}

// Exercise 5.4: Window communication
console.log("\n=== Window Communication ===");

const mainWindow = window;
const childWindow = window.open("", "child", "width=400,height=300");

if (childWindow) {
    childWindow.document.write("<button onclick='window.opener.postMessage(\"Hello from child\", \"*\")'>Send Message</button>");
    childWindow.document.write("<div id='message'></div>");
    
    window.addEventListener("message", function(event) {
        console.log("Message from child:", event.data);
        childWindow.document.getElementById("message").textContent = "Message received: " + event.data;
    });
}
```

### Exercise 6: Window History

```javascript
// Exercise 6.1: History navigation
console.log("=== History Navigation ===");

console.log("History length:", window.history.length);
console.log("Current state:", window.history.state);

// Go back
// window.history.back();

// Go forward
// window.history.forward();

// Go to specific history entry
// window.history.go(-2); // Go back 2 pages
// window.history.go(2);  // Go forward 2 pages

// Exercise 6.2: History API
console.log("\n=== History API ===");

// Push state
window.history.pushState({ page: 1 }, "Page 1", "?page=1");
console.log("State pushed");

// Replace state
window.history.replaceState({ page: 2 }, "Page 2", "?page=2");
console.log("State replaced");

// Exercise 6.3: Handling history changes
console.log("\n=== History Changes ===");

window.addEventListener("popstate", function(event) {
    console.log("State changed:", event.state);
});

// Exercise 6.4: Building navigation
console.log("\n=== Building Navigation ===");

function navigateTo(page) {
    const state = { page: page, timestamp: Date.now() };
    const url = `?page=${page}`;
    
    window.history.pushState(state, `Page ${page}`, url);
    console.log("Navigated to:", page);
}

// navigateTo(1);
// navigateTo(2);
// navigateTo(3);

// Exercise 6.5: History state management
console.log("\n=== History State Management ===");

const historyStack = [];

function saveState(data) {
    historyStack.push(data);
    window.history.pushState(data, JSON.stringify(data));
    console.log("State saved:", data);
}

function restoreState() {
    if (historyStack.length > 0) {
        const state = historyStack[historyStack.length - 1];
        console.log("Restored state:", state);
    }
}
```

### Exercise 7: Scroll Methods

```javascript
// Exercise 7.1: Basic scroll methods
console.log("=== Basic Scroll Methods ===");

// Scroll to specific position
// window.scrollTo(0, 500);

// Scroll by offset
// window.scrollBy(0, 100);

// Scroll element into view
// document.getElementById("myElement").scrollIntoView();

// Exercise 7.2: Scroll properties
console.log("\n=== Scroll Properties ===");

console.log("Scroll X:", window.scrollX);
console.log("Scroll Y:", window.scrollY);
console.log("Page X offset:", window.pageXOffset);
console.log("Page Y offset:", window.pageYOffset);

// Exercise 7.3: Smooth scrolling
console.log("\n=== Smooth Scrolling ===");

// Smooth scroll to position
window.scrollTo({
    top: 500,
    behavior: "smooth"
});

// Smooth scroll by offset
window.scrollBy({
    top: 100,
    behavior: "smooth"
});

// Exercise 7.4: Element scrolling
console.log("\n=== Element Scrolling ===");

const element = document.getElementById("scrollContainer");

if (element) {
    // Scroll element
    element.scrollTop = 100;
    element.scrollLeft = 50;
    
    // Scroll to position
    element.scrollTo({
        top: 200,
        left: 100,
        behavior: "smooth"
    });
    
    // Scroll by offset
    element.scrollBy({
        top: 50,
        left: 25,
        behavior: "smooth"
    });
}

// Exercise 7.5: Scroll to element
console.log("\n=== Scroll to Element ===");

function scrollToElement(elementId) {
    const element = document.getElementById(elementId);
    if (element) {
        element.scrollIntoView({ behavior: "smooth", block: "start" });
    }
}

// scrollToElement("targetElement");
```

### Exercise 8: Scroll-to-Top Practice

```javascript
// Exercise 8.1: Basic scroll to top
console.log("=== Basic Scroll to Top ===");

function scrollToTop() {
    window.scrollTo({
        top: 0,
        behavior: "smooth"
    });
}

// scrollToTop();

// Exercise 8.2: Button scroll to top
console.log("\n=== Button Scroll to Top ===");

function createScrollToTopButton() {
    const button = document.createElement("button");
    button.textContent = "↑ Scroll to Top";
    button.style.position = "fixed";
    button.style.bottom = "20px";
    button.style.right = "20px";
    button.style.padding = "10px 20px";
    button.style.backgroundColor = "#1890ff";
    button.style.color = "white";
    button.style.border = "none";
    button.style.borderRadius = "5px";
    button.style.cursor = "pointer";
    button.style.display = "none";
    
    button.addEventListener("click", scrollToTop);
    
    // Show/hide based on scroll position
    window.addEventListener("scroll", function() {
        if (window.scrollY > 300) {
            button.style.display = "block";
        } else {
            button.style.display = "none";
        }
    });
    
    document.body.appendChild(button);
}

// createScrollToTopButton();

// Exercise 8.3: Progress indicator
console.log("\n=== Scroll Progress Indicator ===");

function createScrollIndicator() {
    const progressBar = document.createElement("div");
    progressBar.style.position = "fixed";
    progressBar.style.top = "0";
    progressBar.style.left = "0";
    progressBar.style.height = "3px";
    progressBar.style.backgroundColor = "#1890ff";
    progressBar.style.width = "0%";
    progressBar.style.transition = "width 0.1s";
    
    window.addEventListener("scroll", function() {
        const scrollTop = window.scrollY;
        const docHeight = document.documentElement.scrollHeight - window.innerHeight;
        const scrollPercent = (scrollTop / docHeight) * 100;
        
        progressBar.style.width = scrollPercent + "%";
    });
    
    document.body.appendChild(progressBar);
}

// createScrollIndicator();

// Exercise 8.4: Section scroll navigation
console.log("\n=== Section Scroll Navigation ===");

function createSectionNav() {
    const sections = document.querySelectorAll("section[id]");
    const nav = document.createElement("nav");
    nav.style.position = "fixed";
    nav.style.top = "20px";
    nav.style.right = "20px";
    nav.style.backgroundColor = "white";
    nav.style.padding = "10px";
    nav.style.borderRadius = "5px";
    nav.style.boxShadow = "0 2px 10px rgba(0,0,0,0.1)";
    
    sections.forEach(section => {
        const link = document.createElement("a");
        link.href = "#" + section.id;
        link.textContent = section.id;
        link.style.display = "block";
        link.style.padding = "5px 0";
        link.style.textDecoration = "none";
        link.style.color = "#333";
        
        link.addEventListener("click", function(e) {
            e.preventDefault();
            section.scrollIntoView({ behavior: "smooth" });
        });
        
        nav.appendChild(link);
    });
    
    document.body.appendChild(nav);
}

// createSectionNav();

// Exercise 8.5: Parallax scroll effect
console.log("\n=== Parallax Scroll Effect ===");

function createParallaxEffect() {
    window.addEventListener("scroll", function() {
        const scrolled = window.scrollY;
        const parallaxElements = document.querySelectorAll(".parallax");
        
        parallaxElements.forEach(element => {
            const speed = element.dataset.speed || 0.5;
            element.style.transform = `translateY(${scrolled * speed}px)`;
        });
    });
}

// createParallaxEffect();
```

### Exercise 9: localStorage + Color App Practice

```javascript
// Exercise 9.1: Basic localStorage operations
console.log("=== Basic localStorage ===");

// Save data
localStorage.setItem("username", "John");
localStorage.setItem("age", "30");

// Retrieve data
const username = localStorage.getItem("username");
const age = localStorage.getItem("age");

console.log("Username:", username);
console.log("Age:", age);

// Remove data
localStorage.removeItem("age");

// Clear all data
// localStorage.clear();

// Exercise 9.2: Storing objects
console.log("\n=== Storing Objects ===");

const user = {
    name: "John",
    email: "john@example.com",
    preferences: {
        theme: "dark",
        language: "en"
    }
};

// Store object (must be stringified)
localStorage.setItem("user", JSON.stringify(user));

// Retrieve object
const storedUser = JSON.parse(localStorage.getItem("user"));
console.log("Stored user:", storedUser);

// Exercise 9.3: Color app
console.log("\n=== Color App ===");

class ColorApp {
    constructor() {
        this.colors = ["#ff0000", "#00ff00", "#0000ff", "#ffff00", "#ff00ff", "#00ffff"];
        this.loadSettings();
    }
    
    loadSettings() {
        const savedColor = localStorage.getItem("backgroundColor");
        if (savedColor) {
            this.setBackgroundColor(savedColor);
        }
    }
    
    saveSettings(color) {
        localStorage.setItem("backgroundColor", color);
    }
    
    setBackgroundColor(color) {
        document.body.style.backgroundColor = color;
        this.saveSettings(color);
    }
    
    getRandomColor() {
        return this.colors[Math.floor(Math.random() * this.colors.length)];
    }
    
    createColorPalette() {
        const palette = document.createElement("div");
        palette.style.position = "fixed";
        palette.style.top = "20px";
        palette.style.left = "20px";
        palette.style.padding = "10px";
        palette.style.backgroundColor = "white";
        palette.style.borderRadius = "5px";
        palette.style.boxShadow = "0 2px 10px rgba(0,0,0,0.1)";
        
        this.colors.forEach(color => {
            const colorBox = document.createElement("div");
            colorBox.style.width = "30px";
            colorBox.style.height = "30px";
            colorBox.style.backgroundColor = color;
            colorBox.style.margin = "5px";
            colorBox.style.cursor = "pointer";
            colorBox.style.display = "inline-block";
            colorBox.style.border = "2px solid #ddd";
            
            colorBox.addEventListener("click", () => {
                this.setBackgroundColor(color);
            });
            
            palette.appendChild(colorBox);
        });
        
        const randomButton = document.createElement("button");
        randomButton.textContent = "Random Color";
        randomButton.style.marginTop = "10px";
        randomButton.style.padding = "5px 10px";
        randomButton.addEventListener("click", () => {
            this.setBackgroundColor(this.getRandomColor());
        });
        
        palette.appendChild(randomButton);
        document.body.appendChild(palette);
    }
}

const colorApp = new ColorApp();
colorApp.createColorPalette();

// Exercise 9.4: Advanced color app with themes
console.log("\n=== Advanced Color App ===");

class AdvancedColorApp {
    constructor() {
        this.themes = {
            light: {
                background: "#ffffff",
                text: "#333333",
                accent: "#1890ff"
            },
            dark: {
                background: "#1a1a1a",
                text: "#ffffff",
                accent: "#52c41a"
            },
            blue: {
                background: "#e6f7ff",
                text: "#003a8c",
                accent: "#1890ff"
            }
        };
        this.loadTheme();
    }
    
    loadTheme() {
        const savedTheme = localStorage.getItem("theme");
        if (savedTheme && this.themes[savedTheme]) {
            this.applyTheme(savedTheme);
        }
    }
    
    saveTheme(themeName) {
        localStorage.setItem("theme", themeName);
    }
    
    applyTheme(themeName) {
        const theme = this.themes[themeName];
        if (theme) {
            document.body.style.backgroundColor = theme.background;
            document.body.style.color = theme.text;
            this.saveTheme(themeName);
        }
    }
    
    createThemeSwitcher() {
        const switcher = document.createElement("div");
        switcher.style.position = "fixed";
        switcher.style.top = "20px";
        switcher.style.right = "20px";
        switcher.style.padding = "10px";
        switcher.style.backgroundColor = "white";
        switcher.style.borderRadius = "5px";
        switcher.style.boxShadow = "0 2px 10px rgba(0,0,0,0.1)";
        
        Object.keys(this.themes).forEach(themeName => {
            const button = document.createElement("button");
            button.textContent = themeName.charAt(0).toUpperCase() + themeName.slice(1);
            button.style.margin = "5px";
            button.style.padding = "5px 10px";
            button.style.cursor = "pointer";
            
            button.addEventListener("click", () => {
                this.applyTheme(themeName);
            });
            
            switcher.appendChild(button);
        });
        
        document.body.appendChild(switcher);
    }
}

const advancedColorApp = new AdvancedColorApp();
advancedColorApp.createThemeSwitcher();
```

### Exercise 10: sessionStorage

```javascript
// Exercise 10.1: Basic sessionStorage operations
console.log("=== Basic sessionStorage ===");

// Save data
sessionStorage.setItem("sessionData", "This is session data");
sessionStorage.setItem("tempValue", "123");

// Retrieve data
const sessionData = sessionStorage.getItem("sessionData");
const tempValue = sessionStorage.getItem("tempValue");

console.log("Session data:", sessionData);
console.log("Temp value:", tempValue);

// Exercise 10.2: localStorage vs sessionStorage
console.log("\n=== localStorage vs sessionStorage ===");

function compareStorage() {
    localStorage.setItem("local", "I persist");
    sessionStorage.setItem("session", "I expire on close");
    
    console.log("localStorage:", localStorage.getItem("local"));
    console.log("sessionStorage:", sessionStorage.getItem("session"));
    
    console.log("localStorage persists after tab close");
    console.log("sessionStorage clears on tab close");
}

compareStorage();

// Exercise 10.3: Form data persistence
console.log("\n=== Form Data Persistence ===");

class FormPersistence {
    constructor(formId) {
        this.form = document.getElementById(formId);
        this.storageKey = "formData_" + formId;
        this.loadFormData();
        this.setupListeners();
    }
    
    saveFormData() {
        const formData = new FormData(this.form);
        const data = {};
        
        formData.forEach((value, key) => {
            data[key] = value;
        });
        
        sessionStorage.setItem(this.storageKey, JSON.stringify(data));
    }
    
    loadFormData() {
        const savedData = sessionStorage.getItem(this.storageKey);
        if (savedData) {
            const data = JSON.parse(savedData);
            
            Object.keys(data).forEach(key => {
                const input = this.form.querySelector(`[name="${key}"]`);
                if (input) {
                    input.value = data[key];
                }
            });
        }
    }
    
    clearFormData() {
        sessionStorage.removeItem(this.storageKey);
        this.form.reset();
    }
    
    setupListeners() {
        this.form.addEventListener("input", () => this.saveFormData());
        this.form.addEventListener("submit", () => this.clearFormData());
    }
}

// Usage (requires form in HTML)
// const formPersistence = new FormPersistence("myForm");

// Exercise 10.4: Shopping cart with sessionStorage
console.log("\n=== Shopping Cart with sessionStorage ===");

class SessionCart {
    constructor() {
        this.cart = [];
        this.loadCart();
    }
    
    loadCart() {
        const savedCart = sessionStorage.getItem("cart");
        if (savedCart) {
            this.cart = JSON.parse(savedCart);
        }
    }
    
    saveCart() {
        sessionStorage.setItem("cart", JSON.stringify(this.cart));
    }
    
    addItem(item) {
        this.cart.push(item);
        this.saveCart();
    }
    
    removeItem(index) {
        this.cart.splice(index, 1);
        this.saveCart();
    }
    
    clearCart() {
        this.cart = [];
        sessionStorage.removeItem("cart");
    }
    
    getTotal() {
        return this.cart.reduce((sum, item) => sum + item.price, 0);
    }
    
    getItemCount() {
        return this.cart.length;
    }
}

const sessionCart = new SessionCart();
sessionCart.addItem({ name: "Product 1", price: 10 });
sessionCart.addItem({ name: "Product 2", price: 20 });
console.log("Cart total:", sessionCart.getTotal());
console.log("Cart count:", sessionCart.getItemCount());
```

### Exercise 11: Complete Working Example

**Complete script.js:**
```javascript
// Session 11: BOM (Browser Object Model)
// This script demonstrates BOM APIs and browser interactions

console.log("=== Session 11: BOM ===");

// 1. Window object
console.log("\n--- Window Object ---");
console.log("Window width:", window.innerWidth);
console.log("Window height:", window.innerHeight);

// 2. BOM dialogs
console.log("\n--- BOM Dialogs ---");
// alert("Hello, World!");
// const confirmed = confirm("Are you sure?");
// const name = prompt("Enter your name:");

// 3. Timers
console.log("\n--- Timers ---");
setTimeout(() => console.log("Delayed execution"), 2000);

let counter = 0;
const intervalId = setInterval(() => {
    counter++;
    console.log("Interval tick:", counter);
    if (counter >= 3) {
        clearInterval(intervalId);
    }
}, 1000);

// 4. Location
console.log("\n--- Location ---");
console.log("Current URL:", window.location.href);
console.log("Path:", window.location.pathname);

// 5. History
console.log("\n--- History ---");
console.log("History length:", window.history.length);

// 6. Storage
console.log("\n--- Storage ---");
localStorage.setItem("test", "value");
console.log("Stored value:", localStorage.getItem("test"));
localStorage.removeItem("test");

sessionStorage.setItem("sessionTest", "sessionValue");
console.log("Session value:", sessionStorage.getItem("sessionTest"));

console.log("\n=== Session 11 Complete ===");
```

**Complete index.html:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Session 11 - BOM</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            max-width: 1200px;
            margin: 0 auto;
            padding: 20px;
            background-color: #f5f5f5;
            transition: background-color 0.3s;
        }
        h1 {
            color: #333;
            text-align: center;
        }
        .section {
            background: white;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 2px 4px rgba(0,0,0,0.1);
            margin-bottom: 20px;
        }
        .section h2 {
            color: #1890ff;
            margin-top: 0;
        }
        .demo {
            background: #f9f9f9;
            padding: 15px;
            border-radius: 4px;
            margin-top: 10px;
        }
        .demo button {
            padding: 8px 16px;
            margin: 5px;
            background-color: #1890ff;
            color: white;
            border: none;
            border-radius: 4px;
            cursor: pointer;
        }
        .demo button:hover {
            background-color: #0c7cd5;
        }
        .demo input {
            padding: 8px;
            margin: 5px;
            border: 1px solid #ddd;
            border-radius: 4px;
        }
        .output {
            background: #f0f0f0;
            padding: 15px;
            border-radius: 4px;
            font-family: monospace;
            white-space: pre-wrap;
            margin-top: 10px;
            max-height: 300px;
            overflow-y: auto;
        }
        .color-palette {
            display: flex;
            gap: 10px;
            margin-top: 15px;
        }
        .color-box {
            width: 50px;
            height: 50px;
            border-radius: 8px;
            cursor: pointer;
            border: 3px solid #ddd;
            transition: transform 0.2s;
        }
        .color-box:hover {
            transform: scale(1.1);
        }
        .timer-display {
            font-size: 2em;
            text-align: center;
            margin: 20px 0;
            font-family: monospace;
        }
        .scroll-indicator {
            position: fixed;
            top: 0;
            left: 0;
            height: 4px;
            background: #1890ff;
            transition: width 0.1s;
        }
        .scroll-top-btn {
            position: fixed;
            bottom: 20px;
            right: 20px;
            padding: 15px 20px;
            background: #1890ff;
            color: white;
            border: none;
            border-radius: 50%;
            cursor: pointer;
            font-size: 20px;
            display: none;
            box-shadow: 0 2px 10px rgba(0,0,0,0.2);
        }
        .scroll-top-btn:hover {
            background: #0c7cd5;
        }
        .content-section {
            height: 500px;
            padding: 20px;
            margin: 10px 0;
            background: white;
            border-radius: 8px;
        }
    </style>
</head>
<body>
    <h1>Session 11: BOM (Browser Object Model)</h1>
    
    <div class="scroll-indicator" id="scrollIndicator"></div>
    <button class="scroll-top-btn" id="scrollTopBtn">↑</button>
    
    <div class="section">
        <h2>Topics Covered</h2>
        <ul>
            <li>What is the BOM</li>
            <li>alert/confirm/prompt dialogs</li>
            <li>setTimeout/clearTimeout</li>
            <li>setInterval/clearInterval</li>
            <li>Window location object</li>
            <li>Window open/close</li>
            <li>Window history</li>
            <li>Scroll methods</li>
            <li>localStorage</li>
            <li>sessionStorage</li>
        </ul>
    </div>

    <div class="section">
        <h2>BOM Dialogs</h2>
        <div class="demo">
            <button onclick="demoAlert()">Alert</button>
            <button onclick="demoConfirm()">Confirm</button>
            <button onclick="demoPrompt()">Prompt</button>
            <div id="dialogOutput" class="output">
                Click buttons to see dialog examples...
            </div>
        </div>
    </div>

    <div class="section">
        <h2>Timers</h2>
        <div class="demo">
            <button onclick="startTimeout()">Start Timeout</button>
            <button onclick="startInterval()">Start Interval</button>
            <button onclick="stopTimers()">Stop All</button>
            <div class="timer-display" id="timerDisplay">0.00</div>
            <div id="timerOutput" class="output">
                Timer status will appear here...
            </div>
        </div>
    </div>

    <div class="section">
        <h2>Window Location</h2>
        <div class="demo">
            <button onclick="showLocation()">Show Location</button>
            <button onclick="navigateToExample()">Navigate to Example</button>
            <button onclick="reloadPage()">Reload Page</button>
            <div id="locationOutput" class="output">
                Click to see location information...
            </div>
        </div>
    </div>

    <div class="section">
        <h2>Color App (localStorage)</h2>
        <div class="demo">
            <div class="color-palette" id="colorPalette">
                <div class="color-box" style="background: #ff0000;" data-color="#ff0000"></div>
                <div class="color-box" style="background: #00ff00;" data-color="#00ff00"></div>
                <div class="color-box" style="background: #0000ff;" data-color="#0000ff"></div>
                <div class="color-box" style="background: #ffff00;" data-color="#ffff00"></div>
                <div class="color-box" style="background: #ff00ff;" data-color="#ff00ff"></div>
                <div class="color-box" style="background: #00ffff;" data-color="#00ffff"></div>
            </div>
            <button onclick="randomColor()">Random Color</button>
            <button onclick="resetColor()">Reset</button>
            <div id="colorOutput" class="output">
                Selected color will appear here...
            </div>
        </div>
    </div>

    <div class="section">
        <h2>Session Storage Demo</h2>
        <div class="demo">
            <input type="text" id="sessionInput" placeholder="Enter session data">
            <button onclick="saveSessionData()">Save</button>
            <button onclick="loadSessionData()">Load</button>
            <button onclick="clearSessionData()">Clear</button>
            <div id="sessionOutput" class="output">
                Session storage operations...
            </div>
        </div>
    </div>

    <div class="section">
        <h2>Scroll Demo</h2>
        <div class="content-section" id="section1">
            <h3>Section 1</h3>
            <p>Scroll down to see the scroll indicator and scroll-to-top button.</p>
            <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit.</p>
        </div>
        <div class="content-section" id="section2">
            <h3>Section 2</h3>
            <p>More content here.</p>
            <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit.</p>
        </div>
        <div class="content-section" id="section3">
            <h3>Section 3</h3>
            <p>Even more content.</p>
            <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit.</p>
        </div>
        <div class="demo">
            <button onclick="scrollToSection1()">Scroll to Section 1</button>
            <button onclick="scrollToSection2()">Scroll to Section 2</button>
            <button onclick="scrollToSection3()">Scroll to Section 3</button>
        </div>
    </div>

    <div class="section">
        <h2>Console Output</h2>
        <p>Open the browser console (F12) to see all JavaScript examples.</p>
    </div>

    <script src="script.js" defer></script>
    <script>
        // Dialog demos
        function demoAlert() {
            alert("This is an alert dialog!");
            document.getElementById("dialogOutput").textContent = "Alert shown";
        }

        function demoConfirm() {
            const result = confirm("Are you sure you want to continue?");
            document.getElementById("dialogOutput").textContent = 
                `Confirm result: ${result ? "OK" : "Cancel"}`;
        }

        function demoPrompt() {
            const name = prompt("Please enter your name:");
            document.getElementById("dialogOutput").textContent = 
                name ? `Hello, ${name}!` : "User cancelled or entered nothing";
        }

        // Timer demos
        let timeoutId;
        let intervalId;
        let counter = 0;

        function startTimeout() {
            timeoutId = setTimeout(() => {
                document.getElementById("timerOutput").textContent = "Timeout executed!";
            }, 2000);
            document.getElementById("timerOutput").textContent = "Timeout started (2 seconds)";
        }

        function startInterval() {
            counter = 0;
            intervalId = setInterval(() => {
                counter++;
                document.getElementById("timerDisplay").textContent = counter.toFixed(2);
                document.getElementById("timerOutput").textContent = `Interval tick: ${counter}`;
            }, 100);
        }

        function stopTimers() {
            clearTimeout(timeoutId);
            clearInterval(intervalId);
            document.getElementById("timerOutput").textContent = "All timers stopped";
        }

        // Location demos
        function showLocation() {
            const output = `=== Location Information ===\n`;
            output += `Full URL: ${window.location.href}\n`;
            output += `Protocol: ${window.location.protocol}\n`;
            output += `Hostname: ${window.location.hostname}\n`;
            output += `Path: ${window.location.pathname}\n`;
            output += `Search: ${window.location.search}\n`;
            output += `Hash: ${window.location.hash}`;
            document.getElementById("locationOutput").textContent = output;
        }

        function navigateToExample() {
            window.location.href = "https://example.com";
        }

        function reloadPage() {
            window.location.reload();
        }

        // Color app
        document.getElementById("colorPalette").addEventListener("click", function(e) {
            if (e.target.classList.contains("color-box")) {
                const color = e.target.dataset.color;
                document.body.style.backgroundColor = color;
                localStorage.setItem("backgroundColor", color);
                document.getElementById("colorOutput").textContent = `Background color: ${color}`;
            }
        });

        function randomColor() {
            const colors = ["#ff0000", "#00ff00", "#0000ff", "#ffff00", "#ff00ff", "#00ffff"];
            const randomColor = colors[Math.floor(Math.random() * colors.length)];
            document.body.style.backgroundColor = randomColor;
            localStorage.setItem("backgroundColor", randomColor);
            document.getElementById("colorOutput").textContent = `Random color: ${randomColor}`;
        }

        function resetColor() {
            document.body.style.backgroundColor = "#f5f5f5";
            localStorage.removeItem("backgroundColor");
            document.getElementById("colorOutput").textContent = "Color reset to default";
        }

        // Load saved color
        window.addEventListener("load", function() {
            const savedColor = localStorage.getItem("backgroundColor");
            if (savedColor) {
                document.body.style.backgroundColor = savedColor;
                document.getElementById("colorOutput").textContent = `Loaded color: ${savedColor}`;
            }
        });

        // Session storage
        function saveSessionData() {
            const data = document.getElementById("sessionInput").value;
            sessionStorage.setItem("myData", data);
            document.getElementById("sessionOutput").textContent = `Saved: ${data}`;
        }

        function loadSessionData() {
            const data = sessionStorage.getItem("myData");
            document.getElementById("sessionOutput").textContent = 
                data ? `Loaded: ${data}` : "No data in session storage";
        }

        function clearSessionData() {
            sessionStorage.removeItem("myData");
            document.getElementById("sessionOutput").textContent = "Session data cleared";
        }

        // Scroll demos
        function scrollToSection1() {
            document.getElementById("section1").scrollIntoView({ behavior: "smooth" });
        }

        function scrollToSection2() {
            document.getElementById("section2").scrollIntoView({ behavior: "smooth" });
        }

        function scrollToSection3() {
            document.getElementById("section3").scrollIntoView({ behavior: "smooth" });
        }

        // Scroll indicator
        window.addEventListener("scroll", function() {
            const scrollTop = window.scrollY;
            const docHeight = document.documentElement.scrollHeight - window.innerHeight;
            const scrollPercent = (scrollTop / docHeight) * 100;
            
            document.getElementById("scrollIndicator").style.width = scrollPercent + "%";
            
            // Show/hide scroll-to-top button
            const scrollBtn = document.getElementById("scrollTopBtn");
            if (scrollTop > 300) {
                scrollBtn.style.display = "block";
            } else {
                scrollBtn.style.display = "none";
            }
        });

        // Scroll to top
        document.getElementById("scrollTopBtn").addEventListener("click", function() {
            window.scrollTo({ top: 0, behavior: "smooth" });
        });
    </script>
</body>
</html>
```

---

## 📝 Review (0.5h)

### BOM Challenge

```javascript
// Challenge 1: Create a countdown timer
// - Allow user to set time in seconds
// - Display countdown
// - Show "Time's up!" message
// - Play sound when done (optional)

// Challenge 2: Build a shopping cart with localStorage
// - Add items to cart
// - Update quantities
// - Remove items
// - Calculate total
// - Persist cart across page reloads

// Challenge 3: Create a theme switcher
// - Light/Dark mode
// - Save preference in localStorage
// - Apply theme on page load
// - Add smooth transitions

// Challenge 4: Build a form with sessionStorage
// - Multi-step form
// - Save progress between steps
// - Clear on completion
// - Show progress indicator

// Challenge 5: Create a bookmark/notes app
// - Add notes
// - Edit notes
// - Delete notes
// - Persist with localStorage
// - Search functionality
```

### Review Questions

1. **What is the BOM?**
   - [ ] A programming language
   - [ ] Browser Object Model for browser interaction
   - [ ] Document Object Model
   - [ ] A styling framework

2. **What does alert() do?**
   - [ ] Shows a confirmation dialog
   - [ ] Shows an alert message with OK button
   - [ ] Shows an input dialog
   - [ ] Shows a file dialog

3. **What does setTimeout() do?**
   - [ ] Executes code repeatedly
   - [ ] Executes code after a delay
   - [ ] Executes code immediately
   - [ ] Cancels a timeout

4. **What does setInterval() do?**
   - [ ] Executes code once after delay
   - [ ] Executes code repeatedly at intervals
   - [ ] Cancels an interval
   - [ ] Shows an alert

5. **What is the difference between localStorage and sessionStorage?**
   - [ ] No difference
   - [ ] localStorage persists after tab close, sessionStorage doesn't
   - [ ] sessionStorage persists after tab close, localStorage doesn't
   - [ ] Both clear on tab close

6. **What does window.location.href do?**
   - [ ] Returns the current URL
   - [ ] Navigates to a new URL
   - [ ] Reloads the page
   - [ ] Returns the browser history

7. **What does window.history.back() do?**
   - [ ] Goes forward in history
   - [ ] Goes back in history
   - [ ] Reloads the page
   - [ ] Clears history

8. **What does window.scrollTo() do?**
   - [ ] Scrolls element into view
   - [ ] Scrolls window to specific position
   - [ ] Scrolls by offset
   - [ ] Gets scroll position

9. **What does confirm() return?**
   - [ ] Always true
   - [ ] Always false
   - [ ] true if OK, false if Cancel
   - [ ] The entered text

10. **What does clearTimeout() do?**
    - [ ] Starts a timeout
    - [ ] Cancels a scheduled timeout
    - [ ] Starts an interval
    - [ ] Cancels an interval

### Correct Answers

1. ✅ Browser Object Model for browser interaction
2. ✅ Shows an alert message with OK button
3. ✅ Executes code after a delay
4. ✅ Executes code repeatedly at intervals
5. ✅ localStorage persists after tab close, sessionStorage doesn't
6. ✅ Navigates to a new URL
7. ✅ Goes back in history
8. ✅ Scrolls window to specific position
9. ✅ true if OK, false if Cancel
10. ✅ Cancels a scheduled timeout

---

## 🎯 Next Steps

1. ✅ Practice all BOM APIs
2. ✅ Build practical applications with localStorage
3. ✅ Implement timers and intervals
4. ✅ Create smooth scroll effects
5. ✅ Build navigation systems
6. ✅ Learn about cookies and IndexedDB
7. ✅ Explore modern storage APIs

---

## 📚 Additional Resources

- [MDN: Window Object](https://developer.mozilla.org/en-US/docs/Web/API/Window)
- [MDN: localStorage](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage)
- [MDN: sessionStorage](https://developer.mozilla.org/en-US/docs/Web/API/Window/sessionStorage)
- [MDN: setTimeout](https://developer.mozilla.org/en-US/docs/Web/API/WindowOrWorkerGlobalScope/setTimeout)
- [MDN: setInterval](https://developer.mozilla.org/en-US/docs/Web/API/WindowOrWorkerGlobalScope/setInterval)
- [MDN: Location](https://developer.mozilla.org/en-US/docs/Web/API/Location)
- [MDN: History](https://developer.mozilla.org/en-US/docs/Web/API/History)

**Remember:** The BOM provides powerful tools for browser interaction. Master these APIs to create dynamic, user-friendly web applications! 💪
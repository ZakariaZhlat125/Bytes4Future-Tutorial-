# Session 10: DOM Manipulation

## 📚 Theory (1h)

### What is the DOM

The Document Object Model (DOM) is a programming interface that represents HTML and XML documents as a tree structure of objects.

#### DOM Structure

```html
<!DOCTYPE html>
<html>
    <head>
        <title>My Page</title>
    </head>
    <body>
        <div id="container">
            <h1>Hello World</h1>
            <p class="text">This is a paragraph</p>
        </div>
    </body>
</html>
```

#### DOM Tree Representation

```
Document
├── html
    ├── head
    │   └── title
    └── body
        └── div (id="container")
            ├── h1
            └── p (class="text")
```

#### Key DOM Concepts

- **Document**: The root of the DOM tree
- **Elements**: HTML tags (div, p, h1, etc.)
- **Nodes**: Any object in the DOM tree (elements, text, comments, etc.)
- **Attributes**: Properties of elements (id, class, href, etc.)
- **Text Content**: The text inside elements

### Selecting Elements

JavaScript provides several methods to select elements from the DOM.

#### getElementById()

Returns the element with the specified ID.

```javascript
// HTML: <div id="myDiv">Content</div>
const element = document.getElementById("myDiv");
console.log(element); // <div id="myDiv">Content</div>
```

#### getElementsByClassName()

Returns a collection of elements with the specified class name.

```javascript
// HTML: <p class="text">Para 1</p><p class="text">Para 2</p>
const elements = document.getElementsByClassName("text");
console.log(elements.length); // 2
console.log(elements[0]); // First paragraph
```

#### getElementsByTagName()

Returns a collection of elements with the specified tag name.

```javascript
// HTML: <div>Div 1</div><div>Div 2</div>
const divs = document.getElementsByTagName("div");
console.log(divs.length); // 2
```

#### querySelector()

Returns the first element that matches a CSS selector.

```javascript
// HTML: <div class="container"><p class="text">Text</p></div>
const element = document.querySelector(".container .text");
console.log(element); // <p class="text">Text</p>
```

#### querySelectorAll()

Returns all elements that match a CSS selector.

```javascript
// HTML: <p class="text">Para 1</p><p class="text">Para 2</p>
const elements = document.querySelectorAll(".text");
console.log(elements.length); // 2
elements.forEach(el => console.log(el));
```

### Get/Set Elements & Attributes

#### Getting and Setting Element Content

```javascript
const element = document.getElementById("myDiv");

// Get text content
console.log(element.textContent); // "Content"

// Set text content
element.textContent = "New content";

// Get HTML content
console.log(element.innerHTML); // "<p>Content</p>"

// Set HTML content
element.innerHTML = "<strong>New content</strong>";
```

#### Getting and Setting Attributes

```javascript
const link = document.querySelector("a");

// Get attribute
console.log(link.getAttribute("href")); // "https://example.com"

// Set attribute
link.setAttribute("href", "https://newsite.com");

// Check if attribute exists
console.log(link.hasAttribute("target")); // true/false

// Remove attribute
link.removeAttribute("target");
```

#### Common Attributes

```javascript
const img = document.querySelector("img");

// src attribute
img.src = "image.jpg";

// alt attribute
img.alt = "Description";

// id attribute
element.id = "myId";

// class attribute
element.className = "myClass anotherClass";

// style attribute
element.style.color = "red";
```

### Checking Attributes

#### hasAttribute()

Checks if an element has a specific attribute.

```javascript
const element = document.getElementById("myDiv");

if (element.hasAttribute("data-value")) {
    console.log("Has data-value attribute");
}
```

#### getAttribute()

Gets the value of an attribute (returns null if not present).

```javascript
const value = element.getAttribute("data-value");
if (value !== null) {
    console.log("data-value:", value);
}
```

#### Checking Standard Attributes

```javascript
const link = document.querySelector("a");

// href
console.log(link.href); // Full URL
console.log(link.getAttribute("href")); // Exact attribute value

// id
console.log(link.id); // "" if no id
console.log(link.hasAttribute("id")); // false if no id

// class
console.log(link.className); // Space-separated classes
console.log(link.classList); // DOMTokenList
```

---

## 💻 Practical (1.5h)

### Exercise 1: Selecting Elements

```javascript
// Exercise 1.1: Basic selection
console.log("=== Basic Selection ===");

// getElementById
const header = document.getElementById("header");
console.log("Header:", header);

// getElementsByClassName
const paragraphs = document.getElementsByClassName("text");
console.log("Paragraphs:", paragraphs.length);

// getElementsByTagName
const divs = document.getElementsByTagName("div");
console.log("Divs:", divs.length);

// querySelector
const firstPara = document.querySelector(".text");
console.log("First paragraph:", firstPara);

// querySelectorAll
const allParas = document.querySelectorAll(".text");
console.log("All paragraphs:", allParas.length);

// Exercise 1.2: Dynamic selection
console.log("\n=== Dynamic Selection ===");

// Select by data attribute
const dataElements = document.querySelectorAll("[data-id]");
console.log("Elements with data-id:", dataElements.length);

// Select by attribute value
const links = document.querySelectorAll('a[href="https://example.com"]');
console.log("Specific links:", links.length);

// Select nth element
const secondPara = document.querySelectorAll(".text")[1];
console.log("Second paragraph:", secondPara);

// Exercise 1.3: Selection comparison
console.log("\n=== Selection Comparison ===");

// HTMLCollection vs NodeList
const htmlCollection = document.getElementsByClassName("text");
const nodeList = document.querySelectorAll(".text");

console.log("HTMLCollection:", htmlCollection); // Live collection
console.log("NodeList:", nodeList); // Static collection

// HTMLCollection updates when DOM changes
// NodeList does not update automatically
```

### Exercise 2: Get/Set Elements & Attributes

```javascript
// Exercise 2.1: Content manipulation
console.log("=== Content Manipulation ===");

const element = document.getElementById("content");

// textContent
console.log("Original text:", element.textContent);
element.textContent = "Updated text content";
console.log("Updated text:", element.textContent);

// innerHTML
element.innerHTML = "<strong>Bold text</strong> with <em>italics</em>";
console.log("HTML content:", element.innerHTML);

// Exercise 2.2: Attribute manipulation
console.log("\n=== Attribute Manipulation ===");

const link = document.querySelector("a");

// Get attributes
console.log("Original href:", link.getAttribute("href"));
console.log("Original target:", link.getAttribute("target"));

// Set attributes
link.setAttribute("href", "https://newsite.com");
link.setAttribute("target", "_blank");
link.setAttribute("data-custom", "value");

console.log("New href:", link.getAttribute("href"));
console.log("Custom attribute:", link.getAttribute("data-custom"));

// Exercise 2.3: Standard properties
console.log("\n=== Standard Properties ===");

const img = document.querySelector("img");

// Direct property access
img.src = "new-image.jpg";
img.alt = "New description";
img.id = "myImage";

console.log("Image src:", img.src);
console.log("Image alt:", img.alt);
console.log("Image id:", img.id);

// Exercise 2.4: Value property (form elements)
console.log("\n=== Form Element Values ===");

const input = document.querySelector("input");
input.value = "Default value";
console.log("Input value:", input.value);

const textarea = document.querySelector("textarea");
textarea.value = "Multiline text";
console.log("Textarea value:", textarea.value);

const select = document.querySelector("select");
select.value = "option2";
console.log("Select value:", select.value);
```

### Exercise 3: Checking Attributes

```javascript
// Exercise 3.1: hasAttribute
console.log("=== hasAttribute ===");

const element = document.getElementById("myElement");

console.log("Has id:", element.hasAttribute("id"));
console.log("Has data-value:", element.hasAttribute("data-value"));
console.log("Has class:", element.hasAttribute("class"));

// Exercise 3.2: Conditional attribute checking
console.log("\n=== Conditional Checking ===");

const link = document.querySelector("a");

if (link.hasAttribute("target")) {
    console.log("Link has target attribute");
    if (link.getAttribute("target") === "_blank") {
        console.log("Link opens in new tab");
    }
}

// Exercise 3.3: Safe attribute access
console.log("\n=== Safe Attribute Access ===");

const image = document.querySelector("img");

// Safe alt attribute check
const altText = image.getAttribute("alt") || "No description";
console.log("Alt text:", altText);

// Safe data attribute check
const dataValue = image.getAttribute("data-id") || "default";
console.log("Data ID:", dataValue);

// Exercise 3.4: Attribute existence patterns
console.log("\n=== Attribute Patterns ===");

const buttons = document.querySelectorAll("button");

buttons.forEach(button => {
    if (button.hasAttribute("disabled")) {
        console.log("Button is disabled");
    }
    
    if (button.hasAttribute("data-action")) {
        console.log("Button action:", button.getAttribute("data-action"));
    }
});
```

### Exercise 4: Create/Append Elements

```javascript
// Exercise 4.1: Creating elements
console.log("=== Creating Elements ===");

// Create element
const newDiv = document.createElement("div");
newDiv.textContent = "New div content";
newDiv.className = "new-element";
newDiv.id = "myNewDiv";

console.log("Created element:", newDiv);

// Create text node
const textNode = document.createTextNode("Text content");
console.log("Created text node:", textNode);

// Create comment
const comment = document.createComment("This is a comment");
console.log("Created comment:", comment);

// Exercise 4.2: Appending elements
console.log("\n=== Appending Elements ===");

const container = document.getElementById("container");

// Append child
container.appendChild(newDiv);

// Append multiple elements
const para1 = document.createElement("p");
para1.textContent = "Paragraph 1";

const para2 = document.createElement("p");
para2.textContent = "Paragraph 2";

container.appendChild(para1);
container.appendChild(para2);

// Exercise 4.3: Insert before/after
console.log("\n=== Insert Before/After ===");

const referenceElement = document.getElementById("reference");
const newElement = document.createElement("div");
newElement.textContent = "Inserted element";

// Insert before
container.insertBefore(newElement, referenceElement);

// Insert after (using insertAdjacentElement)
const afterElement = document.createElement("div");
afterElement.textContent = "After element";
referenceElement.insertAdjacentElement("afterend", afterElement);

// Exercise 4.4: Replace and remove
console.log("\n=== Replace and Remove ===");

const oldElement = document.getElementById("oldElement");
const replacement = document.createElement("div");
replacement.textContent = "Replacement element";

// Replace
container.replaceChild(replacement, oldElement);

// Remove
const elementToRemove = document.getElementById("removeMe");
if (elementToRemove) {
    elementToRemove.remove();
}
```

### Exercise 5: Product with Title & Description Practice

```javascript
// Exercise 5.1: Create product card
console.log("=== Product Card Creation ===");

function createProductCard(title, description, price, imageUrl) {
    // Create container
    const card = document.createElement("div");
    card.className = "product-card";
    
    // Create image
    const image = document.createElement("img");
    image.src = imageUrl || "https://via.placeholder.com/300";
    image.alt = title;
    image.className = "product-image";
    
    // Create content container
    const content = document.createElement("div");
    content.className = "product-content";
    
    // Create title
    const titleElement = document.createElement("h3");
    titleElement.className = "product-title";
    titleElement.textContent = title;
    
    // Create description
    const descElement = document.createElement("p");
    descElement.className = "product-description";
    descElement.textContent = description;
    
    // Create price
    const priceElement = document.createElement("div");
    priceElement.className = "product-price";
    priceElement.textContent = `$${price.toFixed(2)}`;
    
    // Create button
    const button = document.createElement("button");
    button.className = "product-button";
    button.textContent = "Add to Cart";
    button.setAttribute("data-action", "add-to-cart");
    
    // Assemble the card
    content.appendChild(titleElement);
    content.appendChild(descElement);
    content.appendChild(priceElement);
    content.appendChild(button);
    
    card.appendChild(image);
    card.appendChild(content);
    
    return card;
}

// Create and append product cards
const productsContainer = document.getElementById("products");

const product1 = createProductCard(
    "Laptop",
    "High-performance laptop for professionals",
    999.99,
    "https://via.placeholder.com/300"
);

const product2 = createProductCard(
    "Wireless Headphones",
    "Noise-cancelling wireless headphones",
    149.99,
    "https://via.placeholder.com/300"
);

productsContainer.appendChild(product1);
productsContainer.appendChild(product2);

// Exercise 5.2: Batch product creation
console.log("\n=== Batch Product Creation ===");

const productsData = [
    { title: "Smartphone", description: "Latest model smartphone", price: 699.99 },
    { title: "Tablet", description: "10-inch tablet", price: 349.99 },
    { title: "Smartwatch", description: "Fitness tracking smartwatch", price: 199.99 }
];

productsData.forEach(product => {
    const card = createProductCard(
        product.title,
        product.description,
        product.price,
        "https://via.placeholder.com/300"
    );
    productsContainer.appendChild(card);
});

// Exercise 5.3: Product card with event listeners
console.log("\n=== Product Card with Events ===");

function createInteractiveProductCard(title, description, price) {
    const card = createProductCard(title, description, price, "https://via.placeholder.com/300");
    
    const button = card.querySelector("button");
    button.addEventListener("click", function() {
        console.log(`Added to cart: ${title}`);
        this.textContent = "Added!";
        this.disabled = true;
    });
    
    return card;
}

const interactiveCard = createInteractiveProductCard(
    "Gaming Mouse",
    "RGB gaming mouse with programmable buttons",
    79.99
);

productsContainer.appendChild(interactiveCard);
```

### Exercise 6: Working with Children

```javascript
// Exercise 6.1: Accessing children
console.log("=== Accessing Children ===");

const container = document.getElementById("container");

// childNodes (includes all node types)
console.log("Child nodes:", container.childNodes.length);

// children (only element nodes)
console.log("Children elements:", container.children.length);

// firstChild / lastChild
console.log("First child:", container.firstChild);
console.log("Last child:", container.lastChild);

// firstElementChild / lastElementChild
console.log("First element child:", container.firstElementChild);
console.log("Last element child:", container.lastElementChild);

// Exercise 6.2: Traversing children
console.log("\n=== Traversing Children ===");

// nextSibling / previousSibling
console.log("Next sibling:", container.firstChild.nextSibling);
console.log("Previous sibling:", container.lastChild.previousSibling);

// nextElementSibling / previousElementSibling
console.log("Next element sibling:", container.firstElementChild.nextElementSibling);
console.log("Previous element sibling:", container.lastElementChild.previousElementSibling);

// Exercise 6.3: Modifying children
console.log("\n=== Modifying Children ===");

// Append child
const newChild = document.createElement("div");
newChild.textContent = "New child";
container.appendChild(newChild);

// Insert at specific position
const referenceChild = container.children[0];
const insertedChild = document.createElement("div");
insertedChild.textContent = "Inserted at position 0";
container.insertBefore(insertedChild, referenceChild);

// Replace child
const childToReplace = container.children[1];
const replacementChild = document.createElement("div");
replacementChild.textContent = "Replacement";
container.replaceChild(replacementChild, childToReplace);

// Remove child
const childToRemove = container.lastElementChild;
container.removeChild(childToRemove);

// Exercise 6.4: Clear all children
console.log("\n=== Clear Children ===");

function clearContainer(container) {
    while (container.firstChild) {
        container.removeChild(container.firstChild);
    }
}

// Alternative: innerHTML = ""
// Alternative: textContent = ""
```

### Exercise 7: DOM Events

```javascript
// Exercise 7.1: Basic event handling
console.log("=== Basic Event Handling ===");

const button = document.getElementById("myButton");

// Event listener
button.addEventListener("click", function(event) {
    console.log("Button clicked!");
    console.log("Event object:", event);
});

// Exercise 7.2: Event types
console.log("\n=== Event Types ===");

const element = document.getElementById("myElement");

// Click
element.addEventListener("click", () => console.log("Clicked"));

// Double click
element.addEventListener("dblclick", () => console.log("Double clicked"));

// Mouse events
element.addEventListener("mouseenter", () => console.log("Mouse entered"));
element.addEventListener("mouseleave", () => console.log("Mouse left"));
element.addEventListener("mouseover", () => console.log("Mouse over"));

// Keyboard events
document.addEventListener("keydown", (e) => console.log("Key pressed:", e.key));
document.addEventListener("keyup", (e) => console.log("Key released:", e.key));

// Form events
const form = document.getElementById("myForm");
form.addEventListener("submit", (e) => console.log("Form submitted"));

// Window events
window.addEventListener("load", () => console.log("Page loaded"));
window.addEventListener("resize", () => console.log("Window resized"));

// Exercise 7.3: Event object properties
console.log("\n=== Event Object Properties ===");

document.addEventListener("click", function(event) {
    console.log("Target:", event.target);
    console.log("Current target:", event.currentTarget);
    console.log("Event type:", event.type);
    console.log("Client X:", event.clientX);
    console.log("Client Y:", event.clientY);
    console.log("Ctrl key:", event.ctrlKey);
    console.log("Shift key:", event.shiftKey);
});

// Exercise 7.4: Event delegation
console.log("\n=== Event Delegation ===");

const list = document.getElementById("myList");

// Add click listener to parent instead of each item
list.addEventListener("click", function(event) {
    if (event.target.tagName === "LI") {
        console.log("List item clicked:", event.target.textContent);
    }
});
```

### Exercise 8: Form Validation & preventDefault

```javascript
// Exercise 8.1: Basic form validation
console.log("=== Basic Form Validation ===");

const form = document.getElementById("myForm");

form.addEventListener("submit", function(event) {
    event.preventDefault(); // Prevent form submission
    
    const name = document.getElementById("name").value;
    const email = document.getElementById("email").value;
    const age = document.getElementById("age").value;
    
    let isValid = true;
    let errors = [];
    
    // Validate name
    if (!name || name.trim() === "") {
        isValid = false;
        errors.push("Name is required");
    }
    
    // Validate email
    if (!email || !email.includes("@")) {
        isValid = false;
        errors.push("Valid email is required");
    }
    
    // Validate age
    if (!age || age < 0 || age > 120) {
        isValid = false;
        errors.push("Valid age (0-120) is required");
    }
    
    if (isValid) {
        console.log("Form is valid:", { name, email, age });
    } else {
        console.log("Form errors:", errors);
    }
});

// Exercise 8.2: Real-time validation
console.log("\n=== Real-time Validation ===");

const emailInput = document.getElementById("email");

emailInput.addEventListener("input", function() {
    const email = this.value;
    const isValid = email.includes("@") && email.includes(".");
    
    if (isValid) {
        this.style.borderColor = "green";
    } else {
        this.style.borderColor = "red";
    }
});

// Exercise 8.3: preventDefault examples
console.log("\n=== preventDefault Examples ===");

// Prevent link navigation
const link = document.querySelector("a[href]");
link.addEventListener("click", function(event) {
    event.preventDefault();
    console.log("Link click prevented");
});

// Prevent form submission
const button = document.querySelector('button[type="submit"]');
button.addEventListener("click", function(event) {
    event.preventDefault();
    console.log("Submit prevented");
});

// Prevent context menu
document.addEventListener("contextmenu", function(event) {
    event.preventDefault();
    console.log("Context menu prevented");
});

// Exercise 8.4: Custom validation functions
console.log("\n=== Custom Validation Functions ===");

function validateEmail(email) {
    const regex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
    return regex.test(email);
}

function validatePhoneNumber(phone) {
    const regex = /^\d{10}$/;
    return regex.test(phone);
}

function validatePassword(password) {
    return password.length >= 8;
}

function validateForm(formData) {
    const errors = [];
    
    if (!validateEmail(formData.email)) {
        errors.push("Invalid email format");
    }
    
    if (formData.phone && !validatePhoneNumber(formData.phone)) {
        errors.push("Phone must be 10 digits");
    }
    
    if (!validatePassword(formData.password)) {
        errors.push("Password must be at least 8 characters");
    }
    
    return {
        isValid: errors.length === 0,
        errors: errors
    };
}
```

### Exercise 9: Event Simulation

```javascript
// Exercise 9.1: Click simulation
console.log("=== Click Simulation ===");

const button = document.getElementById("myButton");

button.addEventListener("click", function() {
    console.log("Button was clicked!");
});

// Programmatically trigger click
button.click();

// Exercise 9.2: Focus simulation
console.log("\n=== Focus Simulation ===");

const input = document.getElementById("myInput");

input.addEventListener("focus", function() {
    console.log("Input focused");
    this.style.backgroundColor = "lightblue";
});

input.addEventListener("blur", function() {
    console.log("Input lost focus");
    this.style.backgroundColor = "white";
});

// Programmatically trigger focus
input.focus();

// Exercise 9.3: Blur simulation
console.log("\n=== Blur Simulation ===");

setTimeout(() => {
    input.blur();
}, 2000);

// Exercise 9.4: Event dispatching
console.log("\n=== Event Dispatching ===");

const customEvent = new Event("customEvent");

element.addEventListener("customEvent", function() {
    console.log("Custom event triggered");
});

element.dispatchEvent(customEvent);

// Exercise 9.5: Custom event with data
console.log("\n=== Custom Event with Data ===");

const dataEvent = new CustomEvent("dataEvent", {
    detail: { message: "Hello!", value: 42 }
});

element.addEventListener("dataEvent", function(event) {
    console.log("Custom data:", event.detail);
});

element.dispatchEvent(dataEvent);
```

### Exercise 10: Complete Working Example

**Complete script.js:**
```javascript
// Session 10: DOM Manipulation
// This script demonstrates DOM manipulation and events

console.log("=== Session 10: DOM Manipulation ===");

// 1. Selecting elements
console.log("\n--- Selecting Elements ---");
const header = document.getElementById("header");
const paragraphs = document.getElementsByClassName("text");
const firstPara = document.querySelector(".text");
const allParas = document.querySelectorAll(".text");

console.log("Header:", header);
console.log("Paragraphs:", paragraphs.length);

// 2. Get/set content
console.log("\n--- Content Manipulation ---");
const content = document.getElementById("content");
if (content) {
    content.textContent = "Updated content";
    content.innerHTML = "<strong>Bold content</strong>";
}

// 3. Attributes
console.log("\n--- Attributes ---");
const link = document.querySelector("a");
if (link) {
    link.setAttribute("href", "https://example.com");
    console.log("Href:", link.getAttribute("href"));
}

// 4. Create elements
console.log("\n--- Creating Elements ---");
const newDiv = document.createElement("div");
newDiv.textContent = "New element";
newDiv.className = "new-element";

const container = document.getElementById("container");
if (container) {
    container.appendChild(newDiv);
}

// 5. Events
console.log("\n--- Events ---");
const button = document.getElementById("myButton");
if (button) {
    button.addEventListener("click", function() {
        console.log("Button clicked!");
    });
}

// 6. Form validation
console.log("\n--- Form Validation ---");
const form = document.getElementById("myForm");
if (form) {
    form.addEventListener("submit", function(e) {
        e.preventDefault();
        console.log("Form submitted");
    });
}

console.log("\n=== Session 10 Complete ===");
```

**Complete index.html:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Session 10 - DOM Manipulation</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            max-width: 1200px;
            margin: 0 auto;
            padding: 20px;
            background-color: #f5f5f5;
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
        .demo input, .demo button, .demo select {
            padding: 8px;
            margin: 5px;
            border: 1px solid #ddd;
            border-radius: 4px;
        }
        .demo button {
            background-color: #1890ff;
            color: white;
            border: none;
            cursor: pointer;
        }
        .demo button:hover {
            background-color: #0c7cd5;
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
        .product-card {
            border: 1px solid #e8e8e8;
            border-radius: 8px;
            padding: 15px;
            margin: 10px 0;
            background: white;
            max-width: 300px;
        }
        .product-image {
            width: 100%;
            height: 150px;
            object-fit: cover;
            border-radius: 4px;
        }
        .product-content {
            padding: 10px 0;
        }
        .product-title {
            margin: 0 0 5px 0;
            color: #333;
        }
        .product-description {
            color: #666;
            font-size: 0.9em;
            margin: 5px 0;
        }
        .product-price {
            color: #1890ff;
            font-weight: bold;
            font-size: 1.2em;
            margin: 10px 0;
        }
        .product-button {
            background-color: #52c41a;
            color: white;
            border: none;
            padding: 8px 16px;
            border-radius: 4px;
            cursor: pointer;
            width: 100%;
        }
        .product-button:hover {
            background-color: #389e0d;
        }
        .product-button:disabled {
            background-color: #ccc;
            cursor: not-allowed;
        }
        .form-demo {
            background: white;
            padding: 20px;
            border-radius: 4px;
            border: 1px solid #e8e8e8;
        }
        .form-group {
            margin-bottom: 15px;
        }
        .form-group label {
            display: block;
            margin-bottom: 5px;
            font-weight: bold;
        }
        .form-group input {
            width: 100%;
            padding: 8px;
            border: 1px solid #ddd;
            border-radius: 4px;
            box-sizing: border-box;
        }
        .form-group input.invalid {
            border-color: #ff4d4f;
        }
        .form-group input.valid {
            border-color: #52c41a;
        }
        .error-message {
            color: #ff4d4f;
            font-size: 0.9em;
            margin-top: 5px;
        }
    </style>
</head>
<body>
    <h1>Session 10: DOM Manipulation</h1>
    
    <div class="section">
        <h2>Topics Covered</h2>
        <ul>
            <li>What is the DOM</li>
            <li>Selecting elements</li>
            <li>Get/set elements & attributes</li>
            <li>Checking attributes</li>
            <li>Create/append elements</li>
            <li>Working with children</li>
            <li>DOM events</li>
            <li>Form validation & preventDefault</li>
            <li>Event simulation</li>
        </ul>
    </div>

    <div class="section">
        <h2>Element Selection Demo</h2>
        <div class="demo">
            <div id="header" class="text">Header Element</div>
            <p class="text">Paragraph 1</p>
            <p class="text">Paragraph 2</p>
            <button onclick="demoSelection()">Test Selection</button>
            <div id="selectionOutput" class="output">
                Click to test selection methods...
            </div>
        </div>
    </div>

    <div class="section">
        <h2>Content & Attributes Demo</h2>
        <div class="demo">
            <div id="content">Original content</div>
            <a href="https://example.com" id="demoLink">Example Link</a>
            <img src="https://via.placeholder.com/100" alt="Placeholder" id="demoImage">
            <br><br>
            <button onclick="demoContent()">Change Content</button>
            <button onclick="demoAttributes()">Change Attributes</button>
            <div id="attrOutput" class="output">
                Click to see content/attribute changes...
            </div>
        </div>
    </div>

    <div class="section">
        <h2>Create Elements Demo</h2>
        <div class="demo">
            <button onclick="createElement()">Create Element</button>
            <button onclick="createMultiple()">Create Multiple</button>
            <button onclick="clearContainer()">Clear Container</button>
            <div id="container" style="border: 1px dashed #ccc; padding: 10px; margin-top: 10px; min-height: 50px;">
                Container for new elements
            </div>
        </div>
    </div>

    <div class="section">
        <h2>Product Card Demo</h2>
        <div class="demo">
            <input type="text" id="productTitle" placeholder="Product title">
            <input type="text" id="productDesc" placeholder="Description">
            <input type="number" id="productPrice" placeholder="Price">
            <button onclick="addProduct()">Add Product</button>
            <div id="products" style="display: flex; flex-wrap: wrap; gap: 10px; margin-top: 15px;">
                <!-- Products will be added here -->
            </div>
        </div>
    </div>

    <div class="section">
        <h2>Children Traversal Demo</h2>
        <div class="demo">
            <div id="parent" style="border: 1px solid #ccc; padding: 10px;">
                <div>Child 1</div>
                <div>Child 2</div>
                <div>Child 3</div>
            </div>
            <br>
            <button onclick="traverseChildren()">Traverse Children</button>
            <button onclick="modifyChildren()">Modify Children</button>
            <div id="childrenOutput" class="output">
                Click to traverse children...
            </div>
        </div>
    </div>

    <div class="section">
        <h2>Events Demo</h2>
        <div class="demo">
            <button id="clickButton">Click Me</button>
            <button id="dblClickButton">Double Click Me</button>
            <div id="hoverArea" style="padding: 20px; background: #e8e8e8; margin: 10px 0;">
                Hover over this area
            </div>
            <div id="eventOutput" class="output">
                Interact with elements to see events...
            </div>
        </div>
    </div>

    <div class="section">
        <h2>Form Validation Demo</h2>
        <div class="demo">
            <form id="myForm" class="form-demo">
                <div class="form-group">
                    <label for="name">Name:</label>
                    <input type="text" id="name" name="name">
                    <div class="error-message" id="nameError"></div>
                </div>
                <div class="form-group">
                    <label for="email">Email:</label>
                    <input type="email" id="email" name="email">
                    <div class="error-message" id="emailError"></div>
                </div>
                <div class="form-group">
                    <label for="age">Age:</label>
                    <input type="number" id="age" name="age">
                    <div class="error-message" id="ageError"></div>
                </div>
                <button type="submit">Submit</button>
            </form>
            <div id="formOutput" class="output">
                Form validation results will appear here...
            </div>
        </div>
    </div>

    <div class="section">
        <h2>Event Simulation Demo</h2>
        <div class="demo">
            <button id="simButton">Simulate Click</button>
            <input type="text" id="simInput" placeholder="Focus me">
            <br><br>
            <button onclick="simulateClick()">Simulate Click</button>
            <button onclick="simulateFocus()">Simulate Focus</button>
            <button onclick="simulateBlur()">Simulate Blur</button>
            <div id="simOutput" class="output">
                Click to simulate events...
            </div>
        </div>
    </div>

    <div class="section">
        <h2>Console Output</h2>
        <p>Open the browser console (F12) to see all JavaScript examples.</p>
    </div>

    <script src="script.js" defer></script>
    <script>
        // Selection demo
        function demoSelection() {
            const header = document.getElementById("header");
            const paragraphs = document.getElementsByClassName("text");
            const firstPara = document.querySelector(".text");
            const allParas = document.querySelectorAll(".text");
            
            let output = "=== Selection Methods ===\n\n";
            output += `getElementById("header"): ${header ? header.textContent : "Not found"}\n`;
            output += `getElementsByClassName("text"): ${paragraphs.length} elements\n`;
            output += `querySelector(".text"): ${firstPara ? firstPara.textContent : "Not found"}\n`;
            output += `querySelectorAll(".text"): ${allParas.length} elements\n`;
            
            document.getElementById("selectionOutput").textContent = output;
        }

        // Content and attributes demo
        function demoContent() {
            const content = document.getElementById("content");
            content.textContent = "Updated with textContent";
            setTimeout(() => {
                content.innerHTML = "<strong>Updated with innerHTML</strong>";
            }, 1000);
            
            document.getElementById("attrOutput").textContent = "Content updated (check the element above)";
        }

        function demoAttributes() {
            const link = document.getElementById("demoLink");
            const img = document.getElementById("demoImage");
            
            link.setAttribute("href", "https://newsite.com");
            link.setAttribute("target", "_blank");
            
            img.src = "https://via.placeholder.com/150";
            img.alt = "New description";
            
            let output = "=== Attribute Changes ===\n";
            output += `Link href: ${link.getAttribute("href")}\n`;
            output += `Link target: ${link.getAttribute("target")}\n`;
            output += `Image src: ${img.src}\n`;
            output += `Image alt: ${img.alt}`;
            
            document.getElementById("attrOutput").textContent = output;
        }

        // Create elements demo
        function createElement() {
            const container = document.getElementById("container");
            const newDiv = document.createElement("div");
            newDiv.textContent = "New element " + (container.children.length + 1);
            newDiv.style.padding = "10px";
            newDiv.style.margin = "5px 0";
            newDiv.style.background = "#e8e8e8";
            container.appendChild(newDiv);
        }

        function createMultiple() {
            const container = document.getElementById("container");
            for (let i = 0; i < 3; i++) {
                const newDiv = document.createElement("div");
                newDiv.textContent = "Batch element " + (i + 1);
                newDiv.style.padding = "10px";
                newDiv.style.margin = "5px 0";
                newDiv.style.background = "#d4edda";
                container.appendChild(newDiv);
            }
        }

        function clearContainer() {
            const container = document.getElementById("container");
            container.innerHTML = "";
        }

        // Product card demo
        function addProduct() {
            const title = document.getElementById("productTitle").value || "Default Product";
            const description = document.getElementById("productDesc").value || "No description";
            const price = parseFloat(document.getElementById("productPrice").value) || 0;
            
            const card = document.createElement("div");
            card.className = "product-card";
            
            card.innerHTML = `
                <img src="https://via.placeholder.com/300" alt="${title}" class="product-image">
                <div class="product-content">
                    <h3 class="product-title">${title}</h3>
                    <p class="product-description">${description}</p>
                    <div class="product-price">$${price.toFixed(2)}</div>
                    <button class="product-button">Add to Cart</button>
                </div>
            `;
            
            const button = card.querySelector("button");
            button.addEventListener("click", function() {
                this.textContent = "Added!";
                this.disabled = true;
            });
            
            document.getElementById("products").appendChild(card);
            
            // Clear inputs
            document.getElementById("productTitle").value = "";
            document.getElementById("productDesc").value = "";
            document.getElementById("productPrice").value = "";
        }

        // Children traversal demo
        function traverseChildren() {
            const parent = document.getElementById("parent");
            let output = "=== Children Traversal ===\n\n";
            
            output += `Number of children: ${parent.children.length}\n`;
            output += `First child: ${parent.firstElementChild.textContent}\n`;
            output += `Last child: ${parent.lastElementChild.textContent}\n`;
            
            parent.children.forEach((child, index) => {
                output += `Child ${index}: ${child.textContent}\n`;
            });
            
            document.getElementById("childrenOutput").textContent = output;
        }

        function modifyChildren() {
            const parent = document.getElementById("parent");
            Array.from(parent.children).forEach((child, index) => {
                child.style.background = index % 2 === 0 ? "#e8e8e8" : "#d4edda";
                child.textContent = `Modified ${index + 1}`;
            });
        }

        // Events demo
        document.getElementById("clickButton").addEventListener("click", function() {
            document.getElementById("eventOutput").textContent = "Click event triggered!";
        });

        document.getElementById("dblClickButton").addEventListener("dblclick", function() {
            document.getElementById("eventOutput").textContent = "Double click event triggered!";
        });

        document.getElementById("hoverArea").addEventListener("mouseenter", function() {
            document.getElementById("eventOutput").textContent = "Mouse entered the area";
            this.style.background = "#d4edda";
        });

        document.getElementById("hoverArea").addEventListener("mouseleave", function() {
            document.getElementById("eventOutput").textContent = "Mouse left the area";
            this.style.background = "#e8e8e8";
        });

        // Form validation
        document.getElementById("myForm").addEventListener("submit", function(e) {
            e.preventDefault();
            
            const name = document.getElementById("name").value;
            const email = document.getElementById("email").value;
            const age = document.getElementById("age").value;
            
            let isValid = true;
            let errors = [];
            
            // Reset errors
            document.getElementById("nameError").textContent = "";
            document.getElementById("emailError").textContent = "";
            document.getElementById("ageError").textContent = "";
            document.getElementById("name").classList.remove("invalid", "valid");
            document.getElementById("email").classList.remove("invalid", "valid");
            document.getElementById("age").classList.remove("invalid", "valid");
            
            // Validate name
            if (!name || name.trim() === "") {
                isValid = false;
                errors.push("Name is required");
                document.getElementById("nameError").textContent = "Name is required";
                document.getElementById("name").classList.add("invalid");
            } else {
                document.getElementById("name").classList.add("valid");
            }
            
            // Validate email
            if (!email || !email.includes("@")) {
                isValid = false;
                errors.push("Valid email is required");
                document.getElementById("emailError").textContent = "Valid email is required";
                document.getElementById("email").classList.add("invalid");
            } else {
                document.getElementById("email").classList.add("valid");
            }
            
            // Validate age
            if (!age || age < 0 || age > 120) {
                isValid = false;
                errors.push("Valid age (0-120) is required");
                document.getElementById("ageError").textContent = "Valid age (0-120) is required";
                document.getElementById("age").classList.add("invalid");
            } else {
                document.getElementById("age").classList.add("valid");
            }
            
            if (isValid) {
                document.getElementById("formOutput").textContent = 
                    `Form is valid!\nName: ${name}\nEmail: ${email}\nAge: ${age}`;
            } else {
                document.getElementById("formOutput").textContent = 
                    "Form errors:\n" + errors.join("\n");
            }
        });

        // Event simulation
        document.getElementById("simButton").addEventListener("click", function() {
            document.getElementById("simOutput").textContent = "Button was clicked!";
        });

        document.getElementById("simInput").addEventListener("focus", function() {
            document.getElementById("simOutput").textContent = "Input focused";
            this.style.backgroundColor = "lightblue";
        });

        document.getElementById("simInput").addEventListener("blur", function() {
            document.getElementById("simOutput").textContent = "Input lost focus";
            this.style.backgroundColor = "white";
        });

        function simulateClick() {
            document.getElementById("simButton").click();
        }

        function simulateFocus() {
            document.getElementById("simInput").focus();
        }

        function simulateBlur() {
            document.getElementById("simInput").blur();
        }
    </script>
</body>
</html>
```

---

## 📝 Review (0.5h)

### classList

The `classList` property provides methods to manipulate an element's classes.

```javascript
const element = document.getElementById("myElement");

// Add class
element.classList.add("new-class");

// Remove class
element.classList.remove("old-class");

// Toggle class
element.classList.toggle("active");

// Check if class exists
element.classList.contains("active"); // true/false

// Replace class
element.classList.replace("old", "new");

// Get all classes
element.classList.value; // "class1 class2 class3"
```

### CSS Styling via JS

```javascript
const element = document.getElementById("myElement");

// Individual properties
element.style.color = "red";
element.style.backgroundColor = "blue";
element.style.fontSize = "20px";

// Multiple properties with cssText
element.style.cssText = "color: red; background: blue; font-size: 20px;";

// Using setProperty
element.style.setProperty("color", "red");
element.style.setProperty("--main-color", "blue");

// Using getComputedStyle
const styles = window.getComputedStyle(element);
console.log(styles.color);
console.log(styles.backgroundColor);
```

### before/after/prepend/append/remove

```javascript
const parent = document.getElementById("parent");
const element = document.createElement("div");
element.textContent = "New element";

// prepend - insert as first child
parent.prepend(element);

// append - insert as last child
parent.append(element);

// before - insert before reference element
const reference = document.getElementById("reference");
reference.before(element);

// after - insert after reference element
reference.after(element);

// remove - remove element
element.remove();

// removeChild - remove child from parent
parent.removeChild(element);
```

### DOM Traversing

```javascript
const element = document.getElementById("myElement");

// Parent
element.parentNode;
element.parentElement;

// Children
element.children; // Element nodes only
element.childNodes; // All node types

// Siblings
element.previousSibling;
element.nextSibling;
element.previousElementSibling;
element.nextElementSibling;

// First/last child
element.firstElementChild;
element.lastElementChild;
```

### DOM Cloning

```javascript
const original = document.getElementById("original");

// Shallow clone
const clone = original.cloneNode(false); // Clone element only

// Deep clone
const deepClone = original.cloneNode(true); // Clone element and all descendants

// Clone and insert
const cloned = original.cloneNode(true);
document.getElementById("container").appendChild(cloned);
```

### addEventListener

```javascript
const button = document.getElementById("myButton");

// Basic event listener
button.addEventListener("click", function(event) {
    console.log("Clicked!");
});

// With options
button.addEventListener("click", function(event) {
    console.log("Clicked!");
}, { once: true }); // Remove after first trigger

// Named function for removal
function handleClick(event) {
    console.log("Clicked!");
}
button.addEventListener("click", handleClick);

// Remove event listener
button.removeEventListener("click", handleClick);

// Event delegation
document.addEventListener("click", function(event) {
    if (event.target.matches(".button")) {
        console.log("Button clicked:", event.target);
    }
});
```

### DOM Challenge

```javascript
// Challenge 1: Create a task list application
// - Add tasks
// - Delete tasks
// - Mark tasks as complete
// - Show task count

// Challenge 2: Build a color picker
// - Create color swatches
// - Change background color on click
// - Show selected color

// Challenge 3: Form validation
// - Validate email format
// - Validate password strength
// - Show real-time feedback
// - Prevent invalid submission

// Challenge 4: Dynamic table
// - Add rows dynamically
// - Delete rows
// - Sort by columns
// - Filter by search

// Challenge 5: Modal system
// - Open modal on button click
// - Close modal on overlay click
// - Close with escape key
// - Prevent body scroll when open
```

### Review Questions

1. **What is the DOM?**
   - [ ] A programming language
   - [ ] A programming interface for HTML/XML documents
   - [ ] A database
   - [ ] A styling language

2. **Which method selects an element by ID?**
   - [ ] querySelector
   - [ ] getElementsByClassName
   - [ ] getElementById
   - [ ] getElementsByTagName

3. **What does querySelectorAll return?**
   - [ ] A single element
   - [ ] A NodeList of all matching elements
   - [ ] An HTMLCollection
   - [ ] An array

4. **What does preventDefault() do?**
   - [ ] Prevents default browser behavior
   - [ ] Removes event listeners
   - [ ] Stops event propagation
   - [ ] Deletes the element

5. **What is event delegation?**
   - [ ] Creating multiple event listeners
   - [ ] Using a single listener on a parent to handle child events
   - [ ] Removing event listeners
   - [ ] Preventing events

6. **What does classList.add() do?**
   - [ ] Removes a class
   - [ ] Adds a class
   - [ ] Checks for a class
   - [ ] Replaces a class

7. **What is the difference between append and prepend?**
   - [ ] No difference
   - [ ] append adds at beginning, prepend at end
   - [ ] append adds at end, prepend at beginning
   - [ ] Both remove elements

8. **What does cloneNode(true) do?**
   - [ ] Clones only the element
   - [ ] Clones element and all descendants
   - [ ] Clones nothing
   - [ ] Clones the parent only

9. **What is the difference between parentNode and parentElement?**
   - [ ] No difference
   - [ ] parentNode can be any node, parentElement is always an element
   - [ ] parentNode is always an element
   - [ ] parentElement includes text nodes

10. **What does addEventListener do?**
    - [ ] Removes an event listener
    - [ ] Adds an event listener to an element
    - [ ] Creates an element
    - [ ] Deletes an element

### Correct Answers

1. ✅ A programming interface for HTML/XML documents
2. ✅ getElementById
3. ✅ A NodeList of all matching elements
4. ✅ Prevents default browser behavior
5. ✅ Using a single listener on a parent to handle child events
6. ✅ Adds a class
7. ✅ append adds at end, prepend at beginning
8. ✅ Clones element and all descendants
9. ✅ parentNode can be any node, parentElement is always an element
10. ✅ Adds an event listener to an element

---

## 🎯 Next Steps

1. ✅ Practice all DOM selection methods
2. ✅ Master element creation and manipulation
3. ✅ Understand event handling and delegation
4. ✅ Practice form validation techniques
5. ✅ Learn about DOM performance optimization
6. ✅ Explore modern DOM APIs (MutationObserver, IntersectionObserver)
7. ✅ Practice building interactive UI components

---

## 📚 Additional Resources

- [MDN: DOM Introduction](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model/Introduction)
- [MDN: Locating DOM Elements](https://developer.mozilla.org/en-US/docs/Web/API/Document_object_model/Locating_DOM_elements)
- [MDN: Event Reference](https://developer.mozilla.org/en-US/docs/Web/Events)
- [MDN: classList](https://developer.mozilla.org/en-US/docs/Web/API/Element/classList)
- [JavaScript.info: DOM](https://javascript.info/dom-navigation)
- [JavaScript.info: Events](https://javascript.info/introduction-browser-events)

**Remember:** DOM manipulation is essential for creating interactive web applications. Master these skills to build dynamic user interfaces! 💪
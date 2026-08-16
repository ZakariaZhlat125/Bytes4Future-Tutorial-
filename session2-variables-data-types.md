# Session 2: Variables, Data Types & Strings Basics

## 📚 Theory (1h)

### Data Types & typeof

JavaScript has two categories of data types: **primitive** and **reference**.

#### Primitive Data Types
Primitive types are immutable and stored directly in memory.

| Type | Description | Example |
|------|-------------|---------|
| **String** | Textual data | `"Hello"`, `'World'` |
| **Number** | Numeric values (integers & floats) | `42`, `3.14` |
| **Boolean** | Logical values | `true`, `false` |
| **Undefined** | Variable declared but not assigned | `let x;` |
| **Null** | Intentional absence of value | `null` |
| **Symbol** | Unique identifiers (ES6) | `Symbol("id")` |
| **BigInt** | Large integers (ES2020) | `9007199254740991n` |

#### Reference Data Types
Reference types are stored as references and can be modified.

| Type | Description | Example |
|------|-------------|---------|
| **Object** | Collection of properties | `{name: "John", age: 30}` |
| **Array** | Ordered list of values | `[1, 2, 3]` |
| **Function** | Executable code blocks | `function() {}` |
| **Date** | Date and time values | `new Date()` |
| **RegExp** | Pattern matching | `/pattern/` |

#### Using typeof Operator

The `typeof` operator returns the type of a value.

```javascript
// Primitive types
typeof "Hello"           // "string"
typeof 42                // "number"
typeof true              // "boolean"
typeof undefined         // "undefined"
typeof null              // "object" (historical bug)
typeof Symbol("id")      // "symbol"
typeof 9007199254740991n // "bigint"

// Reference types
typeof {}                // "object"
typeof []                // "object"
typeof function() {}     // "function"
typeof new Date()        // "object"
typeof /pattern/         // "object"
```

**Important Note:** `typeof null` returns `"object"` due to a historical JavaScript bug. Always check for null explicitly.

```javascript
// Better null check
const value = null;
console.log(value === null); // true
console.log(value === undefined); // false
```

### Variables Introduction

Variables are containers for storing data values. In JavaScript, you can declare variables using `var`, `let`, or `const`.

#### What is a Variable?
A variable is a named storage location in memory that holds a value.

```javascript
// Variable declaration and assignment
let userName = "John";
let userAge = 30;
```

#### Variable Declaration vs Assignment

```javascript
// Declaration only
let userName; // undefined

// Assignment
userName = "John";

// Declaration and assignment together
let userAge = 30;
```

#### Variable Scope
The scope determines where variables are accessible.

- **Global Scope:** Accessible everywhere in the code
- **Function Scope:** Accessible within a function
- **Block Scope:** Accessible within a block `{}`

### Identifier Naming Rules

Identifiers are names used for variables, functions, and other program elements.

#### Valid Identifier Rules

1. **Must start with:**
   - Letter (a-z, A-Z)
   - Underscore (_)
   - Dollar sign ($)

2. **Can contain:**
   - Letters
   - Numbers (0-9)
   - Underscores
   - Dollar signs

3. **Cannot:**
   - Start with a number
   - Contain spaces
   - Use reserved keywords

#### Examples

```javascript
// Valid identifiers
let userName;
let _private;
let $special;
let user123;
let user_name;
let USER_NAME;

// Invalid identifiers
let 123user;        // Cannot start with number
let user name;      // Cannot contain space
let class;          // Cannot use reserved keyword
let user@name;      // Cannot contain special characters
```

#### Naming Conventions

**Camel Case (recommended for variables):**
```javascript
let userName;
let userAge;
let isActive;
```

**Pascal Case (recommended for classes/constructors):**
```javascript
class UserAccount {}
class DatabaseConnection {}
```

**Snake Case (sometimes used for constants):**
```javascript
const MAX_SIZE = 100;
const API_KEY = "secret";
```

**Screaming Snake Case (for constants):**
```javascript
const MAX_CONNECTIONS = 100;
const DEFAULT_TIMEOUT = 5000;
```

#### Descriptive Naming

```javascript
// Bad - unclear purpose
let x;
let n;
let temp;

// Good - descriptive
let userName;
let numberOfUsers;
let temporaryStorage;
```

### var vs let vs const Comparison

#### var (Old Way - Avoid Using)

```javascript
var name = "John";
```

**Characteristics:**
- **Function scoped** (not block scoped)
- Can be **redeclared** in same scope
- Can be **reassigned**
- **Hoisted** to top of scope (initialized as `undefined`)
- Creates **global variable** when declared outside function

**Problems with var:**
```javascript
// Problem 1: Function scope only
if (true) {
    var x = 10;
}
console.log(x); // 10 (accessible outside block)

// Problem 2: Can be redeclared
var x = 10;
var x = 20; // No error

// Problem 3: Hoisting issues
console.log(y); // undefined (not error)
var y = 10;
```

#### let (Modern Way)

```javascript
let name = "John";
name = "Jane"; // Can reassign
```

**Characteristics:**
- **Block scoped**
- Cannot be **redeclared** in same scope
- Can be **reassigned**
- **Hoisted** to top of block (but not initialized - Temporal Dead Zone)
- Cannot be accessed before declaration

**Examples:**
```javascript
// Block scope
if (true) {
    let x = 10;
}
console.log(x); // ReferenceError: x is not defined

// Cannot redeclare
let x = 10;
let x = 20; // SyntaxError: Identifier 'x' has already been declared

// Temporal Dead Zone
console.log(y); // ReferenceError: Cannot access 'y' before initialization
let y = 10;
```

#### const (Modern Way for Constants)

```javascript
const PI = 3.14159;
// PI = 3.14; // TypeError: Assignment to constant variable
```

**Characteristics:**
- **Block scoped**
- Cannot be **redeclared** in same scope
- Cannot be **reassigned**
- Must be **initialized** at declaration
- **Hoisted** to top of block (but not initialized)

**Important Note:** For objects and arrays, the reference cannot be changed, but contents can be modified.

```javascript
// Primitive value
const PI = 3.14159;
PI = 3.14; // Error

// Object - can modify properties
const user = { name: "John", age: 30 };
user.name = "Jane"; // OK
user.age = 31;     // OK
// user = {};       // Error - cannot reassign

// Array - can modify elements
const numbers = [1, 2, 3];
numbers.push(4);  // OK
numbers[0] = 10;  // OK
// numbers = [];  // Error - cannot reassign
```

#### Comparison Table

| Feature | var | let | const |
|---------|-----|-----|-------|
| Scope | Function | Block | Block |
| Redeclaration | Allowed | Not allowed | Not allowed |
| Reassignment | Allowed | Allowed | Not allowed |
| Hoisting | Yes (initialized as undefined) | Yes (TDZ) | Yes (TDZ) |
| Initialization at declaration | Optional | Optional | Required |
| Use in modern code | Avoid | Default for variables | Default for constants |

#### When to Use Which

```javascript
// Use const for values that never change
const MAX_USERS = 100;
const API_URL = "https://api.example.com";
const CONFIG = { timeout: 5000 };

// Use let for values that will change
let counter = 0;
let currentUser = null;
let isActive = false;

// Avoid var in modern JavaScript
// var is kept for backward compatibility only
```

### String Syntax & Escape Sequences

#### String Syntax

JavaScript supports three ways to create strings:

**1. Single Quotes:**
```javascript
let message = 'Hello, World!';
```

**2. Double Quotes:**
```javascript
let message = "Hello, World!";
```

**3. Template Literals (Backticks):**
```javascript
let message = `Hello, World!`;
```

**Choosing Quotes:**
- Single quotes are most common in JavaScript
- Double quotes are useful when string contains single quotes
- Template literals are best for strings with variables or multi-line strings

#### Escape Sequences

Escape sequences allow you to include special characters in strings.

| Escape Sequence | Character | Description |
|-----------------|-----------|-------------|
| `\'` | `'` | Single quote |
| `\"` | `"` | Double quote |
| `\\` | `\` | Backslash |
| `\n` | | New line |
| `\r` | | Carriage return |
| `\t` | | Tab |
| `\b` | | Backspace |
| `\f` | | Form feed |
| `\uXXXX` | Unicode | Unicode character (e.g., `\u00A9` = ©) |

**Examples:**
```javascript
// Quotes inside strings
let quote1 = 'He said, "Hello!"';
let quote2 = "He said, 'Hello!'";
let quote3 = 'He said, \'Hello!\'';  // Using escape
let quote4 = "He said, \"Hello!\"";  // Using escape

// New lines
let multiline = "Line 1\nLine 2\nLine 3";

// Tab
let formatted = "Name:\tJohn\nAge:\t30";

// Backslash
let path = "C:\\Users\\John\\Documents";

// Unicode
let copyright = "\u00A9"; // ©
let heart = "\u2764";     // ❤
```

#### Template Literals (ES6)

Template literals provide powerful string features.

**Basic Usage:**
```javascript
let name = "John";
let age = 30;
let message = `My name is ${name} and I'm ${age} years old`;
```

**Multi-line Strings:**
```javascript
let html = `
    <div>
        <h1>Hello, ${name}!</h1>
        <p>Age: ${age}</p>
    </div>
`;
```

**Expressions:**
```javascript
let a = 10;
let b = 20;
let result = `The sum of ${a} and ${b} is ${a + b}`;

// Function calls
let message = `Current time: ${new Date().toLocaleTimeString()}`;

// Property access
let user = { name: "John", age: 30 };
let info = `User: ${user.name}, Age: ${user.age}`;
```

**Nested Template Literals:**
```javascript
let name = "John";
let greeting = `Hello, ${`Mr. ${name}`}!`;
```

**Tagged Template Literals:**
```javascript
function highlight(strings, ...values) {
    return strings.reduce((result, string, i) => {
        const value = values[i] ? `<strong>${values[i]}</strong>` : '';
        return result + string + value;
    }, '');
}

let name = "John";
let age = 30;
let message = highlight`Name: ${name}, Age: ${age}`;
// Result: "Name: <strong>John</strong>, Age: <strong>30</strong>"
```

#### String Properties and Methods

**Length:**
```javascript
let text = "Hello, World!";
console.log(text.length); // 13
```

**Accessing Characters:**
```javascript
let text = "Hello";
console.log(text[0]);     // "H"
console.log(text.charAt(0)); // "H"
```

**Case Conversion:**
```javascript
let text = "Hello World";
console.log(text.toUpperCase()); // "HELLO WORLD"
console.log(text.toLowerCase()); // "hello world"
```

**Finding Substrings:**
```javascript
let text = "Hello World";
console.log(text.indexOf("World")); // 6
console.log(text.includes("World")); // true
console.log(text.startsWith("Hello")); // true
console.log(text.endsWith("World")); // true
```

**Extracting Parts:**
```javascript
let text = "Hello World";
console.log(text.slice(0, 5));    // "Hello"
console.log(text.substring(0, 5)); // "Hello"
console.log(text.substr(0, 5));    // "Hello"
```

**Replacing:**
```javascript
let text = "Hello World";
console.log(text.replace("World", "JavaScript")); // "Hello JavaScript"
```

**Splitting:**
```javascript
let text = "apple,banana,orange";
console.log(text.split(",")); // ["apple", "banana", "orange"]
```

**Trimming:**
```javascript
let text = "  Hello World  ";
console.log(text.trim()); // "Hello World"
console.log(text.trimStart()); // "Hello World  "
console.log(text.trimEnd()); // "  Hello World"
```

---

## 💻 Practical (1.5h)

### Exercise 1: Data Types & typeof

```javascript
// Exercise 1.1: Test typeof operator
console.log("=== typeof Operator Tests ===");

let values = [
    "Hello",
    42,
    3.14,
    true,
    false,
    undefined,
    null,
    Symbol("id"),
    9007199254740991n,
    {},
    [],
    function() {},
    new Date()
];

values.forEach((value, index) => {
    console.log(`Value ${index + 1}:`, value, "- Type:", typeof value);
});

// Exercise 1.2: Type checking function
function checkType(value) {
    const type = typeof value;
    if (type === 'object' && value === null) {
        return 'null';
    }
    if (Array.isArray(value)) {
        return 'array';
    }
    return type;
}

console.log("\n=== Custom Type Checking ===");
console.log("null:", checkType(null));              // "null"
console.log("array:", checkType([1, 2, 3]));        // "array"
console.log("object:", checkType({a: 1}));          // "object"
console.log("function:", checkType(function() {}));  // "function"
```

### Exercise 2: Variables & Naming

```javascript
// Exercise 2.1: Variable declarations
console.log("=== Variable Declarations ===");

// Using let
let userName = "John";
let userAge = 30;
let isActive = true;

console.log("User:", userName, "Age:", userAge, "Active:", isActive);

// Using const
const MAX_USERS = 100;
const API_URL = "https://api.example.com";
const DEFAULT_CONFIG = {
    timeout: 5000,
    retries: 3
};

console.log("Max users:", MAX_USERS);
console.log("API URL:", API_URL);
console.log("Config:", DEFAULT_CONFIG);

// Exercise 2.2: Variable reassignment
console.log("\n=== Variable Reassignment ===");

let counter = 0;
console.log("Initial counter:", counter);

counter = counter + 1;
console.log("After increment:", counter);

counter += 5;
console.log("After adding 5:", counter);

// Exercise 2.3: Object and array modification with const
console.log("\n=== Const with Objects/Arrays ===");

const user = {
    name: "John",
    age: 30
};

console.log("Original user:", user);

user.name = "Jane";  // OK - modifying property
user.age = 31;       // OK - modifying property
console.log("Modified user:", user);

// user = {};        // Error - cannot reassign

const numbers = [1, 2, 3];
console.log("Original array:", numbers);

numbers.push(4);     // OK - adding element
numbers[0] = 10;     // OK - modifying element
console.log("Modified array:", numbers);

// numbers = [];     // Error - cannot reassign
```

### Exercise 3: String Basics

```javascript
// Exercise 3.1: String syntax
console.log("=== String Syntax ===");

let singleQuote = 'Hello, World!';
let doubleQuote = "Hello, World!";
let templateLiteral = `Hello, World!`;

console.log("Single quote:", singleQuote);
console.log("Double quote:", doubleQuote);
console.log("Template literal:", templateLiteral);

// Exercise 3.2: Escape sequences
console.log("\n=== Escape Sequences ===");

let withQuotes = 'He said, "Hello!"';
let withNewLine = "Line 1\nLine 2\nLine 3";
let withTab = "Name:\tJohn\nAge:\t30";
let withBackslash = "C:\\Users\\John\\Documents";
let withUnicode = "Copyright: \u00A9";

console.log("With quotes:", withQuotes);
console.log("With new lines:", withNewLine);
console.log("With tabs:", withTab);
console.log("With backslash:", withBackslash);
console.log("With unicode:", withUnicode);

// Exercise 3.3: String methods
console.log("\n=== String Methods ===");

let text = "Hello, World!";

console.log("Original:", text);
console.log("Length:", text.length);
console.log("Uppercase:", text.toUpperCase());
console.log("Lowercase:", text.toLowerCase());
console.log("Includes 'World':", text.includes("World"));
console.log("Starts with 'Hello':", text.startsWith("Hello"));
console.log("Ends with '!':", text.endsWith("!"));
console.log("Replace 'World' with 'JavaScript':", text.replace("World", "JavaScript"));
console.log("Slice (0,5):", text.slice(0, 5));
console.log("Split by ',':", text.split(","));
console.log("Trim:", "  Hello  ".trim());
```

### Exercise 4: Concatenation

```javascript
// Exercise 4.1: Basic concatenation
console.log("=== Basic Concatenation ===");

let firstName = "John";
let lastName = "Doe";
let fullName = firstName + " " + lastName;

console.log("First name:", firstName);
console.log("Last name:", lastName);
console.log("Full name:", fullName);

// Exercise 4.2: Concatenation with different types
console.log("\n=== Concatenation with Different Types ===");

let name = "John";
let age = 30;
let message = name + " is " + age + " years old";

console.log("Message:", message);

let price = 19.99;
let item = "Book";
let description = item + " costs $" + price;

console.log("Description:", description);

// Exercise 4.3: Concatenation challenges
console.log("\n=== Concatenation Challenges ===");

// Challenge 1: Create a greeting
let greeting = "Hello";
let target = "World";
let fullGreeting = greeting + ", " + target + "!";
console.log("Greeting:", fullGreeting);

// Challenge 2: Build an address
let street = "123 Main St";
let city = "New York";
let zip = "10001";
let address = street + ", " + city + " " + zip;
console.log("Address:", address);

// Challenge 3: Format a date
let day = "15";
let month = "August";
let year = "2026";
let date = day + " " + month + " " + year;
console.log("Date:", date);
```

### Exercise 5: Template Literals

```javascript
// Exercise 5.1: Basic template literals
console.log("=== Basic Template Literals ===");

let name = "John";
let age = 30;
let occupation = "Developer";

let message = `My name is ${name}, I'm ${age} years old, and I work as a ${occupation}`;
console.log("Message:", message);

// Exercise 5.2: Template literals with expressions
console.log("\n=== Template Literals with Expressions ===");

let a = 10;
let b = 20;
let sum = `The sum of ${a} and ${b} is ${a + b}`;
console.log("Sum:", sum);

let price = 100;
let discount = 0.2;
let finalPrice = `Original: $${price}, Discount: ${discount * 100}%, Final: $${price * (1 - discount)}`;
console.log("Price calculation:", finalPrice);

// Exercise 5.3: Multi-line template literals
console.log("\n=== Multi-line Template Literals ===");

let html = `
    <div class="user-card">
        <h2>${name}</h2>
        <p>Age: ${age}</p>
        <p>Occupation: ${occupation}</p>
    </div>
`;
console.log("HTML template:", html);

let email = `
    Dear ${name},

    Thank you for your interest in our company.
    We will review your application and get back to you soon.

    Best regards,
    HR Team
`;
console.log("Email template:", email);

// Exercise 5.4: Template literals with objects
console.log("\n=== Template Literals with Objects ===");

let user = {
    name: "John Doe",
    email: "john@example.com",
    age: 30,
    city: "New York"
};

let userInfo = `
    User Profile:
    -------------
    Name: ${user.name}
    Email: ${user.email}
    Age: ${user.age}
    City: ${user.city}
`;
console.log(userInfo);
```

### Exercise 6: Variables & Concatenation Challenge

```javascript
// Challenge: Build a user profile generator
console.log("=== User Profile Generator Challenge ===");

function generateUserProfile(firstName, lastName, age, email, city) {
    // Your task: Complete this function using template literals
    // Return a formatted user profile string

    let fullName = firstName + " " + lastName;
    let userProfile = `
        USER PROFILE
        ============
        Name: ${fullName}
        Age: ${age}
        Email: ${email}
        City: ${city}
        
        User ${fullName} is ${age} years old and lives in ${city}.
        Contact them at ${email}.
    `;

    return userProfile;
}

// Test the function
let profile1 = generateUserProfile("John", "Doe", 30, "john@example.com", "New York");
console.log(profile1);

let profile2 = generateUserProfile("Jane", "Smith", 25, "jane@example.com", "Los Angeles");
console.log(profile2);

// Challenge: Build a product description generator
console.log("\n=== Product Description Generator Challenge ===");

function generateProductDescription(name, price, category, inStock) {
    // Your task: Complete this function
    // Return a formatted product description

    let stockStatus = inStock ? "In Stock" : "Out of Stock";
    let description = `
        PRODUCT DETAILS
        ================
        Product: ${name}
        Price: $${price}
        Category: ${category}
        Status: ${stockStatus}
        
        ${name} is available in the ${category} category for $${price}.
        Current status: ${stockStatus}.
    `;

    return description;
}

// Test the function
let product1 = generateProductDescription("Laptop", 999.99, "Electronics", true);
console.log(product1);

let product2 = generateProductDescription("T-Shirt", 19.99, "Clothing", false);
console.log(product2);

// Challenge: Build a sentence builder
console.log("\n=== Sentence Builder Challenge ===");

function buildSentence(article, noun, verb, adjective, adverb) {
    // Your task: Build a sentence using template literals
    // Example: "The quick brown fox jumps quickly"

    let sentence = `${article} ${adjective} ${noun} ${verb} ${adverb}.`;
    return sentence;
}

// Test the function
console.log(buildSentence("The", "fox", "jumps", "quick", "quickly"));
console.log(buildSentence("A", "cat", "sleeps", "lazy", "peacefully"));
console.log(buildSentence("The", "developer", "codes", "talented", "efficiently"));
```

### Exercise 7: Complete Working Example

**Complete script.js:**
```javascript
// Session 2: Variables, Data Types & Strings Basics
// This script demonstrates variables, data types, and string operations

console.log("=== Session 2: Variables, Data Types & Strings ===");

// 1. Data types demonstration
console.log("\n--- Data Types ---");
let stringValue = "Hello";
let numberValue = 42;
let booleanValue = true;
let nullValue = null;
let undefinedValue = undefined;
let objectValue = { name: "John", age: 30 };
let arrayValue = [1, 2, 3, 4, 5];

console.log("String:", stringValue, "Type:", typeof stringValue);
console.log("Number:", numberValue, "Type:", typeof numberValue);
console.log("Boolean:", booleanValue, "Type:", typeof booleanValue);
console.log("Null:", nullValue, "Type:", typeof nullValue, "(Note: typeof null is 'object')");
console.log("Undefined:", undefinedValue, "Type:", typeof undefinedValue);
console.log("Object:", objectValue, "Type:", typeof objectValue);
console.log("Array:", arrayValue, "Type:", typeof arrayValue);

// 2. Variables demonstration
console.log("\n--- Variables ---");

// let - can be reassigned
let counter = 0;
console.log("Initial counter:", counter);
counter = 10;
console.log("Updated counter:", counter);

// const - cannot be reassigned
const MAX_VALUE = 100;
console.log("Max value:", MAX_VALUE);
// MAX_VALUE = 200; // This would cause an error

// const with objects (can modify properties)
const user = {
    name: "John",
    age: 30
};
console.log("Original user:", user);
user.name = "Jane"; // This is allowed
user.age = 31;     // This is allowed
console.log("Modified user:", user);

// 3. String operations
console.log("\n--- String Operations ---");

let firstName = "John";
let lastName = "Doe";

// Concatenation
let fullNameConcat = firstName + " " + lastName;
console.log("Concatenation:", fullNameConcat);

// Template literals
let fullNameTemplate = `${firstName} ${lastName}`;
console.log("Template literal:", fullNameTemplate);

// String methods
let message = "Hello, World!";
console.log("Original:", message);
console.log("Uppercase:", message.toUpperCase());
console.log("Length:", message.length);
console.log("Includes 'World':", message.includes("World"));

// 4. Escape sequences
console.log("\n--- Escape Sequences ---");
let quotedText = 'He said, "Hello!"';
let multiline = "Line 1\nLine 2\nLine 3";
let withTab = "Name:\tJohn";
console.log("Quoted:", quotedText);
console.log("Multiline:", multiline);
console.log("With tab:", withTab);

// 5. Practical example: User profile
console.log("\n--- User Profile Example ---");

function createUserProfile(name, age, email, city) {
    return `
        USER PROFILE
        ============
        Name: ${name}
        Age: ${age}
        Email: ${email}
        City: ${city}
        
        ${name} is ${age} years old and lives in ${city}.
    `;
}

let userProfile = createUserProfile("John Doe", 30, "john@example.com", "New York");
console.log(userProfile);

console.log("\n=== Session 2 Complete ===");
```

**Complete index.html:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Session 2 - Variables, Data Types & Strings</title>
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
        .output {
            background: #f0f0f0;
            padding: 15px;
            border-radius: 4px;
            font-family: monospace;
            white-space: pre-wrap;
        }
    </style>
</head>
<body>
    <h1>Session 2: Variables, Data Types & Strings</h1>
    
    <div class="section">
        <h2>Instructions</h2>
        <p>Open the browser console (F12) to see the JavaScript output.</p>
        <p>This session covers:</p>
        <ul>
            <li>Data types and typeof operator</li>
            <li>Variables (let, const)</li>
            <li>Identifier naming rules</li>
            <li>String syntax and escape sequences</li>
            <li>Concatenation and template literals</li>
        </ul>
    </div>

    <div class="section">
        <h2>Console Output</h2>
        <div class="output" id="console-output">
            JavaScript output will appear in the browser console...
        </div>
    </div>

    <script src="script.js" defer></script>
</body>
</html>
```

---

## 📝 Review (0.5h)

### Q&A

**Q1: What is the difference between primitive and reference data types?**
A: Primitive types (string, number, boolean, etc.) are stored directly in memory and are immutable. Reference types (objects, arrays, functions) are stored as references and can be modified.

**Q2: Why does `typeof null` return "object"?**
A: This is a historical bug in JavaScript. `null` should technically return "null", but for backward compatibility, it returns "object". Always check for null explicitly using `value === null`.

**Q3: When should I use `let` vs `const`?**
A: Use `const` by default for values that won't change. Use `let` only when you need to reassign a variable. Avoid `var` in modern JavaScript.

**Q4: What is the Temporal Dead Zone?**
A: The Temporal Dead Zone (TDZ) is the period between entering a scope and the actual declaration of a `let` or `const` variable. During this time, accessing the variable throws a ReferenceError.

**Q5: Can I modify properties of a `const` object?**
A: Yes, you can modify properties of a `const` object or add/remove elements from a `const` array. What you cannot do is reassign the entire object or array to a new value.

**Q6: What are template literals and when should I use them?**
A: Template literals are strings enclosed in backticks (`) that support interpolation (${expression}) and multi-line strings. Use them when you need to include variables or expressions in strings, or for multi-line strings.

**Q7: What are escape sequences used for?**
A: Escape sequences allow you to include special characters in strings that would otherwise be difficult to represent, such as quotes, newlines, tabs, and Unicode characters.

**Q8: What is the difference between `slice()`, `substring()`, and `substr()`?**
A: 
- `slice(start, end)`: Extracts from start to end (end not included). Supports negative indices.
- `substring(start, end)`: Similar to slice but doesn't support negative indices.
- `substr(start, length)`: Extracts from start for a specified length. (Deprecated)

**Q9: How do I check if a value is an array?**
A: Use `Array.isArray(value)`. This is the most reliable method since `typeof []` returns "object".

**Q10: What are the naming conventions for JavaScript identifiers?**
A: 
- Variables and functions: camelCase (userName, calculateTotal)
- Classes/Constructors: PascalCase (UserAccount, DatabaseConnection)
- Constants: SCREAMING_SNAKE_CASE (MAX_USERS, API_URL)

### Review Questions

1. **What does `typeof null` return?**
   - [ ] "null"
   - [ ] "object"
   - [ ] "undefined"
   - [ ] "string"

2. **Which keyword should you use by default for variables that won't change?**
   - [ ] var
   - [ ] let
   - [ ] const
   - [ ] All of the above

3. **What is the correct way to include a variable in a string?**
   - [ ] `"Hello " + name`
   - [ ] `"Hello {name}"`
   - [ ] `` `Hello ${name}` ``
   - [ ] Both A and C

4. **Which escape sequence represents a new line?**
   - [ ] `\t`
   - [ ] `\n`
   - [ ] `\r`
   - [ ] `\b`

5. **What is the Temporal Dead Zone?**
   - [ ] A gaming term
   - [ ] The period before a let/const variable is declared
   - [ ] A debugging tool
   - [ ] A type of error

6. **Can you modify properties of a const object?**
   - [ ] Yes
   - [ ] No
   - [ ] Only if it's an array
   - [ ] Only if you use let

7. **What is the naming convention for variables in JavaScript?**
   - [ ] snake_case
   - [ ] PascalCase
   - [ ] camelCase
   - [ ] kebab-case

8. **How do you check if a value is an array?**
   - [ ] `typeof value === "array"`
   - [ ] `value instanceof Array`
   - [ ] `Array.isArray(value)`
   - [ ] Both B and C

9. **What is the result of `"Hello" + " " + "World"`?**
   - [ ] `"Hello World"`
   - [ ] `"HelloWorld"`
   - [ ] `"Hello  World"`
   - [ ] Error

10. **Which string method converts text to uppercase?**
    - [ ] `toUpper()`
    - [ ] `toUpperCase()`
    - [ ] `upperCase()`
    - [ ] `toUpperString()`

### Correct Answers

1. ✅ "object"
2. ✅ const
3. ✅ Both A and C
4. ✅ `\n`
5. ✅ The period before a let/const variable is declared
6. ✅ Yes
7. ✅ camelCase
8. ✅ Both B and C
9. ✅ "Hello World"
10. ✅ `toUpperCase()`

---

## 🎯 Next Steps

1. ✅ Practice using `let` and `const` in different scenarios
2. ✅ Master template literals for string interpolation
3. ✅ Learn all common string methods
4. ✅ Understand the difference between primitive and reference types
5. ✅ Practice proper naming conventions
6. ✅ Complete the user profile generator challenge

---

## 📚 Additional Resources

- [MDN: Data Types](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures)
- [MDN: typeof](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/typeof)
- [MDN: Template Literals](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals)
- [JavaScript.info: Variables](https://javascript.info/variables)
- [JavaScript.info: Strings](https://javascript.info/string)

**Remember:** Understanding data types and proper variable usage is fundamental to writing clean, bug-free JavaScript code! 💪
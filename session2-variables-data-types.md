# Session 2: Variables, Data Types & Strings Basics — Active Learning Redesign

## Session Plan for the Instructor

- **Total time:** approximately 90 to 120 minutes
- **Pacing rule:** never explain theory continuously for more than 15–20 minutes. After every concept, students must predict, write, or fix code.
- **Pedagogical pattern for every topic:**
  1. **Problem** — a realistic mini-situation
  2. **Guess** — ask: "What do you expect to happen?"
  3. **Explain** — the shortest rule that fixes the problem
  4. **Code** — live code written in front of the students, step by step
  5. **Challenge** — students solve a small task on their own or in groups
  6. **Review** — discuss the answer and the most common mistake

### Competition and Points

- 1 point per correct prediction in the "Guess" phase.
- 1–3 points per completed challenge, depending on difficulty.
- A "Bug Hunter" badge for each student who finds and fixes an intentional error.
- Keep a simple tally on a shared board or in the chat.

### Instructor Questions to Ask During the Session

- "What type is this value?"
- "What happens if we reassign a `const`?"
- "Which naming convention should we use?"
- "Is this a good variable name?"
- "Why does `typeof null` say `object`?"

---

## Part 0: Warm-Up — The Sticky Notes (5 minutes)

### Problem

A student wants to remember a username, a score, and whether the user is logged in. The student writes everything in one big message string.

```javascript
let everything = "John 250 true";
```

### Guess

Ask: "Is this easy to work with? What if we need to add 10 to the score?"

### Explain

Variables let us store values with names. Each variable has a type, which tells JavaScript what kind of data it holds.

### Live Code

```javascript
let userName = "John";
let score = 250;
let isLoggedIn = true;

score = score + 10;
console.log(userName, score, isLoggedIn);
```

### Review

Variables make values easy to read, update, and reuse.

---

## Part 1: Data Types

### 1.1 Primitive Types

#### Problem

We have different kinds of values. JavaScript needs to know what kind each one is.

#### Live Code

```javascript
let name = "John";
let age = 30;
let price = 19.99;
let isActive = true;
let nothing = null;
let notAssigned;

console.log(typeof name);        // "string"
console.log(typeof age);         // "number"
console.log(typeof price);       // "number"
console.log(typeof isActive);    // "boolean"
console.log(typeof nothing);     // "object" (historical bug)
console.log(typeof notAssigned); // "undefined"
```

#### Explain

- Primitive types: `string`, `number`, `boolean`, `undefined`, `null`, `symbol`, `bigint`.
- `typeof null` is a historical bug; use `value === null` to check for null.
- `undefined` means a variable was declared but not assigned.

#### Challenge 1.1 — typeof Predictions (individual, 3 minutes)

- **Requirement:** Predict the type of each value before running the code.
- **Time limit:** 3 minutes
- **Values:** `"42"`, `42`, `true`, `null`, `undefined`, `Symbol("id")`, `9007199254740991n`.

### 1.2 Reference Types

#### Live Code

```javascript
let user = { name: "John", age: 30 };
let colors = ["red", "green", "blue"];

console.log(typeof user);   // "object"
console.log(typeof colors); // "object"
console.log(Array.isArray(colors)); // true
```

#### Challenge 1.2 — Array or Object? (individual, 3 minutes)

- **Requirement:** Write `isArray(value)` that returns `true` only for arrays.
- **Time limit:** 3 minutes
- **Hint:** Use `Array.isArray`.

---

## Part 2: Variables

### 2.1 Declaration and Assignment

#### Problem

We need a place to store a value and change it later.

#### Live Code

```javascript
// Declaration
let userName;
console.log(userName); // undefined

// Assignment
userName = "John";
console.log(userName); // "John"

// Declaration and assignment
let userAge = 30;
```

#### Challenge 2.1 — Declare and Assign (individual, 2 minutes)

- **Requirement:** Declare `let city;`, then assign `"New York"`, then print it.
- **Time limit:** 2 minutes

### 2.2 var, let, const

#### Problem

Some variables should change, others should not. `var` can cause unexpected bugs.

#### Live Code

```javascript
// var is function scoped
if (true) {
  var oldWay = "I leak out";
}
console.log(oldWay); // works

// let is block scoped
if (true) {
  let newWay = "I stay inside";
}
// console.log(newWay); // ReferenceError

// const cannot be reassigned
const PI = 3.14159;
// PI = 3.14; // TypeError
```

#### Challenge 2.2 — Fix the var (individual, 4 minutes)

- **Requirement:** Rewrite this code using `let` or `const` and explain why it is safer.
- **Time limit:** 4 minutes

```javascript
var score = 0;
var score = 10;
console.log(score);
```

### 2.3 const with Objects and Arrays

#### Problem

A `const` object cannot be replaced, but can its contents change?

#### Live Code

```javascript
const user = { name: "John", age: 30 };
user.name = "Jane"; // OK
user.age = 31;      // OK
// user = {};       // Error

const numbers = [1, 2, 3];
numbers.push(4);    // OK
numbers[0] = 10;    // OK
// numbers = [];    // Error
```

#### Challenge 2.3 — Const Surprise (individual, 4 minutes)

- **Requirement:** Predict which lines will error, then run the code.
- **Time limit:** 4 minutes

### 2.4 When to Use Which

#### Live Code

```javascript
const MAX_USERS = 100;
const API_URL = "https://api.example.com";

let counter = 0;
let currentUser = null;

// Use const unless you need to reassign
```

#### Challenge 2.4 — let or const? (individual, 3 minutes)

- **Requirement:** Choose `let` or `const` for: userName, totalScore, PI, apiKey.
- **Time limit:** 3 minutes

---

## Part 3: Naming Rules and Conventions

### 3.1 Valid Identifiers

#### Problem

Variable names must follow rules or JavaScript throws an error.

#### Live Code

```javascript
// Valid
let userName;
let _private;
let $button;
let user123;
let user_name;

// Invalid
// let 123user;     // Cannot start with number
// let user name;   // Cannot contain space
// let class;       // Reserved keyword
```

#### Challenge 3.1 — Valid or Invalid? (individual, 3 minutes)

- **Requirement:** Classify each name: `firstName`, `2ndPlace`, `my-variable`, `class`, `_score`, `$value`.
- **Time limit:** 3 minutes

### 3.2 Naming Conventions

#### Live Code

```javascript
// camelCase for variables
let userName = "John";
let numberOfUsers = 100;

// PascalCase for classes
class UserAccount {}

// SCREAMING_SNAKE_CASE for constants
const MAX_CONNECTIONS = 100;
```

#### Challenge 3.2 — Rename (individual, 3 minutes)

- **Requirement:** Rewrite these with camelCase or screaming snake case: `myname`, `totalscore`, `apikey`, `maxusers`.
- **Time limit:** 3 minutes

---

## Part 4: Strings

### 4.1 String Syntax

#### Problem

We need to create text. JavaScript gives us three ways.

#### Live Code

```javascript
let single = 'Hello';
let double = "World";
let template = `Hello, ${double}!`;

console.log(single);
console.log(double);
console.log(template);
```

#### Challenge 4.1 — Choose Quotes (individual, 3 minutes)

- **Requirement:** Write these strings: `It's a nice day`, `"Hello," she said`, `He said, "It's fine"`.
- **Time limit:** 3 minutes
- **Hint:** Mix quotes or use backslashes.

### 4.2 Escape Sequences

#### Problem

Special characters inside strings need special treatment.

#### Live Code

```javascript
console.log("He said, \"Hello!\"");
console.log("Line 1\nLine 2");
console.log("Name:\tJohn");
console.log("C:\\Users\\John");
console.log("\u00A9"); // ©
```

#### Challenge 4.2 — Escape Practice (individual, 3 minutes)

- **Requirement:** Print this exact output using one `console.log`:

```
Name: "John"
Age:	30
```

- **Time limit:** 3 minutes

### 4.3 String Methods

#### Live Code

```javascript
let text = "Hello, World!";

console.log(text.length);                  // 13
console.log(text[0]);                      // "H"
console.log(text.toUpperCase());           // "HELLO, WORLD!"
console.log(text.toLowerCase());           // "hello, world!"
console.log(text.indexOf("World"));        // 6
console.log(text.includes("World"));       // true
console.log(text.startsWith("Hello"));     // true
console.log(text.slice(0, 5));             // "Hello"
console.log(text.replace("World", "JS"));  // "Hello, JS!"
console.log("  hello  ".trim());           // "hello"
console.log("a,b,c".split(","));           // ["a", "b", "c"]
```

#### Challenge 4.3 — String Detective (individual, 4 minutes)

- **Requirement:** Extract the word "World" from `"Hello, World!"` and make it uppercase.
- **Time limit:** 4 minutes

### 4.4 Concatenation vs Template Literals

#### Problem

Build a sentence from variables.

#### Live Code

```javascript
let name = "John";
let age = 30;

// Concatenation
let message1 = name + " is " + age + " years old";

// Template literal
let message2 = `${name} is ${age} years old`;

console.log(message1);
console.log(message2);
```

#### Challenge 4.4 — Build a Product Card (individual, 5 minutes)

- **Requirement:** Use a template literal to build this string:

```
Product: Laptop
Price: $999.99
In Stock: true
```

- **Time limit:** 5 minutes

### 4.5 Multi-line Strings

#### Live Code

```javascript
let name = "John";
let age = 30;

let html = `
  <div class="user-card">
    <h2>${name}</h2>
    <p>Age: ${age}</p>
  </div>
`;

console.log(html);
```

#### Challenge 4.5 — Email Template (individual, 5 minutes)

- **Requirement:** Create a multi-line email template using template literals.
- **Time limit:** 5 minutes

---

## Bug Hunt 1

### Problem

The following code has three deliberate bugs. Ask students to find them.

```javascript
let user name = "John";
let score = 0;
score = "ten";

const PI = 3.14;
PI = 3.14159;

let count = 0;
let count = 1;
```

### Issues

1. `let user name` contains a space, which is invalid.
2. `const PI = 3.14; PI = 3.14159;` tries to reassign a `const`.
3. `let count` is declared twice in the same scope.

### Fixed Version

```javascript
let userName = "John";
let score = 0;
score = 10;

const PI = 3.14;
// PI = 3.14159; // Not allowed

let count = 0;
count = 1;
```

### Points

1 point per found bug.

---

## Part 5: Type Checking in Practice

### Live Code

```javascript
function checkType(value) {
  if (value === null) return "null";
  if (Array.isArray(value)) return "array";
  return typeof value;
}

console.log(checkType(null));           // "null"
console.log(checkType([1, 2, 3]));      // "array"
console.log(checkType({ a: 1 }));       // "object"
console.log(checkType(42));             // "number"
```

#### Challenge 5.1 — Better typeof (individual, 4 minutes)

- **Requirement:** Write `checkType(value)` that returns `"null"` for `null`, `"array"` for arrays, and `typeof` for everything else.
- **Time limit:** 4 minutes

---

## Part 6: Concatenation and Template Literals

### 6.1 Building Strings

#### Live Code

```javascript
let firstName = "John";
let lastName = "Doe";
let fullName = firstName + " " + lastName;
console.log(fullName);

let address = "123 Main St" + ", " + "New York" + " " + "10001";
console.log(address);
```

#### Challenge 6.1 — Address Builder (individual, 3 minutes)

- **Requirement:** Build a full address string from `street`, `city`, `zip`.
- **Time limit:** 3 minutes

### 6.2 Template Literals with Expressions

#### Live Code

```javascript
let price = 100;
let discount = 0.2;
let finalPrice = price * (1 - discount);

let message = `Original: $${price}, Discount: ${discount * 100}%, Final: $${finalPrice}`;
console.log(message);
```

#### Challenge 6.2 — Price Tag (individual, 4 minutes)

- **Requirement:** Use a template literal to show price, discount, and final price.
- **Time limit:** 4 minutes

### 6.3 User Profile Generator

#### Live Code

```javascript
function generateUserProfile(firstName, lastName, age, email, city) {
  let fullName = firstName + " " + lastName;
  return `
    USER PROFILE
    ============
    Name: ${fullName}
    Age: ${age}
    Email: ${email}
    City: ${city}

    ${fullName} is ${age} years old and lives in ${city}.
    Contact them at ${email}.
  `;
}

console.log(generateUserProfile("John", "Doe", 30, "john@example.com", "New York"));
```

#### Challenge 6.3 — Product Description (individual, 5 minutes)

- **Requirement:** Write `generateProductDescription(name, price, category, inStock)`.
- **Time limit:** 5 minutes
- **Hint:** Use a ternary for stock status.

---

## Bug Hunt 2

### Problem

Find the bugs in this string code.

```javascript
let price = 19.99;
let message = "The price is $" + price.toFixed(2)"!";
console.log(message);

let name = "John";
let age = 30;
let info = `My name is ${name} and I am ${Age} years old.`;
console.log(info);

let html = "
  <div>
    <h1>Hello</h1>
  </div>
";
console.log(html);
```

### Issues

1. `+ price.toFixed(2)"!"` is missing the `+` operator.
2. `${Age}` uses uppercase `Age` but the variable is `age`.
3. Multi-line string uses double quotes, which is not valid without `\n` or using template literals.

### Fixed Version

```javascript
let price = 19.99;
let message = "The price is $" + price.toFixed(2) + "!";
console.log(message);

let name = "John";
let age = 30;
let info = `My name is ${name} and I am ${age} years old.`;
console.log(info);

let html = `
  <div>
    <h1>Hello</h1>
  </div>
`;
console.log(html);
```

### Points

1 point per found issue.

---

## Group Challenge: Type Detective Race

- **Time:** 10 minutes
- **Teams:** 2 or 3 students per team
- **Task:** Each team writes the correct `checkType` output for a list of values.
- **Scoring:** 1 point per correct answer. First team to finish with all correct gets 2 bonus points.

### Values

```javascript
[ "Hello", 42, 3.14, true, null, undefined, {}, [], new Date(), Symbol("x"), 100n ]
```

### Instructor Answer Key

```javascript
"Hello"         -> "string"
42              -> "number"
3.14            -> "number"
true            -> "boolean"
null            -> "null" (not "object")
undefined       -> "undefined"
{}              -> "object"
[]              -> "array"
new Date()      -> "object"
Symbol("x")     -> "symbol"
100n            -> "bigint"
```

---

## Individual Challenges — Progressive Difficulty

### Level 1: Fix the Name (3 minutes)

- **Requirement:** These names are invalid or bad. Rewrite them: `let 1stName; let my-name; let CLASS; let total score;`.

### Level 2: typeof Output (3 minutes)

- **Requirement:** Predict the output of `typeof` for these values: `"42"`, `42`, `true`, `null`, `undefined`, `[]`, `{}`.

### Level 3: let or const (3 minutes)

- **Requirement:** Choose `let` or `const` for: `userName`, `PI`, `score`, `MAX_ATTEMPTS`, `email`.

### Level 4: Escape the Path (4 minutes)

- **Requirement:** Print `C:\Users\John\Documents` using one `console.log`.

### Level 5: String Slice (4 minutes)

- **Requirement:** Extract `"World"` from `"Hello, World!"` and convert it to uppercase.

### Level 6: Product Card (5 minutes)

- **Requirement:** Use a template literal to display product info from variables.

---

## Mini Project: Profile Card Generator

### Time

25 minutes

### Goal

Combine variables, data types, `typeof`, template literals, and string methods into one HTML page.

### Requirements for the Students

1. Create an HTML page with inputs:
   - First name
   - Last name
   - Age
   - Email
   - City
   - Job title

2. On a button click, display a formatted profile card using template literals.

3. Show the `typeof` each input value in the console.

### Starter HTML

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Profile Card Generator</title>
  <style>
    body { font-family: Arial, sans-serif; max-width: 600px; margin: 20px auto; }
    .section { background: #f9f9f9; padding: 15px; margin-bottom: 20px; border-radius: 4px; }
    input, button { padding: 8px; margin: 5px; }
    .card { background: white; border: 1px solid #ddd; padding: 20px; border-radius: 4px; }
  </style>
</head>
<body>
  <h1>Profile Card Generator</h1>

  <div class="section">
    <input type="text" id="firstName" placeholder="First name" value="John">
    <input type="text" id="lastName" placeholder="Last name" value="Doe">
    <input type="number" id="age" placeholder="Age" value="30">
    <input type="text" id="email" placeholder="Email" value="john@example.com">
    <input type="text" id="city" placeholder="City" value="New York">
    <input type="text" id="job" placeholder="Job title" value="Developer">
    <button onclick="generateCard()">Generate</button>
  </div>

  <div class="card" id="cardOutput">
    Your card will appear here...
  </div>

  <script>
    function generateCard() {
      let firstName = document.getElementById("firstName").value;
      let lastName = document.getElementById("lastName").value;
      let age = parseInt(document.getElementById("age").value);
      let email = document.getElementById("email").value;
      let city = document.getElementById("city").value;
      let job = document.getElementById("job").value;

      console.log("firstName:", typeof firstName);
      console.log("age:", typeof age);
      console.log("email:", typeof email);

      let fullName = firstName + " " + lastName;
      let initials = (firstName[0] + lastName[0]).toUpperCase();

      let card = `
        <h2>${fullName}</h2>
        <p><strong>Initials:</strong> ${initials}</p>
        <p><strong>Age:</strong> ${age}</p>
        <p><strong>Job:</strong> ${job}</p>
        <p><strong>Email:</strong> ${email}</p>
        <p><strong>City:</strong> ${city}</p>
      `;

      document.getElementById("cardOutput").innerHTML = card;
    }
  </script>
</body>
</html>
```

### Review Questions for the Mini Project

- "Why does `age` need `parseInt`?"
- "What happens if `firstName` is empty?"
- "How can we make the initials using `slice`?"

---

<details>
<summary>Trainer Solutions — Do Not Show Until Students Try</summary>

## Trainer Solutions — Do Not Show Until Students Try

### Challenge 1.2

```javascript
function isArray(value) {
  return Array.isArray(value);
}
```

### Challenge 2.2

```javascript
let score = 0;
score = 10;
console.log(score);
```

### Challenge 2.3

```javascript
const user = { name: "John", age: 30 };
user.name = "Jane"; // OK
// user = {};       // Error

const numbers = [1, 2, 3];
numbers.push(4);   // OK
// numbers = [];   // Error
```

### Challenge 2.4

- `userName` → `let`
- `totalScore` → `let`
- `PI` → `const`
- `apiKey` → `const`

### Challenge 3.1

- `firstName` → valid
- `2ndPlace` → invalid
- `my-variable` → invalid
- `class` → invalid
- `_score` → valid
- `$value` → valid

### Challenge 3.2

- `myname` → `myName`
- `totalscore` → `totalScore`
- `apikey` → `apiKey`
- `maxusers` → `MAX_USERS`

### Challenge 4.1

```javascript
let sentence1 = "It's a nice day";
let sentence2 = '"Hello," she said';
let sentence3 = 'He said, "It\'s fine"';
```

### Challenge 4.2

```javascript
console.log("Name: \"John\"\nAge:\t30");
```

### Challenge 4.3

```javascript
let text = "Hello, World!";
let word = text.slice(7, 12).toUpperCase();
console.log(word); // "WORLD"
```

### Challenge 4.4

```javascript
let productName = "Laptop";
let productPrice = 999.99;
let inStock = true;

let card = `
  Product: ${productName}
  Price: $${productPrice}
  In Stock: ${inStock}
`;
console.log(card);
```

### Challenge 5.1

```javascript
function checkType(value) {
  if (value === null) return "null";
  if (Array.isArray(value)) return "array";
  return typeof value;
}
```

### Challenge 6.1

```javascript
let street = "123 Main St";
let city = "New York";
let zip = "10001";
let address = street + ", " + city + " " + zip;
console.log(address);
```

### Challenge 6.2

```javascript
let price = 100;
let discount = 0.2;
let finalPrice = price * (1 - discount);
console.log(`Original: $${price}, Discount: ${discount * 100}%, Final: $${finalPrice}`);
```

### Challenge 6.3

```javascript
function generateProductDescription(name, price, category, inStock) {
  let status = inStock ? "In Stock" : "Out of Stock";
  return `
    PRODUCT: ${name}
    Price: $${price}
    Category: ${category}
    Status: ${status}
  `;
}
```

### Individual Challenges Solutions

```javascript
// Level 1
let firstName;
let myName;
let className; // or myClass
let totalScore;

// Level 2
"42"        -> "string"
42          -> "number"
true        -> "boolean"
null        -> "object" (typeof bug)
undefined   -> "undefined"
[]          -> "object"
{}          -> "object"

// Level 3
let userName;
const PI = 3.14;
let score;
const MAX_ATTEMPTS = 3;
let email;

// Level 4
console.log("C:\\Users\\John\\Documents");

// Level 5
let text = "Hello, World!";
console.log(text.slice(7, 12).toUpperCase());

// Level 6
let productName = "Book";
let productPrice = 19.99;
let inStock = true;
console.log(`Product: ${productName}\nPrice: $${productPrice}\nIn Stock: ${inStock}`);
```

</details>

---

## Review Questions

1. What does `typeof null` return?
   - [ ] "null"
   - [x] "object"
   - [ ] "undefined"
   - [ ] "string"

2. Which keyword should you use by default for values that won't change?
   - [ ] var
   - [ ] let
   - [x] const
   - [ ] All of the above

3. What is the correct way to include a variable in a string?
   - [x] `"Hello " + name`
   - [ ] `"Hello {name}"`
   - [x] `` `Hello ${name}` ``
   - [ ] Both A and C

4. Which escape sequence represents a new line?
   - [ ] `\t`
   - [x] `\n`
   - [ ] `\r`
   - [ ] `\b`

5. What is the Temporal Dead Zone?
   - [ ] A gaming term
   - [x] The period before a let/const variable is declared
   - [ ] A debugging tool
   - [ ] A type of error

6. Can you modify properties of a const object?
   - [x] Yes
   - [ ] No
   - [ ] Only if it's an array
   - [ ] Only if you use let

7. What is the naming convention for variables in JavaScript?
   - [ ] snake_case
   - [ ] PascalCase
   - [x] camelCase
   - [ ] kebab-case

8. How do you check if a value is an array?
   - [ ] `typeof value === "array"`
   - [ ] `value instanceof Array`
   - [x] `Array.isArray(value)`
   - [ ] Both B and C

9. What is the result of `"Hello" + " " + "World"`?
   - [x] `"Hello World"`
   - [ ] `"HelloWorld"`
   - [ ] `"Hello  World"`
   - [ ] Error

10. Which string method converts text to uppercase?
    - [ ] `toUpper()`
    - [x] `toUpperCase()`
    - [ ] `upperCase()`
    - [ ] `toUpperString()`

---

## Additional Resources

- [MDN: Data Types](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures)
- [MDN: typeof](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/typeof)
- [MDN: Template Literals](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals)
- [JavaScript.info: Variables](https://javascript.info/variables)
- [JavaScript.info: Strings](https://javascript.info/string)

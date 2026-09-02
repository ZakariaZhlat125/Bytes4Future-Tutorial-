# Session 7: Functions (Part 1) — Student Active-Learning Lab

Welcome to this session. The goal today is to learn how to write, call, and reuse **functions**. Do not just read — predict, type, run, and fix.

---

## 🧭 How to move through this session

1. **Read the problem first.**
2. **Stop.** Do not look at the code yet.
3. **Write your prediction** in a comment or notebook.
4. **Type the code and run it.**
5. **Compare, ask why, then change one thing.**
6. **Do the challenge before you look at the answer key.**

---

## Part 0: Warm-Up — The Repeated Code

### The problem

A program needs to greet three different people. A student copies the same code:

```javascript
console.log("Hello, John!");
console.log("Hello, Jane!");
console.log("Hello, Bob!");
```

### 🤔 Think

What if we need to greet `100` people? How many `console.log` lines would that be?

```text
Number of console.log lines needed for 100 people: __________
```

Is copying the line the best way?

### 🔮 Predict

Look at this code and write what you think will print:

```javascript
function greet(name) {
  console.log("Hello, " + name + "!");
}

greet("John");
greet("Jane");
greet("Bob");
```

### ✅ Result

Run the code.

### 🧠 Discover

A **function** is a reusable block of code. We define it once and call it whenever we need it.

- `name` inside the parentheses is a **parameter**.
- `"John"`, `"Jane"`, `"Bob"` are the **arguments** passed in.

### 🧪 Experiment

Add `greet("You");` at the end. What happens? What happens if you call `greet();` with no argument?

---

## Part 1: Function Declarations and Calls

### 1.1 Basic Function

### The problem

Add two numbers. We want to reuse the addition.

### 🔮 Predict

```javascript
function add(a, b) {
  return a + b;
}

let result = add(5, 3);
console.log(result);        // ?
console.log(add(10, 20));   // ?
```

What will the two `console.log` lines print?

### ✅ Result

Run it.

### 🧠 Why?

`return` sends a value back to the caller. `result` gets `8`. `add(10, 20)` returns `30`, which is passed directly to `console.log`.

### Challenge 1.1 — Multiply

Write a function `multiply(a, b)` that returns the product.

```javascript
// your code here
```

---

### 1.2 Function Without Return

### The problem

A function only prints a message. What happens if we try to save its result?

### 🔮 Predict

```javascript
function logMessage(message) {
  console.log(message);
}

let output = logMessage("Hello");
console.log(output);
```

What will the second `console.log` print?

### ✅ Result

Run it.

### 🧠 Why?

If a function does not use `return`, it gives back `undefined`. `console.log` does **not** give a result back to the code — it only prints.

### Challenge 1.2 — Return vs Log

Predict what `output` will be, then run the code. Explain why.

```javascript
// your code here
```

---

### 1.3 Multiple Parameters

### The problem

Build a full name from two or three parts.

### 🔮 Predict

```javascript
function createFullName(firstName, lastName, middleName) {
  if (middleName) {
    return `${firstName} ${middleName} ${lastName}`;
  }
  return `${firstName} ${lastName}`;
}

console.log(createFullName("John", "Doe"));
console.log(createFullName("John", "Doe", "William"));
```

What will each line print?

### ✅ Result

Run it.

### 🧠 Why?

When a `return` is reached, the function stops immediately. The second `return` only runs if `middleName` is missing.

### Challenge 1.3 — Full Name

Write a function `getInitials(firstName, lastName)` that returns `"J. Doe"`.

```javascript
// your code here
```

---

## Part 2: Return Statement

### 2.1 Single Return

### The problem

A function should give back a result that we can use later.

### 🔮 Predict

```javascript
function square(num) {
  return num * num;
}

let area = square(4);
console.log(area);
```

What is `area`? What does `console.log` print?

### ✅ Result

Run it.

### 🧠 Why?

`return` lets the function output a value. `square(4)` becomes `16`.

### Challenge 2.1 — Rectangle Area

Write a function `rectangleArea(width, height)` that returns the area.

```javascript
// your code here
```

---

### 2.2 Multiple Returns

### The problem

Categorize a number as positive, negative, or zero.

### 🔮 Predict

```javascript
function getNumberStatus(num) {
  if (num > 0) return "positive";
  if (num < 0) return "negative";
  return "zero";
}

console.log(getNumberStatus(5));
console.log(getNumberStatus(-5));
console.log(getNumberStatus(0));
```

What will each line print?

### ✅ Result

Run it.

### 🧠 Why?

This is called **early return**. As soon as a condition is met, the function stops. The last `return` is the default.

### Challenge 2.2 — Grade

Write a function `getGrade(score)` with early returns for invalid scores and grades A, B, C, D, F.

```javascript
// your code here
```

---

### 2.3 Returning Different Types

### The problem

A function can give back numbers, strings, booleans, arrays, or objects.

### 🔮 Predict

```javascript
function getValue(type) {
  switch (type) {
    case "number": return 42;
    case "string": return "Hello";
    case "boolean": return true;
    case "array": return [1, 2, 3];
    case "object": return { key: "value" };
    default: return null;
  }
}

console.log(typeof getValue("number"));
console.log(Array.isArray(getValue("array")));
console.log(getValue("banana"));
```

What will each line print?

### ✅ Result

Run it.

### Challenge 2.3 — Describe Value

Write `describe(value)` that returns `"number"`, `"string"`, `"boolean"`, or `"other"` using `typeof`.

```javascript
// your code here
```

---

## Part 3: Default Parameters

### 3.1 Basic Defaults

### The problem

A greeting function should work even if the caller does not pass a name.

### 🔮 Predict

```javascript
function greet(name = "Guest") {
  console.log(`Hello, ${name}!`);
}

greet("John");
greet();
```

What will the second `greet()` print?

### ✅ Result

Run it.

### 🧠 Why?

`name = "Guest"` is a **default parameter**. If no argument is given, `name` becomes `"Guest"`.

### Challenge 3.1 — Default Country

Write `createUser(name, country = "Unknown")` that returns an object.

```javascript
// your code here
```

---

### 3.2 Multiple Defaults

### The problem

Set defaults for many parameters.

### 🔮 Predict

```javascript
function createUser(name = "Anonymous", age = 0, country = "Unknown") {
  return { name, age, country };
}

console.log(createUser("John", 30, "USA"));
console.log(createUser("John", 30));
console.log(createUser("John"));
console.log(createUser());
```

For the third and fourth calls, what will `age` and `country` be?

### ✅ Result

Run it.

### 🧠 Why?

If an argument is not given, the default is used. Each parameter gets its own default.

### Challenge 3.2 — Default Price

Write `calculateTotal(price, quantity = 1, tax = 0.1)` that returns the total.

```javascript
// your code here
```

**Hint:** `price * quantity * (1 + tax)`

---

### 3.3 Defaults with `undefined` and Other Falsy Values

### The problem

When does the default actually kick in?

### 🔮 Predict

```javascript
function setVolume(value = 50) {
  console.log(value);
}

setVolume();
setVolume(undefined);
setVolume(null);
setVolume(0);
setVolume("");
```

For each line, what do you think will print? Will they all be `50`?

### ✅ Result

Run it.

### 🧠 Why?

Only `undefined` triggers the default. `null`, `0`, and `""` are real values that you passed in.

### 🧪 Experiment

Call `setVolume(false)` and `setVolume(NaN)`. Does the default work?

---

## Part 4: Rest Parameters

### 4.1 Basic Rest

### The problem

Add any number of values without knowing how many in advance.

### 🔮 Predict

```javascript
function sumAll(...numbers) {
  let sum = 0;
  for (let num of numbers) {
    sum += num;
  }
  return sum;
}

console.log(sumAll(1, 2, 3));
console.log(sumAll(1, 2, 3, 4, 5));
console.log(sumAll());
```

What is `numbers` inside the function? What does `sumAll()` return?

### ✅ Result

Run it.

### 🧠 Why?

`...numbers` is a **rest parameter**. It turns all the remaining arguments into an array.

### Challenge 4.1 — Average

Write `average(...numbers)` that returns the average.

```javascript
// your code here
```

---

### 4.2 Rest with Regular Parameters

### The problem

Use a normal parameter plus a rest parameter.

### 🔮 Predict

```javascript
function greetAll(greeting, ...names) {
  names.forEach(name => {
    console.log(`${greeting}, ${name}!`);
  });
}

greetAll("Hello", "John", "Jane", "Bob");
```

How many lines will print? What is `greeting`?

### ✅ Result

Run it.

### Challenge 4.2 — Build HTML

Write `buildHTML(tag, ...content)` that returns `<tag>content joined</tag>`.

```javascript
// your code here
```

**Hint:** `content.join("")`

---

### 4.3 Rest vs `arguments`

### The problem

Older JavaScript used `arguments`. Modern JavaScript uses rest parameters.

### 🔮 Predict

```javascript
// Old way
function oldSum() {
  let sum = 0;
  for (let i = 0; i < arguments.length; i++) {
    sum += arguments[i];
  }
  return sum;
}

// New way
function newSum(...numbers) {
  return numbers.reduce((sum, num) => sum + num, 0);
}

console.log(oldSum(1, 2, 3, 4, 5));
console.log(newSum(1, 2, 3, 4, 5));
```

Will both give the same answer? What is `arguments`?

### ✅ Result

Run it.

### 🧪 Experiment

Change `newSum` to `newSum(1, 2, 3, 4, 5, 6)`. Does it still work? Does `oldSum` also work?

---

## Bug Hunt 1

### The mission

Find the bugs in this function. Do not run it yet. Read and write what you think is wrong.

```javascript
function calculateDiscount(price, discount = 10) {
  if (price < 0) {
    return "Invalid";
  }
  let final = price - (price * discount / 100);
  console.log(final);
}

let result = calculateDiscount(100);
console.log("Result:", result);
```

### 🐛 What I think is wrong

1. _______________________________________________________________
2. _______________________________________________________________

### ✅ Fixed version

Write your fixed version, then test it.

```javascript
// your fixed version here
```

---

## Part 5: Anonymous Functions and Function Expressions

### 5.1 Function Expression

### The problem

A function can be assigned to a variable.

### 🔮 Predict

```javascript
const multiply = function(a, b) {
  return a * b;
};

console.log(multiply(5, 3));
```

Will `multiply(5, 3)` give `15`?

### ✅ Result

Run it.

### 🧠 Why?

This is a **function expression**. The variable `multiply` holds the function.

### Challenge 5.1 — Anonymous Greeting

Create an anonymous function assigned to `greet` and call it.

```javascript
// your code here
```

---

### 5.2 IIFE

### The problem

Run a function immediately to avoid global variables.

### 🔮 Predict

```javascript
(function() {
  console.log("This function runs immediately!");
})();

(function(name) {
  console.log(`Hello, ${name}!`);
})("John");

const result = (function(a, b) {
  return a + b;
})(5, 3);
console.log(result);
```

How many outputs will you see? What will `result` be?

### ✅ Result

Run it.

### 🧠 Why?

An **IIFE** is an "Immediately Invoked Function Expression." It runs right after it is defined.

### Challenge 5.2 — IIFE Sum

Write an IIFE that prints the product of `7` and `8`.

```javascript
// your code here
```

---

### 5.3 Functions in Objects

### The problem

A calculator can be an object with several functions inside.

### 🔮 Predict

```javascript
const calculator = {
  add: function(a, b) { return a + b; },
  subtract: function(a, b) { return a - b; },
  multiply: function(a, b) { return a * b; },
  divide: function(a, b) {
    if (b === 0) return "Cannot divide by zero";
    return a / b;
  }
};

console.log(calculator.add(5, 3));
console.log(calculator.divide(10, 0));
```

What will the second `console.log` print?

### ✅ Result

Run it.

### 🧠 Why?

Functions can be values inside objects. You call them with `objectName.methodName()`.

### Challenge 5.3 — Object of Operations

Create an object `math` with `power` and `root` functions.

```javascript
// your code here
```

---

## Part 6: Nested Functions and Closures

### 6.1 Returning a Function

### The problem

Create a function that makes other greeting functions.

### 🔮 Predict

```javascript
function createGreeter(greeting) {
  return function(name) {
    return `${greeting}, ${name}!`;
  };
}

const sayHello = createGreeter("Hello");
const sayGoodbye = createGreeter("Goodbye");

console.log(sayHello("John"));
console.log(sayGoodbye("John"));
```

What does `sayHello` become? What does it print when called?

### ✅ Result

Run it.

### 🧠 Why?

A function can **return** another function. The inner function remembers the value of `greeting` from when it was created.

### Challenge 6.1 — Function Factory

Write `createMultiplier(factor)` that returns a function.

```javascript
// your code here
```

---

### 6.2 Counter with Closure

### The problem

Make a counter that keeps its own private count.

### 🔮 Predict

```javascript
function createCounter() {
  let count = 0;

  return {
    increment: function() { return ++count; },
    decrement: function() { return --count; },
    getCount: function() { return count; }
  };
}

const counter1 = createCounter();
const counter2 = createCounter();

console.log(counter1.increment()); // ?
console.log(counter1.increment()); // ?
console.log(counter2.increment()); // ?
console.log(counter1.getCount());  // ?
```

What will each line print? Is `counter2` connected to `counter1`?

### ✅ Result

Run it.

### 🧠 Why?

This is a **closure**. The inner functions remember the `count` variable. Each `createCounter()` call makes a new private `count`.

### Challenge 6.2 — Bank Account

Write `createBankAccount(balance)` that returns `deposit(amount)`, `withdraw(amount)`, and `getBalance()`.

```javascript
// your code here
```

**Hint:** Use closure to keep `balance` private.

---

### 6.3 Private Data

### The problem

A bank account should not let anyone directly change the balance from outside.

### 🔮 Predict

```javascript
function createBankAccount(initialBalance) {
  let balance = initialBalance;

  return {
    deposit(amount) {
      if (amount > 0) {
        balance += amount;
        return `Deposited $${amount}. New balance: $${balance}`;
      }
      return "Invalid deposit";
    },
    withdraw(amount) {
      if (amount > 0 && amount <= balance) {
        balance -= amount;
        return `Withdrew $${amount}. New balance: $${balance}`;
      }
      return "Invalid withdrawal";
    },
    getBalance() {
      return balance;
    }
  };
}

const account = createBankAccount(100);
console.log(account.deposit(50));
console.log(account.withdraw(30));
console.log(account.getBalance());
```

What will each line print? What happens if you write `account.balance = 1000` after this?

### ✅ Result

Run it.

### 🧪 Experiment

Try `console.log(account.balance)`. What do you get? Try `account.deposit(-100)`. What happens?

---

## Part 7: Arrow Functions

### 7.1 Basic Arrow

### The problem

Write shorter functions.

### 🔮 Predict

```javascript
// Traditional
function add(a, b) {
  return a + b;
}

// Arrow
const addArrow = (a, b) => {
  return a + b;
};

// Concise
const addConcise = (a, b) => a + b;

console.log(addConcise(5, 3));
```

Will `addConcise` give the same result as `add`?

### ✅ Result

Run it.

### 🧠 Why?

Arrow functions can be written with `=>`. If the body is one expression, you can leave out `return` and the braces.

### Challenge 7.1 — Arrow Square

Write `const square = ...` as a concise arrow function.

```javascript
// your code here
```

---

### 7.2 Single Parameter and No Parameters

### The problem

Arrow functions can have even shorter syntax.

### 🔮 Predict

```javascript
const square = num => num * num;
const sayHello = () => console.log("Hello!");

console.log(square(4));
sayHello();
```

Will `square(4)` work? Will `sayHello()` print?

### ✅ Result

Run it.

### 🧠 Why?

With a single parameter, you can drop the parentheses. With no parameters, you need empty parentheses `()`.

### Challenge 7.2 — Arrow Greeting

Write `const greet = name => ...` that returns a string.

```javascript
// your code here
```

---

### 7.3 Returning Objects

### The problem

Return an object from a concise arrow function.

### 🔮 Predict

```javascript
const createUser = (name, age) => ({ name, age });
console.log(createUser("John", 30));
```

Why are there parentheses around `{ name, age }`?

### ✅ Result

Run it.

### 🧠 Why?

Without the parentheses, `{ name, age }` would look like a function body with a block, not an object.

### Challenge 7.3 — Arrow Object

Write `const makeBook = (title, pages) => ...` that returns `{ title, pages }`.

```javascript
// your code here
```

---

### 7.4 Arrow as Callback

### The problem

Arrow functions work great as short callbacks.

### 🔮 Predict

```javascript
const numbers = [1, 2, 3, 4, 5];

const doubled = numbers.map(function(num) {
  return num * 2;
});

const doubledArrow = numbers.map(num => num * 2);
console.log(doubledArrow);
```

What will `doubledArrow` be?

### ✅ Result

Run it.

### 🧠 Why?

`map` runs a function for every item in an array. Arrow functions make this much shorter.

### Challenge 7.4 — Filter Adults

Use an arrow function with `filter` to return users aged `18` or older.

```javascript
let users = [
  { name: "John", age: 30 },
  { name: "Jane", age: 16 },
  { name: "Bob", age: 21 }
];

// your code here
```

---

## Part 8: Practical Function Projects

### 8.1 Calculator

### 🔮 Predict

```javascript
function calculator(operation, ...numbers) {
  if (numbers.length === 0) return "No numbers provided";

  switch (operation) {
    case "add":
      return numbers.reduce((sum, num) => sum + num, 0);
    case "subtract":
      return numbers.reduce((result, num) => result - num);
    case "multiply":
      return numbers.reduce((product, num) => product * num, 1);
    case "divide":
      if (numbers.includes(0)) return "Cannot divide by zero";
      return numbers.reduce((result, num) => result / num);
    default:
      return "Invalid operation";
  }
}

console.log(calculator("add", 1, 2, 3));
console.log(calculator("multiply", 2, 3, 4));
console.log(calculator("divide", 100, 2, 5));
```

What will each line print?

### ✅ Result

Run it.

### 🧪 Experiment

Call `calculator("subtract", 10, 2, 3)`. What is the result? Is it what you expected? Why?

---

### 8.2 Validator

### 🔮 Predict

```javascript
function validateUser(user) {
  const errors = [];

  if (!user.name || user.name.trim() === "") {
    errors.push("Name is required");
  }

  if (!user.email || !user.email.includes("@")) {
    errors.push("Valid email is required");
  }

  if (!user.age || user.age < 0 || user.age > 120) {
    errors.push("Valid age (0-120) is required");
  }

  return {
    isValid: errors.length === 0,
    errors
  };
}

console.log(validateUser({ name: "John", email: "john@example.com", age: 30 }));
console.log(validateUser({ name: "", email: "invalid", age: -5 }));
```

What will each `console.log` show?

### ✅ Result

Run it.

### 🧠 Why?

The function builds an `errors` array and returns an object with two properties.

---

### 8.3 Flexible Calculator

### 🔮 Predict

```javascript
function flexibleCalculator(...args) {
  if (args.length === 0) return 0;

  const operation = args[0];
  const numbers = args.slice(1);

  if (numbers.length === 0) return 0;

  switch (operation) {
    case "+":
      return numbers.reduce((sum, n) => sum + n, 0);
    case "-":
      return numbers.reduce((result, n) => result - n);
    case "*":
      return numbers.reduce((product, n) => product * n, 1);
    case "/":
      return numbers.reduce((result, n) => result / n);
    default:
      return "Invalid operation";
  }
}

console.log(flexibleCalculator("+", 1, 2, 3, 4));
console.log(flexibleCalculator("*", 2, 3, 4));
```

What is `args`? What does `args.slice(1)` do?

### ✅ Result

Run it.

### 🧪 Experiment

Call `flexibleCalculator("-", 20, 5, 2)`. What do you get? Why?

---

## Bug Hunt 2

### The mission

Find the bugs in this closure code.

```javascript
function createCounter() {
  let count = 0;

  return {
    increment: function() {
      count++;
    },
    getCount: function() {
      return count;
    }
  };
}

let c = createCounter();
c.increment();
c.increment();
console.log(c.count);
```

### 🐛 What I think is wrong

1. _______________________________________________________________
2. _______________________________________________________________

### ✅ Fixed version

Write your fixed version, then test it.

```javascript
// your fixed version here
```

---

## Group Challenge: Function Builder

If you are working in a group, split into teams of 2 or 3.

Each team must write one small function for each situation.

### Situations

1. `celsiusToFahrenheit(c)`
2. `getInitials(firstName, lastName)`
3. `sumEven(...numbers)`
4. `createMultiplier(factor)`
5. `validateEmail(email)`

Set a timer for 10 minutes. The first team with all five correct functions wins.

---

## Individual Challenges — Progressive Difficulty

Do these in order. Do not look at the answer key until you have tried.

### Level 1: Add and Multiply

Write `add(a, b)` and `multiply(a, b)`.

```javascript
// your code here
```

### Level 2: Default Tax

Write `calculateTotal(price, tax = 0.1)`.

```javascript
// your code here
```

### Level 3: Sum Rest

Write `sum(...numbers)` that adds all arguments.

```javascript
// your code here
```

### Level 4: Arrow Average

Write `const average = ...` as an arrow function.

```javascript
// your code here
```

### Level 5: Validator Function

Write `validatePassword(password)` that returns `true` if the password has at least `8` characters.

```javascript
// your code here
```

### Level 6: Bank Account

Write a closure-based bank account with `deposit`, `withdraw`, and `getBalance`.

```javascript
// your code here
```

---

## Mini Project: Function Dashboard

### Time

25 minutes

### Goal

Combine function declarations, parameters, `return`, defaults, rest, arrow functions, and a simple closure in one HTML page.

### Requirements

1. Create an HTML page with sections:
   - **Greeting:** input for name, optional age; output a personalized greeting.
   - **Calculator:** inputs for numbers and operation; output the result.
   - **Counter:** buttons to `+`, `-`, and `show` the count using a closure.

2. Use these concepts:
   - Function declaration
   - Default parameters
   - Rest parameters
   - Arrow function as callback
   - Closure for the counter

### Starter HTML

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Function Dashboard</title>
  <style>
    body { font-family: Arial, sans-serif; max-width: 700px; margin: 20px auto; }
    .section { background: #f9f9f9; padding: 15px; margin-bottom: 20px; border-radius: 4px; }
    input, select, button { padding: 8px; margin: 5px; }
    .output { background: #f0f0f0; padding: 15px; border-radius: 4px; margin-top: 10px; }
  </style>
</head>
<body>
  <h1>Function Dashboard</h1>

  <div class="section">
    <h2>Greeting</h2>
    <input type="text" id="greetName" placeholder="Name" value="John">
    <input type="number" id="greetAge" placeholder="Age">
    <button onclick="showGreeting()">Greet</button>
    <div id="greetingOutput" class="output"></div>
  </div>

  <div class="section">
    <h2>Calculator</h2>
    <input type="text" id="calcNumbers" placeholder="Numbers (comma separated)" value="1,2,3,4">
    <select id="calcOp">
      <option value="add">Add</option>
      <option value="subtract">Subtract</option>
      <option value="multiply">Multiply</option>
    </select>
    <button onclick="showCalc()">Calculate</button>
    <div id="calcOutput" class="output"></div>
  </div>

  <div class="section">
    <h2>Counter</h2>
    <button onclick="counter.increment(); showCounter()">+</button>
    <button onclick="counter.decrement(); showCounter()">-</button>
    <div class="output" id="counterOutput">0</div>
  </div>

  <script>
    // Greeting
    const makeGreeting = (name = "Guest", age) => {
      if (age && age >= 18) return `Hello, ${name}. You are an adult.`;
      return `Hello, ${name}.`;
    };

    function showGreeting() {
      let name = document.getElementById("greetName").value.trim();
      let age = parseInt(document.getElementById("greetAge").value);
      let text = makeGreeting(name || undefined, isNaN(age) ? undefined : age);
      document.getElementById("greetingOutput").textContent = text;
    }

    // Calculator
    function calculate(operation, ...numbers) {
      switch (operation) {
        case "add": return numbers.reduce((a, b) => a + b, 0);
        case "subtract": return numbers.reduce((a, b) => a - b);
        case "multiply": return numbers.reduce((a, b) => a * b, 1);
        default: return "Invalid";
      }
    }

    function showCalc() {
      let input = document.getElementById("calcNumbers").value;
      let numbers = input.split(",").map(n => parseFloat(n.trim())).filter(n => !isNaN(n));
      let op = document.getElementById("calcOp").value;
      let result = calculate(op, ...numbers);
      document.getElementById("calcOutput").textContent = `Result: ${result}`;
    }

    // Counter with closure
    function createCounter() {
      let count = 0;
      return {
        increment: () => count++,
        decrement: () => count--,
        getCount: () => count
      };
    }

    const counter = createCounter();

    function showCounter() {
      document.getElementById("counterOutput").textContent = counter.getCount();
    }
  </script>
</body>
</html>
```

### Questions to think about

1. What is the difference between `name || undefined` and just `name` in the greeting?
2. Which function uses rest parameters?
3. Why is the counter shared between button clicks?
4. What happens if `showCalc` receives an empty input string?

### Extension ideas

- Add a `divide` option to the calculator.
- Validate the password input before greeting.
- Add a reset button for the counter.

---

## Review Questions

Answer these before you finish.

1. What is the correct syntax for a function declaration?
   - [ ] `function add(a, b) { return a + b; }`
   - [ ] `const add = function(a, b) { return a + b; }`
   - [ ] `const add = (a, b) => a + b;`
   - [ ] All of the above

2. What does a function return if no return statement is provided?
   - [ ] null
   - [ ] undefined
   - [ ] 0
   - [ ] Error

3. How do you set a default parameter value?
   - [ ] `function greet(name = "Guest") {}`
   - [ ] `function name = "Guest" {}`
   - [ ] `function name(default = "Guest") {}`
   - [ ] `function name("Guest") {}`

4. What symbol is used for rest parameters?
   - [ ] `*`
   - [ ] `...`
   - [ ] `&`
   - [ ] `#`

5. What is a closure?
   - [ ] A way to close functions
   - [ ] A function with access to outer scope variables
   - [ ] A type of loop
   - [ ] A method to end execution

6. Which arrow function syntax is correct for a single parameter?
   - [ ] `(num) => num * 2`
   - [ ] `num => num * 2`
   - [ ] `=> num * 2`
   - [ ] `num -> num * 2`

7. Can you have multiple rest parameters in one function?
   - [ ] Yes
   - [ ] No
   - [ ] Only if they're the same type
   - [ ] Only in arrow functions

8. What is an IIFE?
   - [ ] A function that returns immediately
   - [ ] A function that runs immediately after definition
   - [ ] A function inside another function
   - [ ] A function with no parameters

9. How do you return an object in a concise arrow function?
   - [ ] `=> { key: value }`
   - [ ] `=> ({ key: value })`
   - [ ] `=> key: value`
   - [ ] `=> return { key: value }`

10. What is the main advantage of arrow functions?
    - [ ] They're always faster
    - [ ] They have a shorter syntax and lexical `this`
    - [ ] They can be hoisted
    - [ ] They support more features

---

## Additional Resources

- [MDN: Functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Functions)
- [MDN: Default Parameters](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Default_parameters)
- [MDN: Rest Parameters](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/rest_parameters)
- [MDN: Arrow Functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions)
- [JavaScript.info: Functions](https://javascript.info/function-basics)
- [JavaScript.info: Arrow Functions](https://javascript.info/arrow-functions-basics)

---

<details>
<summary>Answer Key — Try everything first!</summary>

### Challenge 1.1

```javascript
function multiply(a, b) {
  return a * b;
}
```

### Challenge 1.2

`output` is `undefined` because `logMessage` does not return a value.

### Challenge 1.3

```javascript
function getInitials(firstName, lastName) {
  return `${firstName[0].toUpperCase()}. ${lastName}`;
}
```

### Challenge 2.1

```javascript
function rectangleArea(width, height) {
  return width * height;
}
```

### Challenge 2.2

```javascript
function getGrade(score) {
  if (score < 0 || score > 100) return "Invalid";
  if (score >= 90) return "A";
  if (score >= 80) return "B";
  if (score >= 70) return "C";
  if (score >= 60) return "D";
  return "F";
}
```

### Challenge 2.3

```javascript
function describe(value) {
  if (typeof value === "number") return "number";
  if (typeof value === "string") return "string";
  if (typeof value === "boolean") return "boolean";
  return "other";
}
```

### Challenge 3.1

```javascript
function createUser(name, country = "Unknown") {
  return { name, country };
}
```

### Challenge 3.2

```javascript
function calculateTotal(price, quantity = 1, tax = 0.1) {
  return price * quantity * (1 + tax);
}
```

### Challenge 4.1

```javascript
function average(...numbers) {
  if (numbers.length === 0) return 0;
  return numbers.reduce((sum, n) => sum + n, 0) / numbers.length;
}
```

### Challenge 4.2

```javascript
function buildHTML(tag, ...content) {
  return `<${tag}>${content.join("")}</${tag}>`;
}
```

### Challenge 5.1

```javascript
const greet = function(name) {
  return `Hello, ${name}!`;
};
```

### Challenge 5.2

```javascript
(() => console.log(7 * 8))();
```

### Challenge 5.3

```javascript
const math = {
  power: (a, b) => Math.pow(a, b),
  root: a => Math.sqrt(a)
};
```

### Challenge 6.1

```javascript
function createMultiplier(factor) {
  return n => n * factor;
}
```

### Challenge 6.2

```javascript
function createBankAccount(balance) {
  return {
    deposit(amount) {
      if (amount > 0) balance += amount;
      return balance;
    },
    withdraw(amount) {
      if (amount > 0 && amount <= balance) balance -= amount;
      return balance;
    },
    getBalance() {
      return balance;
    }
  };
}
```

### Challenge 7.1

```javascript
const square = num => num * num;
```

### Challenge 7.2

```javascript
const greet = name => `Hello, ${name}!`;
```

### Challenge 7.3

```javascript
const makeBook = (title, pages) => ({ title, pages });
```

### Challenge 7.4

```javascript
let users = [
  { name: "John", age: 30 },
  { name: "Jane", age: 16 }
];
let adults = users.filter(u => u.age >= 18);
console.log(adults);
```

### Bug Hunt 1 Fixed Version

```javascript
function calculateDiscount(price, discount = 10) {
  if (price < 0) {
    return "Invalid price";
  }
  let final = price - (price * discount / 100);
  return final;
}

console.log(calculateDiscount(100));        // 90
console.log(calculateDiscount(100, 20));    // 80
console.log(calculateDiscount(-50));        // "Invalid price"
```

### Bug Hunt 2 Fixed Version

```javascript
function createCounter() {
  let count = 0;

  return {
    increment: function() {
      return ++count;
    },
    getCount: function() {
      return count;
    }
  };
}

let c = createCounter();
console.log(c.increment()); // 1
console.log(c.increment()); // 2
console.log(c.getCount());  // 2
```

### Individual Challenges Solutions

```javascript
// Level 1
function add(a, b) { return a + b; }
function multiply(a, b) { return a * b; }

// Level 2
function calculateTotal(price, tax = 0.1) {
  return price * (1 + tax);
}

// Level 3
function sum(...numbers) {
  return numbers.reduce((s, n) => s + n, 0);
}

// Level 4
const average = (...numbers) =>
  numbers.length === 0 ? 0 : numbers.reduce((s, n) => s + n, 0) / numbers.length;

// Level 5
function validatePassword(password) {
  return password.length >= 8;
}

// Level 6
function createBankAccount(balance) {
  return {
    deposit(amount) { if (amount > 0) balance += amount; return balance; },
    withdraw(amount) { if (amount > 0 && amount <= balance) balance -= amount; return balance; },
    getBalance() { return balance; }
  };
}
```

### Group Challenge Answer Key

```javascript
function celsiusToFahrenheit(c) {
  return (c * 9 / 5) + 32;
}

function getInitials(firstName, lastName) {
  return `${firstName[0].toUpperCase()}. ${lastName}`;
}

function sumEven(...numbers) {
  return numbers.filter(n => n % 2 === 0).reduce((sum, n) => sum + n, 0);
}

function createMultiplier(factor) {
  return n => n * factor;
}

function validateEmail(email) {
  if (!email) return "Email is required";
  if (!email.includes("@")) return "Email must contain @";
  if (!email.includes(".")) return "Email must contain .";
  return "Valid";
}
```

</details>

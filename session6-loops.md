# Session 6: Loops

## 📚 Theory (1h)

### For Loop Concept

Loops allow you to execute code repeatedly. The `for` loop is the most common loop in JavaScript.

#### Basic For Loop Syntax

```javascript
for (initialization; condition; increment) {
    // code to execute
}
```

#### For Loop Components

1. **Initialization**: Runs once at the beginning
2. **Condition**: Checked before each iteration
3. **Increment**: Runs after each iteration
4. **Body**: Code executed while condition is true

#### Basic For Loop Example

```javascript
for (let i = 0; i < 5; i++) {
    console.log("Iteration:", i);
}

// Output:
// Iteration: 0
// Iteration: 1
// Iteration: 2
// Iteration: 3
// Iteration: 4
```

#### For Loop Execution Flow

```javascript
for (let i = 0; i < 3; i++) {
    console.log("i =", i);
}

// Step-by-step:
// 1. Initialize: i = 0
// 2. Check condition: 0 < 3 (true) → Execute body
// 3. Increment: i = 1
// 4. Check condition: 1 < 3 (true) → Execute body
// 5. Increment: i = 2
// 6. Check condition: 2 < 3 (true) → Execute body
// 7. Increment: i = 3
// 8. Check condition: 3 < 3 (false) → Exit loop
```

### Looping on Sequences

#### Looping Through Arrays

```javascript
let fruits = ["apple", "banana", "orange"];

// Method 1: Traditional for loop
for (let i = 0; i < fruits.length; i++) {
    console.log(fruits[i]);
}

// Method 2: for...of loop (modern)
for (let fruit of fruits) {
    console.log(fruit);
}

// Method 3: forEach method
fruits.forEach(fruit => {
    console.log(fruit);
});
```

#### Looping Through Strings

```javascript
let text = "Hello";

// Traditional for loop
for (let i = 0; i < text.length; i++) {
    console.log(text[i]);
}

// for...of loop
for (let char of text) {
    console.log(char);
}
```

#### Looping Through Object Properties

```javascript
let user = {
    name: "John",
    age: 30,
    city: "New York"
};

// for...in loop (for object properties)
for (let key in user) {
    console.log(key + ":", user[key]);
}

// Using Object.keys() with for...of
for (let key of Object.keys(user)) {
    console.log(key + ":", user[key]);
}
```

#### Looping Through Numbers (Ranges)

```javascript
// Loop from 1 to 10
for (let i = 1; i <= 10; i++) {
    console.log(i);
}

// Loop from 10 down to 1
for (let i = 10; i >= 1; i--) {
    console.log(i);
}

// Loop with custom step
for (let i = 0; i <= 10; i += 2) {
    console.log(i); // 0, 2, 4, 6, 8, 10
}
```

### Nested Loops

Nested loops are loops inside other loops. The inner loop runs completely for each iteration of the outer loop.

#### Basic Nested Loop

```javascript
for (let i = 0; i < 3; i++) {
    for (let j = 0; j < 3; j++) {
        console.log(`i: ${i}, j: ${j}`);
    }
}

// Output:
// i: 0, j: 0
// i: 0, j: 1
// i: 0, j: 2
// i: 1, j: 0
// i: 1, j: 1
// i: 1, j: 2
// i: 2, j: 0
// i: 2, j: 1
// i: 2, j: 2
```

#### Nested Loop with Arrays

```javascript
let matrix = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
];

for (let i = 0; i < matrix.length; i++) {
    for (let j = 0; j < matrix[i].length; j++) {
        console.log(`matrix[${i}][${j}] = ${matrix[i][j]}`);
    }
}
```

#### Multiplication Table Example

```javascript
// Generate multiplication table
for (let i = 1; i <= 10; i++) {
    let row = "";
    for (let j = 1; j <= 10; j++) {
        row += (i * j).toString().padStart(4, " ");
    }
    console.log(row);
}
```

#### Pattern Printing

```javascript
// Print triangle pattern
for (let i = 1; i <= 5; i++) {
    let line = "";
    for (let j = 1; j <= i; j++) {
        line += "* ";
    }
    console.log(line);
}

// Output:
// * 
// * * 
// * * * 
// * * * * 
// * * * * * 
```

### Loop Control

#### break Statement

The `break` statement exits the loop immediately.

```javascript
for (let i = 0; i < 10; i++) {
    if (i === 5) {
        break; // Exit loop when i equals 5
    }
    console.log(i);
}

// Output: 0, 1, 2, 3, 4
```

#### continue Statement

The `continue` statement skips the current iteration and continues with the next.

```javascript
for (let i = 0; i < 10; i++) {
    if (i % 2 === 0) {
        continue; // Skip even numbers
    }
    console.log(i);
}

// Output: 1, 3, 5, 7, 9
```

#### Labels

Labels allow you to break or continue from specific loops, especially useful with nested loops.

```javascript
outer: for (let i = 0; i < 3; i++) {
    for (let j = 0; j < 3; j++) {
        if (i === 1 && j === 1) {
            break outer; // Break from outer loop
        }
        console.log(`i: ${i}, j: ${j}`);
    }
}

// Output:
// i: 0, j: 0
// i: 0, j: 1
// i: 0, j: 2
// i: 1, j: 0
```

#### continue with Labels

```javascript
outer: for (let i = 0; i < 3; i++) {
    for (let j = 0; j < 3; j++) {
        if (i === 1 && j === 1) {
            continue outer; // Continue outer loop
        }
        console.log(`i: ${i}, j: ${j}`);
    }
}

// Output:
// i: 0, j: 0
// i: 0, j: 1
// i: 0, j: 2
// i: 1, j: 0
// i: 2, j: 0
// i: 2, j: 1
// i: 2, j: 2
```

### For...of Loop

The `for...of` loop iterates over iterable objects (arrays, strings, etc.).

```javascript
let fruits = ["apple", "banana", "orange"];

for (let fruit of fruits) {
    console.log(fruit);
}

// With strings
let text = "Hello";
for (let char of text) {
    console.log(char);
}

// With index access
for (let [index, fruit] of fruits.entries()) {
    console.log(`${index}: ${fruit}`);
}
```

### For...in Loop

The `for...in` loop iterates over object properties.

```javascript
let user = {
    name: "John",
    age: 30,
    city: "New York"
};

for (let key in user) {
    console.log(`${key}: ${user[key]}`);
}

// Note: Not recommended for arrays (order not guaranteed)
let numbers = [10, 20, 30];
for (let index in numbers) {
    console.log(index); // "0", "1", "2" (strings!)
}
```

---

## 💻 Practical (1.5h)

### Exercise 1: Basic For Loop

```javascript
// Exercise 1.1: Count from 1 to 10
console.log("=== Count 1 to 10 ===");
for (let i = 1; i <= 10; i++) {
    console.log(i);
}

// Exercise 1.2: Count backwards from 10 to 1
console.log("\n=== Count 10 to 1 ===");
for (let i = 10; i >= 1; i--) {
    console.log(i);
}

// Exercise 1.3: Count by 2s
console.log("\n=== Count by 2s ===");
for (let i = 0; i <= 20; i += 2) {
    console.log(i);
}

// Exercise 1.4: Sum of numbers 1 to 100
console.log("\n=== Sum 1 to 100 ===");
let sum = 0;
for (let i = 1; i <= 100; i++) {
    sum += i;
}
console.log("Sum:", sum);
```

### Exercise 2: Looping Through Arrays

```javascript
// Exercise 2.1: Loop through array with traditional for
console.log("=== Traditional For Loop ===");
let fruits = ["apple", "banana", "orange", "grape", "mango"];
for (let i = 0; i < fruits.length; i++) {
    console.log(`${i}: ${fruits[i]}`);
}

// Exercise 2.2: Loop with for...of
console.log("\n=== For...of Loop ===");
for (let fruit of fruits) {
    console.log(fruit);
}

// Exercise 2.3: Loop with forEach
console.log("\n=== forEach Loop ===");
fruits.forEach((fruit, index) => {
    console.log(`${index}: ${fruit}`);
});

// Exercise 2.4: Process array elements
console.log("\n=== Process Array ===");
let numbers = [1, 2, 3, 4, 5];
let doubled = [];
for (let num of numbers) {
    doubled.push(num * 2);
}
console.log("Original:", numbers);
console.log("Doubled:", doubled);
```

### Exercise 3: Nested Loops

```javascript
// Exercise 3.1: Basic nested loop
console.log("=== Basic Nested Loop ===");
for (let i = 0; i < 3; i++) {
    for (let j = 0; j < 3; j++) {
        console.log(`i: ${i}, j: ${j}`);
    }
}

// Exercise 3.2: Matrix traversal
console.log("\n=== Matrix Traversal ===");
let matrix = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
];

for (let i = 0; i < matrix.length; i++) {
    for (let j = 0; j < matrix[i].length; j++) {
        console.log(`matrix[${i}][${j}] = ${matrix[i][j]}`);
    }
}

// Exercise 3.3: Print patterns
console.log("\n=== Pattern: Right Triangle ===");
for (let i = 1; i <= 5; i++) {
    let line = "";
    for (let j = 1; j <= i; j++) {
        line += "* ";
    }
    console.log(line);
}

console.log("\n=== Pattern: Square ===");
for (let i = 1; i <= 5; i++) {
    let line = "";
    for (let j = 1; j <= 5; j++) {
        line += "* ";
    }
    console.log(line);
}

console.log("\n=== Pattern: Number Triangle ===");
for (let i = 1; i <= 5; i++) {
    let line = "";
    for (let j = 1; j <= i; j++) {
        line += j + " ";
    }
    console.log(line);
}
```

### Exercise 4: Loop Control

```javascript
// Exercise 4.1: break statement
console.log("=== Break Statement ===");
for (let i = 0; i < 10; i++) {
    if (i === 5) {
        console.log("Breaking at i =", i);
        break;
    }
    console.log(i);
}

// Exercise 4.2: continue statement
console.log("\n=== Continue Statement ===");
for (let i = 0; i < 10; i++) {
    if (i % 2 === 0) {
        continue;
    }
    console.log(i);
}

// Exercise 4.3: Find first even number
console.log("\n=== Find First Even ===");
let numbers = [1, 3, 5, 7, 8, 9, 11];
for (let num of numbers) {
    if (num % 2 === 0) {
        console.log("First even number:", num);
        break;
    }
}

// Exercise 4.4: Skip specific values
console.log("\n=== Skip Multiples of 3 ===");
for (let i = 1; i <= 20; i++) {
    if (i % 3 === 0) {
        continue;
    }
    console.log(i);
}
```

### Exercise 5: Advanced For Examples

```javascript
// Exercise 5.1: Generate multiplication table
console.log("=== Multiplication Table ===");
console.log("   |  1  2  3  4  5  6  7  8  9 10");
console.log("---|----------------------------------");

for (let i = 1; i <= 10; i++) {
    let row = `${i.toString().padStart(2, " ")} |`;
    for (let j = 1; j <= 10; j++) {
        row += (i * j).toString().padStart(3, " ");
    }
    console.log(row);
}

// Exercise 5.2: Find prime numbers
console.log("\n=== Prime Numbers 1-100 ===");
function isPrime(num) {
    if (num < 2) return false;
    for (let i = 2; i <= Math.sqrt(num); i++) {
        if (num % i === 0) return false;
    }
    return true;
}

for (let i = 1; i <= 100; i++) {
    if (isPrime(i)) {
        console.log(i);
    }
}

// Exercise 5.3: Fibonacci sequence
console.log("\n=== Fibonacci Sequence (first 10) ===");
let fib = [0, 1];
for (let i = 2; i < 10; i++) {
    fib[i] = fib[i - 1] + fib[i - 2];
}
console.log(fib);

// Exercise 5.4: Reverse an array
console.log("\n=== Reverse Array ===");
let original = [1, 2, 3, 4, 5];
let reversed = [];
for (let i = original.length - 1; i >= 0; i--) {
    reversed.push(original[i]);
}
console.log("Original:", original);
console.log("Reversed:", reversed);
```

### Exercise 6: Add Products to Page

```javascript
// Exercise 6: Dynamic product list generation
console.log("=== Add Products to Page ===");

class ProductRenderer {
    constructor(containerId) {
        this.container = document.getElementById(containerId);
        this.products = [];
    }

    addProduct(name, price, image, description) {
        const product = {
            id: Date.now(),
            name,
            price,
            image,
            description
        };
        this.products.push(product);
        this.renderProduct(product);
        return product;
    }

    renderProduct(product) {
        const productCard = document.createElement('div');
        productCard.className = 'product-card';
        productCard.id = `product-${product.id}`;
        
        productCard.innerHTML = `
            <img src="${product.image}" alt="${product.name}" class="product-image">
            <div class="product-info">
                <h3 class="product-name">${product.name}</h3>
                <p class="product-price">$${product.price.toFixed(2)}</p>
                <p class="product-description">${product.description}</p>
                <button class="add-to-cart" onclick="addToCart(${product.id})">Add to Cart</button>
                <button class="remove-product" onclick="removeProduct(${product.id})">Remove</button>
            </div>
        `;
        
        this.container.appendChild(productCard);
    }

    removeProduct(id) {
        const productElement = document.getElementById(`product-${id}`);
        if (productElement) {
            productElement.remove();
            this.products = this.products.filter(p => p.id !== id);
        }
    }

    renderAllProducts() {
        this.container.innerHTML = '';
        for (let product of this.products) {
            this.renderProduct(product);
        }
    }

    filterProducts(predicate) {
        this.container.innerHTML = '';
        for (let product of this.products) {
            if (predicate(product)) {
                this.renderProduct(product);
            }
        }
    }

    sortProducts(compareFn) {
        const sorted = [...this.products].sort(compareFn);
        this.container.innerHTML = '';
        for (let product of sorted) {
            this.renderProduct(product);
        }
    }
}

// Sample usage (in browser environment)
/*
const renderer = new ProductRenderer('products-container');

// Add products
renderer.addProduct(
    "Laptop",
    999.99,
    "https://via.placeholder.com/300",
    "High-performance laptop for professionals"
);

renderer.addProduct(
    "Wireless Headphones",
    149.99,
    "https://via.placeholder.com/300",
    "Noise-cancelling wireless headphones"
);

renderer.addProduct(
    "Smart Watch",
    299.99,
    "https://via.placeholder.com/300",
    "Fitness tracking smartwatch"
);
*/
```

### Exercise 7: While Loop

```javascript
// Exercise 7.1: Basic while loop
console.log("=== Basic While Loop ===");
let i = 1;
while (i <= 5) {
    console.log(i);
    i++;
}

// Exercise 7.2: Sum until limit
console.log("\n=== Sum Until Limit ===");
let sum = 0;
let num = 1;
while (sum < 100) {
    sum += num;
    num++;
}
console.log("Sum reached", sum, "at number", num - 1);

// Exercise 7.3: Input validation simulation
console.log("\n=== Input Validation ===");
let password = "";
let attempts = 0;
const maxAttempts = 3;

// Simulate password check
while (password !== "secret123" && attempts < maxAttempts) {
    console.log(`Attempt ${attempts + 1}: Invalid password`);
    attempts++;
    // In real scenario: password = prompt("Enter password:");
    password = attempts === 2 ? "secret123" : "wrong"; // Simulate success on 3rd try
}

if (password === "secret123") {
    console.log("Access granted!");
} else {
    console.log("Access denied. Too many attempts.");
}

// Exercise 7.4: Array processing with while
console.log("\n=== Array Processing with While ===");
let fruits = ["apple", "banana", "orange", "grape"];
let index = 0;

while (index < fruits.length) {
    console.log(fruits[index]);
    index++;
}
```

### Exercise 8: Do-While Loop

```javascript
// Exercise 8.1: Basic do-while loop
console.log("=== Basic Do-While Loop ===");
let i = 1;
do {
    console.log(i);
    i++;
} while (i <= 5);

// Exercise 8.2: Menu system simulation
console.log("\n=== Menu System ===");
let choice;
do {
    console.log("\nMenu:");
    console.log("1. View Products");
    console.log("2. Add Product");
    console.log("3. Exit");
    console.log("Enter choice:");
    
    // Simulate user input
    choice = Math.floor(Math.random() * 3) + 1;
    console.log("Selected:", choice);
    
    if (choice === 1) {
        console.log("Viewing products...");
    } else if (choice === 2) {
        console.log("Adding product...");
    }
} while (choice !== 3);

console.log("Exiting menu...");

// Exercise 8.3: Number guessing game
console.log("\n=== Number Guessing Game ===");
let targetNumber = Math.floor(Math.random() * 10) + 1;
let guess;
let attempts = 0;

console.log("Guess a number between 1 and 10");

do {
    attempts++;
    guess = Math.floor(Math.random() * 10) + 1; // Simulate guess
    console.log(`Attempt ${attempts}: Guessed ${guess}`);
    
    if (guess < targetNumber) {
        console.log("Too low!");
    } else if (guess > targetNumber) {
        console.log("Too high!");
    }
} while (guess !== targetNumber);

console.log(`Correct! You guessed it in ${attempts} attempts.`);

// Exercise 8.4: Do-while vs while comparison
console.log("\n=== Do-While vs While ===");

// Do-while: Always executes at least once
console.log("Do-while (condition false initially):");
let x = 10;
do {
    console.log("Executed:", x);
    x++;
} while (x < 5);

// While: May not execute at all
console.log("\nWhile (condition false initially):");
let y = 10;
while (y < 5) {
    console.log("Executed:", y);
    y++;
}
console.log("While loop didn't execute");
```

### Exercise 9: Loop Comparison and Best Practices

```javascript
// Exercise 9.1: When to use each loop type
console.log("=== Loop Comparison ===");

let numbers = [1, 2, 3, 4, 5];

// For loop: When you need index control
console.log("For loop (with index):");
for (let i = 0; i < numbers.length; i++) {
    console.log(`Index ${i}: ${numbers[i]}`);
}

// For...of: When you just need values
console.log("\nFor...of (values only):");
for (let num of numbers) {
    console.log(num);
}

// ForEach: When you want to process each element
console.log("\nforEach (processing):");
numbers.forEach((num, index) => {
    console.log(`Processing ${num} at index ${index}`);
});

// While: When you don't know iterations in advance
console.log("\nWhile (unknown iterations):");
let count = 0;
while (count < 3) {
    console.log("Count:", count);
    count++;
}

// Do-while: When you need at least one execution
console.log("\nDo-while (at least once):");
let value = 5;
do {
    console.log("Value:", value);
    value--;
} while (value > 10);

// Exercise 9.2: Performance considerations
console.log("\n=== Performance Considerations ===");

let largeArray = Array.from({length: 10000}, (_, i) => i);

// For loop (generally fastest)
console.time("for loop");
for (let i = 0; i < largeArray.length; i++) {
    largeArray[i] *= 2;
}
console.timeEnd("for loop");

// For...of (slower but more readable)
console.time("for...of");
let result = [];
for (let num of largeArray) {
    result.push(num * 2);
}
console.timeEnd("for...of");

// forEach (functional approach)
console.time("forEach");
largeArray.forEach((num, i) => {
    largeArray[i] = num * 2;
});
console.timeEnd("forEach");
```

### Exercise 10: Complete Working Example

**Complete script.js:**
```javascript
// Session 6: Loops
// This script demonstrates various loop types and their uses

console.log("=== Session 6: Loops ===");

// 1. Basic for loop
console.log("\n--- Basic For Loop ---");
for (let i = 1; i <= 5; i++) {
    console.log("Iteration:", i);
}

// 2. Loop through array
console.log("\n--- Loop Through Array ---");
let fruits = ["apple", "banana", "orange"];
for (let i = 0; i < fruits.length; i++) {
    console.log(fruits[i]);
}

// 3. For...of loop
console.log("\n--- For...of Loop ---");
for (let fruit of fruits) {
    console.log(fruit);
}

// 4. Nested loops
console.log("\n--- Nested Loops ---");
for (let i = 0; i < 3; i++) {
    for (let j = 0; j < 3; j++) {
        console.log(`i: ${i}, j: ${j}`);
    }
}

// 5. Loop control
console.log("\n--- Loop Control ---");
console.log("Break example:");
for (let i = 0; i < 10; i++) {
    if (i === 5) break;
    console.log(i);
}

console.log("\nContinue example:");
for (let i = 0; i < 10; i++) {
    if (i % 2 === 0) continue;
    console.log(i);
}

// 6. While loop
console.log("\n--- While Loop ---");
let count = 1;
while (count <= 3) {
    console.log("Count:", count);
    count++;
}

// 7. Do-while loop
console.log("\n--- Do-While Loop ---");
let num = 1;
do {
    console.log("Number:", num);
    num++;
} while (num <= 3);

// 8. Advanced example: Multiplication table
console.log("\n--- Multiplication Table ---");
for (let i = 1; i <= 5; i++) {
    let row = "";
    for (let j = 1; j <= 5; j++) {
        row += (i * j).toString().padStart(4, " ");
    }
    console.log(row);
}

console.log("\n=== Session 6 Complete ===");
```

**Complete index.html:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Session 6 - Loops</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            max-width: 1000px;
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
        .controls {
            background: #f9f9f9;
            padding: 15px;
            border-radius: 4px;
            margin-bottom: 15px;
        }
        .controls input, .controls select, .controls button {
            padding: 8px;
            margin: 5px;
            border: 1px solid #ddd;
            border-radius: 4px;
        }
        .controls button {
            background-color: #1890ff;
            color: white;
            border: none;
            cursor: pointer;
        }
        .controls button:hover {
            background-color: #0c7cd5;
        }
        .products-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
            gap: 20px;
            margin-top: 15px;
        }
        .product-card {
            background: white;
            border: 1px solid #e8e8e8;
            border-radius: 8px;
            padding: 15px;
            transition: transform 0.2s, box-shadow 0.2s;
        }
        .product-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 4px 12px rgba(0,0,0,0.15);
        }
        .product-image {
            width: 100%;
            height: 150px;
            object-fit: cover;
            border-radius: 4px;
            margin-bottom: 10px;
        }
        .product-name {
            margin: 0 0 5px 0;
            color: #333;
        }
        .product-price {
            color: #1890ff;
            font-weight: bold;
            font-size: 1.2em;
            margin: 5px 0;
        }
        .product-description {
            color: #666;
            font-size: 0.9em;
            margin: 5px 0 10px 0;
        }
        .product-actions {
            display: flex;
            gap: 10px;
        }
        .product-actions button {
            flex: 1;
            padding: 8px;
            border: none;
            border-radius: 4px;
            cursor: pointer;
        }
        .add-to-cart {
            background-color: #52c41a;
            color: white;
        }
        .add-to-cart:hover {
            background-color: #389e0d;
        }
        .remove-product {
            background-color: #ff4d4f;
            color: white;
        }
        .remove-product:hover {
            background-color: #cf1322;
        }
        .loop-demo {
            background: #f0f0f0;
            padding: 15px;
            border-radius: 4px;
            font-family: monospace;
            white-space: pre-wrap;
            margin-top: 10px;
            max-height: 300px;
            overflow-y: auto;
        }
        .pattern-output {
            font-family: monospace;
            background: #f0f0f0;
            padding: 15px;
            border-radius: 4px;
            margin-top: 10px;
        }
    </style>
</head>
<body>
    <h1>Session 6: Loops</h1>
    
    <div class="section">
        <h2>Topics Covered</h2>
        <ul>
            <li>For loop concept and syntax</li>
            <li>Looping through sequences (arrays, strings, objects)</li>
            <li>Nested loops</li>
            <li>Loop control (break, continue, labels)</li>
            <li>For...of and for...in loops</li>
            <li>While and do-while loops</li>
        </ul>
    </div>

    <div class="section">
        <h2>Basic Loop Demonstrations</h2>
        <div class="controls">
            <button onclick="demonstrateForLoop()">For Loop (1-10)</button>
            <button onclick="demonstrateForOf()">For...of Loop</button>
            <button onclick="demonstrateWhile()">While Loop</button>
            <button onclick="demonstrateDoWhile()">Do-While Loop</button>
            <button onclick="demonstrateBreakContinue()">Break/Continue</button>
        </div>
        <div id="loopOutput" class="loop-demo">
            Click a button to see loop demonstration...
        </div>
    </div>

    <div class="section">
        <h2>Pattern Printing</h2>
        <div class="controls">
            <button onclick="printTriangle()">Triangle</button>
            <button onclick="printSquare()">Square</button>
            <button onclick="printNumberTriangle()">Number Triangle</button>
            <button onclick="printMultiplicationTable()">Multiplication Table</button>
        </div>
        <div id="patternOutput" class="pattern-output">
            Click a button to see pattern...
        </div>
    </div>

    <div class="section">
        <h2>Add Products to Page</h2>
        <div class="controls">
            <input type="text" id="productName" placeholder="Product name">
            <input type="number" id="productPrice" placeholder="Price" step="0.01">
            <input type="text" id="productImage" placeholder="Image URL">
            <input type="text" id="productDesc" placeholder="Description">
            <button onclick="addProduct()">Add Product</button>
            
            <div style="margin-top: 10px;">
                <button onclick="renderAllProducts()">Show All</button>
                <button onclick="sortProductsPrice()">Sort by Price</button>
                <button onclick="filterExpensive()">Filter Expensive ($50+)</button>
                <button onclick="clearProducts()">Clear All</button>
            </div>
        </div>
        <div id="productsContainer" class="products-grid">
            <p style="grid-column: 1/-1; text-align: center; color: #999;">No products yet</p>
        </div>
    </div>

    <div class="section">
        <h2>Nested Loop Examples</h2>
        <div class="controls">
            <button onclick="demonstrateNestedLoops()">Basic Nested Loop</button>
            <button onclick="demonstrateMatrix()">Matrix Traversal</button>
            <button onclick="findPrimes()">Find Primes (1-100)</button>
            <button onclick="generateFibonacci()">Fibonacci Sequence</button>
        </div>
        <div id="nestedOutput" class="loop-demo">
            Click a button to see nested loop examples...
        </div>
    </div>

    <div class="section">
        <h2>Console Output</h2>
        <p>Open the browser console (F12) to see all JavaScript examples.</p>
    </div>

    <script src="script.js" defer></script>
    <script>
        // Loop demonstrations
        function demonstrateForLoop() {
            let output = "For Loop (1-10):\n";
            for (let i = 1; i <= 10; i++) {
                output += i + " ";
            }
            document.getElementById('loopOutput').textContent = output;
        }

        function demonstrateForOf() {
            let fruits = ["apple", "banana", "orange", "grape"];
            let output = "For...of Loop:\n";
            for (let fruit of fruits) {
                output += fruit + "\n";
            }
            document.getElementById('loopOutput').textContent = output;
        }

        function demonstrateWhile() {
            let output = "While Loop:\n";
            let i = 1;
            while (i <= 5) {
                output += i + " ";
                i++;
            }
            document.getElementById('loopOutput').textContent = output;
        }

        function demonstrateDoWhile() {
            let output = "Do-While Loop:\n";
            let i = 1;
            do {
                output += i + " ";
                i++;
            } while (i <= 5);
            document.getElementById('loopOutput').textContent = output;
        }

        function demonstrateBreakContinue() {
            let output = "Break/Continue Example:\n";
            output += "Break (stop at 5):\n";
            for (let i = 0; i < 10; i++) {
                if (i === 5) break;
                output += i + " ";
            }
            output += "\n\nContinue (skip evens):\n";
            for (let i = 0; i < 10; i++) {
                if (i % 2 === 0) continue;
                output += i + " ";
            }
            document.getElementById('loopOutput').textContent = output;
        }

        // Pattern printing
        function printTriangle() {
            let output = "";
            for (let i = 1; i <= 5; i++) {
                for (let j = 1; j <= i; j++) {
                    output += "* ";
                }
                output += "\n";
            }
            document.getElementById('patternOutput').textContent = output;
        }

        function printSquare() {
            let output = "";
            for (let i = 1; i <= 5; i++) {
                for (let j = 1; j <= 5; j++) {
                    output += "* ";
                }
                output += "\n";
            }
            document.getElementById('patternOutput').textContent = output;
        }

        function printNumberTriangle() {
            let output = "";
            for (let i = 1; i <= 5; i++) {
                for (let j = 1; j <= i; j++) {
                    output += j + " ";
                }
                output += "\n";
            }
            document.getElementById('patternOutput').textContent = output;
        }

        function printMultiplicationTable() {
            let output = "Multiplication Table (1-5):\n";
            output += "   |  1  2  3  4  5\n";
            output += "---|---------------\n";
            for (let i = 1; i <= 5; i++) {
                output += i.toString().padStart(2, " ") + " |";
                for (let j = 1; j <= 5; j++) {
                    output += (i * j).toString().padStart(3, " ");
                }
                output += "\n";
            }
            document.getElementById('patternOutput').textContent = output;
        }

        // Product management
        let products = [];

        function addProduct() {
            const name = document.getElementById('productName').value;
            const price = parseFloat(document.getElementById('productPrice').value);
            const image = document.getElementById('productImage').value || 'https://via.placeholder.com/300';
            const desc = document.getElementById('productDesc').value;

            if (!name || isNaN(price)) {
                alert('Please enter name and price');
                return;
            }

            const product = {
                id: Date.now(),
                name,
                price,
                image,
                description: desc
            };

            products.push(product);
            renderProducts();
            clearInputs();
        }

        function renderProducts(productList = products) {
            const container = document.getElementById('productsContainer');
            
            if (productList.length === 0) {
                container.innerHTML = '<p style="grid-column: 1/-1; text-align: center; color: #999;">No products</p>';
                return;
            }

            container.innerHTML = productList.map(product => `
                <div class="product-card" id="product-${product.id}">
                    <img src="${product.image}" alt="${product.name}" class="product-image">
                    <h3 class="product-name">${product.name}</h3>
                    <p class="product-price">$${product.price.toFixed(2)}</p>
                    <p class="product-description">${product.description || 'No description'}</p>
                    <div class="product-actions">
                        <button class="add-to-cart" onclick="addToCart(${product.id})">Add to Cart</button>
                        <button class="remove-product" onclick="removeProduct(${product.id})">Remove</button>
                    </div>
                </div>
            `).join('');
        }

        function removeProduct(id) {
            products = products.filter(p => p.id !== id);
            renderProducts();
        }

        function renderAllProducts() {
            renderProducts();
        }

        function sortProductsPrice() {
            const sorted = [...products].sort((a, b) => a.price - b.price);
            renderProducts(sorted);
        }

        function filterExpensive() {
            const expensive = products.filter(p => p.price >= 50);
            renderProducts(expensive);
        }

        function clearProducts() {
            products = [];
            renderProducts();
        }

        function addToCart(id) {
            const product = products.find(p => p.id === id);
            alert(`Added ${product.name} to cart!`);
        }

        function clearInputs() {
            document.getElementById('productName').value = '';
            document.getElementById('productPrice').value = '';
            document.getElementById('productImage').value = '';
            document.getElementById('productDesc').value = '';
        }

        // Nested loop examples
        function demonstrateNestedLoops() {
            let output = "Basic Nested Loop:\n";
            for (let i = 0; i < 3; i++) {
                for (let j = 0; j < 3; j++) {
                    output += `i: ${i}, j: ${j}\n`;
                }
            }
            document.getElementById('nestedOutput').textContent = output;
        }

        function demonstrateMatrix() {
            let matrix = [
                [1, 2, 3],
                [4, 5, 6],
                [7, 8, 9]
            ];
            let output = "Matrix Traversal:\n";
            for (let i = 0; i < matrix.length; i++) {
                for (let j = 0; j < matrix[i].length; j++) {
                    output += `matrix[${i}][${j}] = ${matrix[i][j]}\n`;
                }
            }
            document.getElementById('nestedOutput').textContent = output;
        }

        function findPrimes() {
            function isPrime(num) {
                if (num < 2) return false;
                for (let i = 2; i <= Math.sqrt(num); i++) {
                    if (num % i === 0) return false;
                }
                return true;
            }

            let output = "Prime Numbers (1-100):\n";
            for (let i = 1; i <= 100; i++) {
                if (isPrime(i)) {
                    output += i + " ";
                }
            }
            document.getElementById('nestedOutput').textContent = output;
        }

        function generateFibonacci() {
            let fib = [0, 1];
            for (let i = 2; i < 15; i++) {
                fib[i] = fib[i - 1] + fib[i - 2];
            }
            document.getElementById('nestedOutput').textContent = "Fibonacci Sequence (first 15):\n" + fib.join(", ");
        }
    </script>
</body>
</html>
```

---

## 📝 Review (0.5h)

### Loop Challenge

```javascript
// Challenge 1: Print numbers 1-20, but skip multiples of 3
console.log("=== Challenge 1: Skip Multiples of 3 ===");
// Your code here

// Challenge 2: Find the sum of all even numbers from 1-100
console.log("\n=== Challenge 2: Sum Even Numbers ===");
// Your code here

// Challenge 3: Print the following pattern:
// *
// **
// ***
// ****
// *****
console.log("\n=== Challenge 3: Pattern ===");
// Your code here

// Challenge 4: Reverse an array using a loop
let arr = [1, 2, 3, 4, 5];
console.log("\n=== Challenge 4: Reverse Array ===");
// Your code here

// Challenge 5: Find the largest number in an array
let numbers = [10, 5, 20, 8, 15];
console.log("\n=== Challenge 5: Find Largest ===");
// Your code here

// Challenge 6: Count occurrences of each element in an array
let items = ["apple", "banana", "apple", "orange", "banana", "apple"];
console.log("\n=== Challenge 6: Count Occurrences ===");
// Your code here

// Challenge 7: Generate a random number between 1-10 and keep guessing until correct
console.log("\n=== Challenge 7: Number Guessing ===");
// Your code here

// Challenge 8: Create a multiplication table for numbers 1-10
console.log("\n=== Challenge 8: Multiplication Table ===");
// Your code here

// Challenge 9: Remove duplicates from an array using loops
let duplicates = [1, 2, 3, 2, 4, 5, 3, 6];
console.log("\n=== Challenge 9: Remove Duplicates ===");
// Your code here

// Challenge 10: Find the factorial of a number using a loop
let num = 5;
console.log("\n=== Challenge 10: Factorial ===");
// Your code here
```

### Challenge Solutions

```javascript
// Challenge 1: Print numbers 1-20, skip multiples of 3
console.log("=== Challenge 1: Skip Multiples of 3 ===");
for (let i = 1; i <= 20; i++) {
    if (i % 3 === 0) continue;
    console.log(i);
}

// Challenge 2: Sum of even numbers 1-100
console.log("\n=== Challenge 2: Sum Even Numbers ===");
let sum = 0;
for (let i = 1; i <= 100; i++) {
    if (i % 2 === 0) {
        sum += i;
    }
}
console.log("Sum of even numbers:", sum);

// Challenge 3: Print pattern
console.log("\n=== Challenge 3: Pattern ===");
for (let i = 1; i <= 5; i++) {
    let line = "";
    for (let j = 1; j <= i; j++) {
        line += "*";
    }
    console.log(line);
}

// Challenge 4: Reverse array
console.log("\n=== Challenge 4: Reverse Array ===");
let arr = [1, 2, 3, 4, 5];
let reversed = [];
for (let i = arr.length - 1; i >= 0; i--) {
    reversed.push(arr[i]);
}
console.log("Original:", arr);
console.log("Reversed:", reversed);

// Challenge 5: Find largest number
console.log("\n=== Challenge 5: Find Largest ===");
let numbers = [10, 5, 20, 8, 15];
let largest = numbers[0];
for (let num of numbers) {
    if (num > largest) {
        largest = num;
    }
}
console.log("Largest number:", largest);

// Challenge 6: Count occurrences
console.log("\n=== Challenge 6: Count Occurrences ===");
let items = ["apple", "banana", "apple", "orange", "banana", "apple"];
let counts = {};
for (let item of items) {
    counts[item] = (counts[item] || 0) + 1;
}
console.log("Occurrences:", counts);

// Challenge 7: Number guessing
console.log("\n=== Challenge 7: Number Guessing ===");
let target = Math.floor(Math.random() * 10) + 1;
let guess;
let attempts = 0;

while (true) {
    attempts++;
    guess = Math.floor(Math.random() * 10) + 1; // Simulate guess
    console.log(`Attempt ${attempts}: Guessed ${guess}`);
    
    if (guess === target) {
        console.log(`Correct! Found ${target} in ${attempts} attempts`);
        break;
    }
}

// Challenge 8: Multiplication table
console.log("\n=== Challenge 8: Multiplication Table ===");
for (let i = 1; i <= 10; i++) {
    let row = "";
    for (let j = 1; j <= 10; j++) {
        row += (i * j).toString().padStart(4, " ");
    }
    console.log(row);
}

// Challenge 9: Remove duplicates
console.log("\n=== Challenge 9: Remove Duplicates ===");
let duplicates = [1, 2, 3, 2, 4, 5, 3, 6];
let unique = [];
for (let num of duplicates) {
    if (!unique.includes(num)) {
        unique.push(num);
    }
}
console.log("Original:", duplicates);
console.log("Unique:", unique);

// Challenge 10: Factorial
console.log("\n=== Challenge 10: Factorial ===");
let num = 5;
let factorial = 1;
for (let i = 2; i <= num; i++) {
    factorial *= i;
}
console.log(`Factorial of ${num}:`, factorial);
```

### Review Questions

1. **What is the correct syntax for a for loop?**
   - [ ] for (i = 0; i < 5; i++)
   - [ ] for (let i = 0; i < 5; i++)
   - [ ] for (i < 5; i++)
   - [ ] for (let i = 0; i < 5)

2. **What does the `break` statement do?**
   - [ ] Skips the current iteration
   - [ ] Exits the loop immediately
   - [ ] Restarts the loop
   - [ ] Pauses the loop

3. **What does the `continue` statement do?**
   - [ ] Exits the loop
   - [ ] Skips the current iteration
   - [ ] Restarts the loop
   - [ ] Pauses execution

4. **Which loop is best for iterating over array values?**
   - [ ] for loop
   - [ ] while loop
   - [ ] for...of loop
   - [ ] do-while loop

5. **What is the difference between while and do-while?**
   - [ ] No difference
   - [ ] do-while always executes at least once
   - [ ] while always executes at least once
   - [ ] do-while is faster

6. **How do you exit a nested loop from the inner loop?**
   - [ ] break
   - [ ] continue
   - [ ] labeled break
   - [ ] return

7. **What happens if the loop condition is initially false in a while loop?**
   - [ ] Error
   - [ ] Loop executes once
   - [ ] Loop doesn't execute
   - [ ] Infinite loop

8. **Which loop would you use when you don't know the number of iterations?**
   - [ ] for loop
   - [ ] while loop
   - [ ] for...of loop
   - [ ] All of the above

9. **What does `for...in` iterate over?**
   - [ ] Array values
   - [ ] Object properties
   - [ ] String characters
   - [ ] Map entries

10. **How many times will this loop execute? `for (let i = 0; i < 5; i++)`**
    - [ ] 4 times
    - [ ] 5 times
    - [ ] 6 times
    - [ ] Infinite

### Correct Answers

1. ✅ for (let i = 0; i < 5; i++)
2. ✅ Exits the loop immediately
3. ✅ Skips the current iteration
4. ✅ for...of loop
5. ✅ do-while always executes at least once
6. ✅ labeled break
7. ✅ Loop doesn't execute
8. ✅ while loop
9. ✅ Object properties
10. ✅ 5 times

---

## 🎯 Next Steps

1. ✅ Practice all loop types
2. ✅ Master nested loops
3. ✅ Understand when to use each loop type
4. ✅ Practice loop control statements
5. ✅ Complete all challenge exercises
6. ✅ Build applications using loops
7. ✅ Learn about performance considerations

---

## 📚 Additional Resources

- [MDN: for](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for)
- [MDN: while](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/while)
- [MDN: for...of](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...of)
- [MDN: for...in](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...in)
- [JavaScript.info: Loops](https://javascript.info/while-for)
- [JavaScript.info: for...of](https://javascript.info/for..of)

**Remember:** Loops are fundamental to programming. Master them to automate repetitive tasks and process data efficiently! 💪
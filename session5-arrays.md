# Session 5: Arrays

## 📚 Theory (1h)

### Array Introduction

An array is a data structure that stores multiple values in a single variable. Arrays are ordered collections that can hold any type of data.

#### What is an Array?

```javascript
// Array is like a container that holds multiple values
let fruits = ["apple", "banana", "orange"];
let numbers = [1, 2, 3, 4, 5];
let mixed = [1, "hello", true, null, {name: "John"}];
```

#### Creating Arrays

**1. Array Literal (Recommended)**
```javascript
let fruits = ["apple", "banana", "orange"];
let emptyArray = [];
```

**2. Array Constructor**
```javascript
let fruits = new Array("apple", "banana", "orange");
let emptyArray = new Array();
let arrayWithSize = new Array(5); // Creates array with 5 empty slots
```

**3. Array.from()**
```javascript
let fromString = Array.from("hello"); // ["h", "e", "l", "l", "o"]
let fromRange = Array.from({length: 5}, (_, i) => i + 1); // [1, 2, 3, 4, 5]
```

#### Array Characteristics

```javascript
let fruits = ["apple", "banana", "orange"];

// Ordered - elements have positions
console.log(fruits[0]); // "apple" (first element)
console.log(fruits[1]); // "banana" (second element)

// Zero-indexed - positions start at 0
console.log(fruits.length); // 3 (number of elements)

// Can hold any type
let mixed = [1, "string", true, null, undefined, {key: "value"}, [1, 2, 3]];

// Dynamic - can change size
fruits.push("grape"); // Add element
fruits.pop();         // Remove element
```

### Array Length

The `length` property returns the number of elements in an array.

```javascript
let fruits = ["apple", "banana", "orange"];
console.log(fruits.length); // 3

// Length is not read-only
fruits.length = 5; // Adds two empty slots
console.log(fruits); // ["apple", "banana", "orange", empty × 2]

fruits.length = 2; // Removes elements
console.log(fruits); // ["apple", "banana"]

// Clear array
fruits.length = 0;
console.log(fruits); // []
```

### Accessing Array Elements

```javascript
let fruits = ["apple", "banana", "orange", "grape"];

// Access by index
console.log(fruits[0]);    // "apple" (first)
console.log(fruits[1]);    // "banana"
console.log(fruits[3]);    // "grape"

// Access last element
console.log(fruits[fruits.length - 1]); // "grape"

// Negative indices (not supported in basic arrays)
// Use slice for negative-like access
console.log(fruits.slice(-1)[0]); // "grape"

// Accessing non-existent index
console.log(fruits[10]);   // undefined
```

### Adding Elements

#### Adding to End

```javascript
let fruits = ["apple", "banana"];

// push() - adds to end, returns new length
fruits.push("orange");
console.log(fruits); // ["apple", "banana", "orange"]

// push multiple elements
fruits.push("grape", "mango");
console.log(fruits); // ["apple", "banana", "orange", "grape", "mango"]

// Using length property
fruits[fruits.length] = "kiwi";
console.log(fruits); // [..., "kiwi"]
```

#### Adding to Beginning

```javascript
let fruits = ["banana", "orange"];

// unshift() - adds to beginning, returns new length
fruits.unshift("apple");
console.log(fruits); // ["apple", "banana", "orange"]

// unshift multiple elements
fruits.unshift("pear", "grape");
console.log(fruits); // ["pear", "grape", "apple", "banana", "orange"]
```

#### Adding at Specific Position

```javascript
let fruits = ["apple", "banana", "grape"];

// splice() - can add/remove at any position
// splice(start, deleteCount, item1, item2, ...)
fruits.splice(2, 0, "orange"); // Add "orange" at index 2
console.log(fruits); // ["apple", "banana", "orange", "grape"]

// Add multiple elements
fruits.splice(1, 0, "pear", "kiwi");
console.log(fruits); // ["apple", "pear", "kiwi", "banana", "orange", "grape"]
```

### Removing Elements

#### Removing from End

```javascript
let fruits = ["apple", "banana", "orange", "grape"];

// pop() - removes last element, returns removed element
let lastFruit = fruits.pop();
console.log(lastFruit); // "grape"
console.log(fruits);    // ["apple", "banana", "orange"]
```

#### Removing from Beginning

```javascript
let fruits = ["apple", "banana", "orange", "grape"];

// shift() - removes first element, returns removed element
let firstFruit = fruits.shift();
console.log(firstFruit); // "apple"
console.log(fruits);     // ["banana", "orange", "grape"]
```

#### Removing at Specific Position

```javascript
let fruits = ["apple", "banana", "orange", "grape", "mango"];

// splice() - removes elements
fruits.splice(2, 1); // Remove 1 element at index 2
console.log(fruits); // ["apple", "banana", "grape", "mango"]

// Remove multiple elements
fruits.splice(1, 2); // Remove 2 elements starting at index 1
console.log(fruits); // ["apple", "mango"]

// Remove and add
fruits.splice(1, 1, "banana", "orange"); // Replace "mango" with "banana", "orange"
console.log(fruits); // ["apple", "banana", "orange"]
```

#### Removing by Value

```javascript
let fruits = ["apple", "banana", "orange", "banana", "grape"];

// Find index and remove
let index = fruits.indexOf("banana");
if (index > -1) {
    fruits.splice(index, 1);
}
console.log(fruits); // ["apple", "orange", "banana", "grape"]

// Remove all occurrences
let fruits = ["apple", "banana", "orange", "banana", "grape"];
fruits = fruits.filter(item => item !== "banana");
console.log(fruits); // ["apple", "orange", "grape"]
```

### Searching in Arrays

#### indexOf() and lastIndexOf()

```javascript
let fruits = ["apple", "banana", "orange", "grape", "banana"];

// indexOf() - returns first index or -1
console.log(fruits.indexOf("banana"));  // 1
console.log(fruits.indexOf("pear"));    // -1 (not found)

// lastIndexOf() - returns last index or -1
console.log(fruits.lastIndexOf("banana")); // 4
console.log(fruits.lastIndexOf("pear"));   // -1

// Starting position
console.log(fruits.indexOf("banana", 2)); // 4 (search from index 2)
```

#### includes()

```javascript
let fruits = ["apple", "banana", "orange"];

// includes() - returns true/false
console.log(fruits.includes("banana")); // true
console.log(fruits.includes("pear"));   // false

// Case-sensitive
console.log(fruits.includes("Banana")); // false
```

#### find() and findIndex()

```javascript
let users = [
    { id: 1, name: "John", age: 30 },
    { id: 2, name: "Jane", age: 25 },
    { id: 3, name: "Bob", age: 35 }
];

// find() - returns first element that matches condition
let user = users.find(u => u.age > 30);
console.log(user); // { id: 3, name: "Bob", age: 35 }

// findIndex() - returns index of first matching element
let index = users.findIndex(u => u.name === "Jane");
console.log(index); // 1

// Not found
let notFound = users.find(u => u.age > 50);
console.log(notFound); // undefined
```

### Sorting Arrays

#### sort() - String Sorting

```javascript
let fruits = ["banana", "apple", "orange", "grape"];

// Default sort (alphabetical)
fruits.sort();
console.log(fruits); // ["apple", "banana", "grape", "orange"]

// Reverse sort
fruits.reverse();
console.log(fruits); // ["orange", "grape", "banana", "apple"]
```

#### sort() - Numeric Sorting

```javascript
let numbers = [10, 5, 100, 1, 50];

// Default sort (converts to strings)
numbers.sort();
console.log(numbers); // [1, 10, 100, 5, 50] (incorrect for numbers)

// Numeric sort with compare function
numbers.sort((a, b) => a - b);
console.log(numbers); // [1, 5, 10, 50, 100]

// Descending order
numbers.sort((a, b) => b - a);
console.log(numbers); // [100, 50, 10, 5, 1]
```

#### sort() - Object Sorting

```javascript
let users = [
    { name: "John", age: 30 },
    { name: "Jane", age: 25 },
    { name: "Bob", age: 35 }
];

// Sort by age
users.sort((a, b) => a.age - b.age);
console.log(users); // Jane (25), John (30), Bob (35)

// Sort by name
users.sort((a, b) => a.name.localeCompare(b.name));
console.log(users); // Bob, Jane, John
```

### Slicing Arrays

#### slice() - Extract Portion

```javascript
let fruits = ["apple", "banana", "orange", "grape", "mango"];

// slice(start, end) - end is not included
console.log(fruits.slice(1, 3));    // ["banana", "orange"]
console.log(fruits.slice(2));       // ["orange", "grape", "mango"]
console.log(fruits.slice(0, 2));    // ["apple", "banana"]

// Negative indices
console.log(fruits.slice(-2));      // ["grape", "mango"]
console.log(fruits.slice(1, -1));   // ["banana", "orange", "grape"]

// Clone array
let clone = fruits.slice();
console.log(clone); // ["apple", "banana", "orange", "grape", "mango"]
```

### Array Iteration Methods

#### forEach()

```javascript
let fruits = ["apple", "banana", "orange"];

// forEach() - executes function for each element
fruits.forEach((fruit, index) => {
    console.log(`${index}: ${fruit}`);
});

// Output:
// 0: apple
// 1: banana
// 2: orange
```

#### map()

```javascript
let numbers = [1, 2, 3, 4, 5];

// map() - creates new array with transformed elements
let doubled = numbers.map(num => num * 2);
console.log(doubled); // [2, 4, 6, 8, 10]

// map with objects
let users = [
    { name: "John", age: 30 },
    { name: "Jane", age: 25 }
];
let names = users.map(user => user.name);
console.log(names); // ["John", "Jane"]
```

#### filter()

```javascript
let numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

// filter() - creates new array with elements that pass condition
let evenNumbers = numbers.filter(num => num % 2 === 0);
console.log(evenNumbers); // [2, 4, 6, 8, 10]

let users = [
    { name: "John", age: 30 },
    { name: "Jane", age: 25 },
    { name: "Bob", age: 35 }
];
let adults = users.filter(user => user.age >= 30);
console.log(adults); // [{ name: "John", age: 30 }, { name: "Bob", age: 35 }]
```

#### reduce()

```javascript
let numbers = [1, 2, 3, 4, 5];

// reduce() - reduces array to single value
let sum = numbers.reduce((acc, num) => acc + num, 0);
console.log(sum); // 15

// With objects
let cart = [
    { item: "Book", price: 10 },
    { item: "Pen", price: 2 },
    { item: "Notebook", price: 5 }
];
let total = cart.reduce((acc, item) => acc + item.price, 0);
console.log(total); // 17
```

### Joining Arrays

#### join()

```javascript
let fruits = ["apple", "banana", "orange"];

// join() - converts array to string
console.log(fruits.join());          // "apple,banana,orange"
console.log(fruits.join(", "));     // "apple, banana, orange"
console.log(fruits.join(" - "));    // "apple - banana - orange"
console.log(fruits.join(""));       // "applebananaorange"
```

#### concat()

```javascript
let fruits1 = ["apple", "banana"];
let fruits2 = ["orange", "grape"];

// concat() - merges arrays
let allFruits = fruits1.concat(fruits2);
console.log(allFruits); // ["apple", "banana", "orange", "grape"]

// Multiple arrays
let moreFruits = allFruits.concat(["mango"], ["kiwi"]);
console.log(moreFruits); // ["apple", "banana", "orange", "grape", "mango", "kiwi"]

// Spread operator (modern approach)
let combined = [...fruits1, ...fruits2];
console.log(combined); // ["apple", "banana", "orange", "grape"]
```

### Other Useful Array Methods

#### every() and some()

```javascript
let numbers = [2, 4, 6, 8, 10];

// every() - true if all elements pass condition
let allEven = numbers.every(num => num % 2 === 0);
console.log(allEven); // true

// some() - true if at least one element passes condition
let hasGreaterThan5 = numbers.some(num => num > 5);
console.log(hasGreaterThan5); // true
```

#### flat() and flatMap()

```javascript
let nested = [1, [2, [3, [4, 5]]]];

// flat() - flattens nested arrays
console.log(nested.flat());      // [1, 2, [3, [4, 5]]]
console.log(nested.flat(2));     // [1, 2, 3, [4, 5]]
console.log(nested.flat(Infinity)); // [1, 2, 3, 4, 5]

// flatMap() - maps then flattens
let numbers = [1, 2, 3];
let doubled = numbers.flatMap(num => [num, num * 2]);
console.log(doubled); // [1, 2, 2, 4, 3, 6]
```

---

## 💻 Practical (1.5h)

### Exercise 1: Array Basics

```javascript
// Exercise 1.1: Creating arrays
console.log("=== Creating Arrays ===");

let fruits = ["apple", "banana", "orange"];
let numbers = [1, 2, 3, 4, 5];
let mixed = [1, "hello", true, null, {name: "John"}];
let empty = [];

console.log("Fruits:", fruits);
console.log("Numbers:", numbers);
console.log("Mixed:", mixed);
console.log("Empty:", empty);

// Exercise 1.2: Array length
console.log("\n=== Array Length ===");

console.log("Fruits length:", fruits.length);
console.log("Numbers length:", numbers.length);

// Modify length
fruits.length = 5;
console.log("Fruits after length=5:", fruits);

fruits.length = 2;
console.log("Fruits after length=2:", fruits);

// Exercise 1.3: Accessing elements
console.log("\n=== Accessing Elements ===");

let colors = ["red", "green", "blue", "yellow"];

console.log("First element:", colors[0]);
console.log("Second element:", colors[1]);
console.log("Last element:", colors[colors.length - 1]);
console.log("Non-existent:", colors[10]);
```

### Exercise 2: Adding Elements

```javascript
// Exercise 2.1: Adding to end
console.log("=== Adding to End ===");

let fruits = ["apple", "banana"];
console.log("Original:", fruits);

fruits.push("orange");
console.log("After push:", fruits);

fruits.push("grape", "mango");
console.log("After push multiple:", fruits);

// Exercise 2.2: Adding to beginning
console.log("\n=== Adding to Beginning ===");

fruits.unshift("pear");
console.log("After unshift:", fruits);

fruits.unshift("kiwi", "berry");
console.log("After unshift multiple:", fruits);

// Exercise 2.3: Adding at specific position
console.log("\n=== Adding at Specific Position ===");

fruits.splice(3, 0, "peach");
console.log("After splice insert:", fruits);

fruits.splice(1, 0, "lemon", "lime");
console.log("After splice multiple:", fruits);
```

### Exercise 3: Removing Elements

```javascript
// Exercise 3.1: Removing from end
console.log("=== Removing from End ===");

let fruits = ["apple", "banana", "orange", "grape", "mango"];
console.log("Original:", fruits);

let removed = fruits.pop();
console.log("Removed:", removed);
console.log("After pop:", fruits);

// Exercise 3.2: Removing from beginning
console.log("\n=== Removing from Beginning ===");

removed = fruits.shift();
console.log("Removed:", removed);
console.log("After shift:", fruits);

// Exercise 3.3: Removing at specific position
console.log("\n=== Removing at Specific Position ===");

fruits.splice(1, 1);
console.log("After splice remove:", fruits);

// Exercise 3.4: Removing by value
console.log("\n=== Removing by Value ===");

let items = ["apple", "banana", "orange", "banana", "grape"];
console.log("Original:", items);

let index = items.indexOf("banana");
if (index > -1) {
    items.splice(index, 1);
}
console.log("After removing first 'banana':", items);

// Remove all occurrences
items = items.filter(item => item !== "banana");
console.log("After removing all 'banana':", items);
```

### Exercise 4: Searching Arrays

```javascript
// Exercise 4.1: indexOf and lastIndexOf
console.log("=== indexOf and lastIndexOf ===");

let fruits = ["apple", "banana", "orange", "grape", "banana"];

console.log("indexOf('banana'):", fruits.indexOf("banana"));
console.log("lastIndexOf('banana'):", fruits.lastIndexOf("banana"));
console.log("indexOf('pear'):", fruits.indexOf("pear"));

// Exercise 4.2: includes
console.log("\n=== includes ===");

console.log("includes('orange'):", fruits.includes("orange"));
console.log("includes('pear'):", fruits.includes("pear"));

// Exercise 4.3: find and findIndex
console.log("\n=== find and findIndex ===");

let users = [
    { id: 1, name: "John", age: 30 },
    { id: 2, name: "Jane", age: 25 },
    { id: 3, name: "Bob", age: 35 }
];

let user = users.find(u => u.age > 30);
console.log("User with age > 30:", user);

let index = users.findIndex(u => u.name === "Jane");
console.log("Index of Jane:", index);
```

### Exercise 5: Sorting Arrays

```javascript
// Exercise 5.1: String sorting
console.log("=== String Sorting ===");

let fruits = ["banana", "apple", "orange", "grape"];
console.log("Original:", fruits);

fruits.sort();
console.log("After sort:", fruits);

fruits.reverse();
console.log("After reverse:", fruits);

// Exercise 5.2: Numeric sorting
console.log("\n=== Numeric Sorting ===");

let numbers = [10, 5, 100, 1, 50];
console.log("Original:", numbers);

numbers.sort();
console.log("After default sort:", numbers);

numbers.sort((a, b) => a - b);
console.log("After numeric sort (asc):", numbers);

numbers.sort((a, b) => b - a);
console.log("After numeric sort (desc):", numbers);

// Exercise 5.3: Object sorting
console.log("\n=== Object Sorting ===");

let users = [
    { name: "John", age: 30 },
    { name: "Jane", age: 25 },
    { name: "Bob", age: 35 }
];

users.sort((a, b) => a.age - b.age);
console.log("Sorted by age:", users);

users.sort((a, b) => a.name.localeCompare(b.name));
console.log("Sorted by name:", users);
```

### Exercise 6: Slicing and Joining

```javascript
// Exercise 6.1: Slicing
console.log("=== Slicing ===");

let fruits = ["apple", "banana", "orange", "grape", "mango"];

console.log("slice(1, 3):", fruits.slice(1, 3));
console.log("slice(2):", fruits.slice(2));
console.log("slice(-2):", fruits.slice(-2));
console.log("slice():", fruits.slice()); // Clone

// Exercise 6.2: Joining
console.log("\n=== Joining ===");

console.log("join():", fruits.join());
console.log("join(', '):", fruits.join(", "));
console.log("join(' - '):", fruits.join(" - "));

// Exercise 6.3: Concatenating
console.log("\n=== Concatenating ===");

let fruits1 = ["apple", "banana"];
let fruits2 = ["orange", "grape"];

let combined = fruits1.concat(fruits2);
console.log("concat:", combined);

let spreadCombined = [...fruits1, ...fruits2];
console.log("spread:", spreadCombined);
```

### Exercise 7: Array Iteration Methods

```javascript
// Exercise 7.1: forEach
console.log("=== forEach ===");

let fruits = ["apple", "banana", "orange"];
fruits.forEach((fruit, index) => {
    console.log(`${index}: ${fruit}`);
});

// Exercise 7.2: map
console.log("\n=== map ===");

let numbers = [1, 2, 3, 4, 5];
let doubled = numbers.map(num => num * 2);
console.log("Original:", numbers);
console.log("Doubled:", doubled);

let users = [
    { name: "John", age: 30 },
    { name: "Jane", age: 25 }
];
let names = users.map(user => user.name);
console.log("Names:", names);

// Exercise 7.3: filter
console.log("\n=== filter ===");

let allNumbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
let evenNumbers = allNumbers.filter(num => num % 2 === 0);
console.log("Even numbers:", evenNumbers);

let adults = users.filter(user => user.age >= 30);
console.log("Adults:", adults);

// Exercise 7.4: reduce
console.log("\n=== reduce ===");

let nums = [1, 2, 3, 4, 5];
let sum = nums.reduce((acc, num) => acc + num, 0);
console.log("Sum:", sum);

let cart = [
    { item: "Book", price: 10 },
    { item: "Pen", price: 2 },
    { item: "Notebook", price: 5 }
];
let total = cart.reduce((acc, item) => acc + item.price, 0);
console.log("Cart total:", total);
```

### Exercise 8: Array Challenge - Product List Manager

```javascript
// Exercise 8: Complete Product List Manager
console.log("=== Product List Manager ===");

class ProductManager {
    constructor() {
        this.products = [];
    }

    // Add product
    addProduct(name, price, category) {
        const product = {
            id: Date.now(),
            name,
            price,
            category
        };
        this.products.push(product);
        console.log(`Added: ${name} ($${price})`);
        return product;
    }

    // Remove product by id
    removeProduct(id) {
        const index = this.products.findIndex(p => p.id === id);
        if (index > -1) {
            const removed = this.products.splice(index, 1)[0];
            console.log(`Removed: ${removed.name}`);
            return removed;
        }
        console.log("Product not found");
        return null;
    }

    // Find product by name
    findProduct(name) {
        const product = this.products.find(p => 
            p.name.toLowerCase() === name.toLowerCase()
        );
        return product || null;
    }

    // Get all products
    getAllProducts() {
        return this.products;
    }

    // Get products by category
    getProductsByCategory(category) {
        return this.products.filter(p => 
            p.category.toLowerCase() === category.toLowerCase()
        );
    }

    // Sort products by price
    sortByPrice(ascending = true) {
        const sorted = [...this.products].sort((a, b) => 
            ascending ? a.price - b.price : b.price - a.price
        );
        return sorted;
    }

    // Get total value
    getTotalValue() {
        return this.products.reduce((total, product) => 
            total + product.price, 0
        );
    }

    // Get product count
    getProductCount() {
        return this.products.length;
    }

    // Display all products
    displayProducts() {
        console.log("\n--- Products ---");
        this.products.forEach((product, index) => {
            console.log(`${index + 1}. ${product.name} - $${product.price} (${product.category})`);
        });
        console.log(`Total: ${this.getProductCount()} products, $${this.getTotalValue()}`);
    }
}

// Test the ProductManager
const manager = new ProductManager();

// Add products
manager.addProduct("Laptop", 999.99, "Electronics");
manager.addProduct("Book", 19.99, "Books");
manager.addProduct("Headphones", 149.99, "Electronics");
manager.addProduct("Notebook", 5.99, "Stationery");
manager.addProduct("Mouse", 29.99, "Electronics");

manager.displayProducts();

// Find product
console.log("\n--- Find Product ---");
const laptop = manager.findProduct("Laptop");
console.log("Found:", laptop);

// Get by category
console.log("\n--- Electronics ---");
const electronics = manager.getProductsByCategory("Electronics");
console.log("Electronics products:", electronics);

// Sort by price
console.log("\n--- Sorted by Price (Ascending) ---");
const sortedAsc = manager.sortByPrice(true);
console.log(sortedAsc);

console.log("\n--- Sorted by Price (Descending) ---");
const sortedDesc = manager.sortByPrice(false);
console.log(sortedDesc);

// Remove product
console.log("\n--- Remove Product ---");
const removed = manager.removeProduct(laptop.id);
console.log("Removed:", removed);

manager.displayProducts();
```

### Exercise 9: Task List Manager

```javascript
// Exercise 9: Task List Manager
console.log("\n=== Task List Manager ===");

class TaskManager {
    constructor() {
        this.tasks = [];
    }

    // Add task
    addTask(title, priority = "medium") {
        const task = {
            id: Date.now(),
            title,
            priority,
            completed: false,
            createdAt: new Date()
        };
        this.tasks.push(task);
        console.log(`Added task: ${title} (${priority})`);
        return task;
    }

    // Complete task
    completeTask(id) {
        const task = this.tasks.find(t => t.id === id);
        if (task) {
            task.completed = true;
            console.log(`Completed: ${task.title}`);
            return task;
        }
        console.log("Task not found");
        return null;
    }

    // Delete task
    deleteTask(id) {
        const index = this.tasks.findIndex(t => t.id === id);
        if (index > -1) {
            const removed = this.tasks.splice(index, 1)[0];
            console.log(`Deleted: ${removed.title}`);
            return removed;
        }
        console.log("Task not found");
        return null;
    }

    // Get pending tasks
    getPendingTasks() {
        return this.tasks.filter(t => !t.completed);
    }

    // Get completed tasks
    getCompletedTasks() {
        return this.tasks.filter(t => t.completed);
    }

    // Get tasks by priority
    getTasksByPriority(priority) {
        return this.tasks.filter(t => 
            t.priority.toLowerCase() === priority.toLowerCase()
        );
    }

    // Sort by priority
    sortByPriority() {
        const priorityOrder = { high: 0, medium: 1, low: 2 };
        return [...this.tasks].sort((a, b) => 
            priorityOrder[a.priority] - priorityOrder[b.priority]
        );
    }

    // Display tasks
    displayTasks() {
        console.log("\n--- Tasks ---");
        this.tasks.forEach((task, index) => {
            const status = task.completed ? "✓" : "○";
            console.log(`${status} ${index + 1}. ${task.title} (${task.priority})`);
        });
        console.log(`Total: ${this.tasks.length} tasks`);
        console.log(`Pending: ${this.getPendingTasks().length}`);
        console.log(`Completed: ${this.getCompletedTasks().length}`);
    }
}

// Test the TaskManager
const taskManager = new TaskManager();

// Add tasks
taskManager.addTask("Complete project", "high");
taskManager.addTask("Review code", "medium");
taskManager.addTask("Write documentation", "low");
taskManager.addTask("Fix bugs", "high");
taskManager.addTask("Team meeting", "medium");

taskManager.displayTasks();

// Complete some tasks
console.log("\n--- Complete Tasks ---");
const task1 = taskManager.tasks[0];
const task2 = taskManager.tasks[2];
taskManager.completeTask(task1.id);
taskManager.completeTask(task2.id);

taskManager.displayTasks();

// Get pending tasks
console.log("\n--- Pending Tasks ---");
const pending = taskManager.getPendingTasks();
console.log("Pending:", pending);

// Get high priority tasks
console.log("\n--- High Priority Tasks ---");
const highPriority = taskManager.getTasksByPriority("high");
console.log("High priority:", highPriority);

// Sort by priority
console.log("\n--- Sorted by Priority ---");
const sorted = taskManager.sortByPriority();
console.log(sorted);
```

### Exercise 10: Complete Working Example

**Complete script.js:**
```javascript
// Session 5: Arrays
// This script demonstrates array operations and methods

console.log("=== Session 5: Arrays ===");

// 1. Array creation
console.log("\n--- Array Creation ---");
let fruits = ["apple", "banana", "orange"];
let numbers = [1, 2, 3, 4, 5];
console.log("Fruits:", fruits);
console.log("Numbers:", numbers);

// 2. Adding elements
console.log("\n--- Adding Elements ---");
fruits.push("grape");
console.log("After push:", fruits);
fruits.unshift("pear");
console.log("After unshift:", fruits);

// 3. Removing elements
console.log("\n--- Removing Elements ---");
let removed = fruits.pop();
console.log("Removed:", removed);
console.log("After pop:", fruits);

// 4. Searching
console.log("\n--- Searching ---");
console.log("indexOf('banana'):", fruits.indexOf("banana"));
console.log("includes('orange'):", fruits.includes("orange"));

// 5. Sorting
console.log("\n--- Sorting ---");
let unsorted = [10, 5, 100, 1, 50];
console.log("Original:", unsorted);
unsorted.sort((a, b) => a - b);
console.log("Sorted:", unsorted);

// 6. Array methods
console.log("\n--- Array Methods ---");
let doubled = numbers.map(n => n * 2);
console.log("Mapped:", doubled);

let even = numbers.filter(n => n % 2 === 0);
console.log("Filtered:", even);

let sum = numbers.reduce((acc, n) => acc + n, 0);
console.log("Reduced:", sum);

// 7. Slicing and joining
console.log("\n--- Slicing and Joining ---");
console.log("slice(1, 3):", fruits.slice(1, 3));
console.log("join(', '):", fruits.join(", "));

console.log("\n=== Session 5 Complete ===");
```

**Complete index.html:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Session 5 - Arrays</title>
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
        .manager {
            background: #f9f9f9;
            padding: 15px;
            border-radius: 4px;
            margin-top: 10px;
        }
        .manager input, .manager select {
            padding: 8px;
            margin: 5px 0;
            border: 1px solid #ddd;
            border-radius: 4px;
            width: 200px;
        }
        .manager button {
            padding: 10px 20px;
            background-color: #1890ff;
            color: white;
            border: none;
            border-radius: 4px;
            cursor: pointer;
            margin: 5px;
        }
        .manager button:hover {
            background-color: #0c7cd5;
        }
        .manager button.danger {
            background-color: #ff4d4f;
        }
        .manager button.danger:hover {
            background-color: #cf1322;
        }
        .list {
            background: white;
            padding: 15px;
            border-radius: 4px;
            margin-top: 15px;
            border: 1px solid #e8e8e8;
        }
        .list-item {
            padding: 10px;
            border-bottom: 1px solid #f0f0f0;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        .list-item:last-child {
            border-bottom: none;
        }
        .list-item .actions button {
            padding: 5px 10px;
            margin: 0 2px;
            font-size: 12px;
        }
        .priority-high { color: #ff4d4f; }
        .priority-medium { color: #faad14; }
        .priority-low { color: #52c41a; }
        .completed {
            text-decoration: line-through;
            color: #999;
        }
    </style>
</head>
<body>
    <h1>Session 5: Arrays</h1>
    
    <div class="section">
        <h2>Topics Covered</h2>
        <ul>
            <li>Array creation and basics</li>
            <li>Adding and removing elements</li>
            <li>Searching in arrays</li>
            <li>Sorting arrays</li>
            <li>Slicing and joining</li>
            <li>Array iteration methods (forEach, map, filter, reduce)</li>
        </ul>
    </div>

    <div class="section">
        <h2>Product List Manager</h2>
        <div class="manager">
            <input type="text" id="productName" placeholder="Product name">
            <input type="number" id="productPrice" placeholder="Price" step="0.01">
            <select id="productCategory">
                <option value="Electronics">Electronics</option>
                <option value="Books">Books</option>
                <option value="Clothing">Clothing</option>
                <option value="Food">Food</option>
            </select>
            <button onclick="addProduct()">Add Product</button>
            
            <div class="list" id="productList">
                <p>No products yet</p>
            </div>
            
            <div style="margin-top: 15px;">
                <button onclick="sortProductsByPrice('asc')">Sort by Price (Low to High)</button>
                <button onclick="sortProductsByPrice('desc')">Sort by Price (High to Low)</button>
                <button onclick="filterByCategory()">Filter by Category</button>
                <button onclick="showAllProducts()">Show All</button>
            </div>
            
            <div style="margin-top: 10px;">
                <strong>Total Value: $<span id="totalValue">0.00</span></strong>
                <strong style="margin-left: 20px;">Count: <span id="productCount">0</span></strong>
            </div>
        </div>
    </div>

    <div class="section">
        <h2>Task List Manager</h2>
        <div class="manager">
            <input type="text" id="taskTitle" placeholder="Task title">
            <select id="taskPriority">
                <option value="high">High Priority</option>
                <option value="medium" selected>Medium Priority</option>
                <option value="low">Low Priority</option>
            </select>
            <button onclick="addTask()">Add Task</button>
            
            <div class="list" id="taskList">
                <p>No tasks yet</p>
            </div>
            
            <div style="margin-top: 15px;">
                <button onclick="showPendingTasks()">Show Pending</button>
                <button onclick="showCompletedTasks()">Show Completed</button>
                <button onclick="showAllTasks()">Show All</button>
                <button onclick="sortByPriority()">Sort by Priority</button>
            </div>
            
            <div style="margin-top: 10px;">
                <strong>Pending: <span id="pendingCount">0</span></strong>
                <strong style="margin-left: 20px;">Completed: <span id="completedCount">0</span></strong>
            </div>
        </div>
    </div>

    <div class="section">
        <h2>Console Output</h2>
        <p>Open the browser console (F12) to see all JavaScript examples.</p>
    </div>

    <script src="script.js" defer></script>
    <script>
        // Product Manager
        let products = [];

        function addProduct() {
            const name = document.getElementById('productName').value;
            const price = parseFloat(document.getElementById('productPrice').value);
            const category = document.getElementById('productCategory').value;

            if (!name || isNaN(price)) {
                alert('Please enter a valid name and price');
                return;
            }

            const product = {
                id: Date.now(),
                name,
                price,
                category
            };

            products.push(product);
            renderProducts();
            clearProductInputs();
        }

        function removeProduct(id) {
            products = products.filter(p => p.id !== id);
            renderProducts();
        }

        function sortProductsByPrice(order) {
            products.sort((a, b) => order === 'asc' ? a.price - b.price : b.price - a.price);
            renderProducts();
        }

        function filterByCategory() {
            const category = document.getElementById('productCategory').value;
            const filtered = products.filter(p => p.category === category);
            renderProducts(filtered);
        }

        function showAllProducts() {
            renderProducts();
        }

        function renderProducts(productList = products) {
            const listDiv = document.getElementById('productList');
            
            if (productList.length === 0) {
                listDiv.innerHTML = '<p>No products</p>';
            } else {
                listDiv.innerHTML = productList.map(product => `
                    <div class="list-item">
                        <div>
                            <strong>${product.name}</strong> - $${product.price.toFixed(2)} (${product.category})
                        </div>
                        <div class="actions">
                            <button class="danger" onclick="removeProduct(${product.id})">Remove</button>
                        </div>
                    </div>
                `).join('');
            }

            document.getElementById('totalValue').textContent = products.reduce((sum, p) => sum + p.price, 0).toFixed(2);
            document.getElementById('productCount').textContent = products.length;
        }

        function clearProductInputs() {
            document.getElementById('productName').value = '';
            document.getElementById('productPrice').value = '';
        }

        // Task Manager
        let tasks = [];

        function addTask() {
            const title = document.getElementById('taskTitle').value;
            const priority = document.getElementById('taskPriority').value;

            if (!title) {
                alert('Please enter a task title');
                return;
            }

            const task = {
                id: Date.now(),
                title,
                priority,
                completed: false
            };

            tasks.push(task);
            renderTasks();
            document.getElementById('taskTitle').value = '';
        }

        function toggleTask(id) {
            const task = tasks.find(t => t.id === id);
            if (task) {
                task.completed = !task.completed;
                renderTasks();
            }
        }

        function deleteTask(id) {
            tasks = tasks.filter(t => t.id !== id);
            renderTasks();
        }

        function showPendingTasks() {
            const pending = tasks.filter(t => !t.completed);
            renderTasks(pending);
        }

        function showCompletedTasks() {
            const completed = tasks.filter(t => t.completed);
            renderTasks(completed);
        }

        function showAllTasks() {
            renderTasks();
        }

        function sortByPriority() {
            const priorityOrder = { high: 0, medium: 1, low: 2 };
            tasks.sort((a, b) => priorityOrder[a.priority] - priorityOrder[b.priority]);
            renderTasks();
        }

        function renderTasks(taskList = tasks) {
            const listDiv = document.getElementById('taskList');
            
            if (taskList.length === 0) {
                listDiv.innerHTML = '<p>No tasks</p>';
            } else {
                listDiv.innerHTML = taskList.map(task => `
                    <div class="list-item ${task.completed ? 'completed' : ''}">
                        <div>
                            <span class="priority-${task.priority}">[${task.priority.toUpperCase()}]</span>
                            ${task.title}
                        </div>
                        <div class="actions">
                            <button onclick="toggleTask(${task.id})">${task.completed ? 'Undo' : 'Complete'}</button>
                            <button class="danger" onclick="deleteTask(${task.id})">Delete</button>
                        </div>
                    </div>
                `).join('');
            }

            document.getElementById('pendingCount').textContent = tasks.filter(t => !t.completed).length;
            document.getElementById('completedCount').textContent = tasks.filter(t => t.completed).length;
        }
    </script>
</body>
</html>
```

---

## 📝 Review (0.5h)

### Q&A

**Q1: What is the difference between `push()` and `unshift()`?**
A: `push()` adds elements to the end of an array, while `unshift()` adds elements to the beginning of an array.

**Q2: How do you remove the last element from an array?**
A: Use the `pop()` method, which removes and returns the last element.

**Q3: What is the difference between `slice()` and `splice()`?**
A: `slice()` extracts a portion of an array without modifying the original, while `splice()` can add/remove elements and modifies the original array.

**Q4: Why does `sort()` not work correctly for numbers by default?**
A: Because `sort()` converts elements to strings and compares them lexicographically. Use a compare function `(a, b) => a - b` for numeric sorting.

**Q5: What is the difference between `map()` and `forEach()`?**
A: `map()` creates a new array with transformed elements, while `forEach()` executes a function for each element without returning a new array.

**Q6: How do you check if an array contains a specific value?**
A: Use the `includes()` method for simple values, or `find()` for complex conditions with objects.

**Q7: What does `reduce()` do?**
A: `reduce()` iterates through an array and reduces it to a single value by applying a function to each element.

**Q8: How do you clone an array?**
A: Use `slice()`, spread operator `[...array]`, or `Array.from(array)`.

**Q9: What is the difference between `filter()` and `find()`?**
A: `filter()` returns all elements that match the condition, while `find()` returns only the first matching element.

**Q10: How do you remove an element from the middle of an array?**
A: Use `splice(index, 1)` to remove one element at the specified index.

### Review Questions

1. **Which method adds an element to the end of an array?**
   - [ ] unshift()
   - [ ] push()
   - [ ] pop()
   - [ ] shift()

2. **What does `pop()` do?**
   - [ ] Adds element to end
   - [ ] Removes first element
   - [ ] Removes last element
   - [ ] Adds element to beginning

3. **Which method is used to search for an element in an array?**
   - [ ] search()
   - [ ] find()
   - [ ] locate()
   - [ ] get()

4. **What does `map()` return?**
   - [ ] The original array
   - [ ] A new array with transformed elements
   - [ ] A single value
   - [ ] Nothing

5. **How do you sort numbers correctly?**
   - [ ] array.sort()
   - [ ] array.sort((a, b) => a - b)
   - [ ] array.sort((a, b) => b - a)
   - [ ] array.order()

6. **What does `filter()` do?**
   - [ ] Modifies original array
   - [ ] Returns elements that pass condition
   - [ ] Returns first matching element
   - [ ] Removes all elements

7. **Which method concatenates arrays?**
   - [ ] join()
   - [ ] concat()
   - [ ] merge()
   - [ ] combine()

8. **What does `slice()` do to the original array?**
   - [ ] Modifies it
   - [ ] Does not modify it
   - [ ] Deletes elements
   - [ ] Adds elements

9. **How do you get the length of an array?**
   - [ ] array.size
   - [ ] array.length
   - [ ] array.count
   - [ ] array.total

10. **What does `reduce()` return?**
    - [ ] A new array
    - [ ] A single value
    - [ ] The original array
    - [ ] Nothing

### Correct Answers

1. ✅ push()
2. ✅ Removes last element
3. ✅ find()
4. ✅ A new array with transformed elements
5. ✅ array.sort((a, b) => a - b)
6. ✅ Returns elements that pass condition
7. ✅ concat()
8. ✅ Does not modify it
9. ✅ array.length
10. ✅ A single value

---

## 🎯 Next Steps

1. ✅ Practice all array methods
2. ✅ Build array-based applications
3. ✅ Master array iteration methods
4. ✅ Understand when to use map vs filter vs reduce
5. ✅ Practice array manipulation challenges
6. ✅ Learn about array destructuring
7. ✅ Explore advanced array methods (flat, flatMap, etc.)

---

## 📚 Additional Resources

- [MDN: Array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)
- [MDN: Array Methods](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array#instance_methods)
- [JavaScript.info: Arrays](https://javascript.info/array)
- [JavaScript.info: Array Methods](https://javascript.info/array-methods)
- [Array Explorer](https://arrayexplorer.com/)

**Remember:** Arrays are fundamental data structures in JavaScript. Mastering array methods will make you a more efficient and effective developer! 💪
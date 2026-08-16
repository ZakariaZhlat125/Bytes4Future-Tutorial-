# Session 8: Scope, Arrow Functions & Higher-Order Functions

## 📚 Theory (1h)

### Scope

Scope determines the accessibility (visibility) of variables, functions, and objects in different parts of your code.

#### Global Scope

Variables declared outside any function are in the global scope and can be accessed from anywhere.

```javascript
// Global scope
let globalVar = "I'm global";

function accessGlobal() {
    console.log(globalVar); // Can access global variable
}

accessGlobal(); // "I'm global"
console.log(globalVar); // "I'm global"
```

#### Local Scope (Function Scope)

Variables declared inside a function are local to that function and cannot be accessed from outside.

```javascript
function localScopeExample() {
    let localVar = "I'm local";
    console.log(localVar); // "I'm local"
}

localScopeExample();
console.log(localVar); // ReferenceError: localVar is not defined
```

#### Block Scope

Variables declared with `let` and `const` inside a block `{}` are block-scoped.

```javascript
function blockScopeExample() {
    if (true) {
        let blockVar = "I'm block-scoped";
        const blockConst = "I'm also block-scoped";
        console.log(blockVar); // "I'm block-scoped"
    }
    
    console.log(blockVar); // ReferenceError: blockVar is not defined
}

blockScopeExample();
```

#### Lexical Scope

Functions can access variables from their outer (enclosing) scope due to lexical scoping.

```javascript
function outerFunction() {
    let outerVar = "I'm from outer scope";
    
    function innerFunction() {
        let innerVar = "I'm from inner scope";
        console.log(outerVar); // Can access outer scope
        console.log(innerVar); // Can access inner scope
    }
    
    innerFunction();
    // console.log(innerVar); // ReferenceError: innerVar is not defined
}

outerFunction();
```

#### Scope Chain

JavaScript looks for variables in the current scope, then in outer scopes, until it reaches the global scope.

```javascript
let globalVar = "global";

function level1() {
    let level1Var = "level 1";
    
    function level2() {
        let level2Var = "level 2";
        
        function level3() {
            console.log(globalVar);  // Found in global scope
            console.log(level1Var);  // Found in level1 scope
            console.log(level2Var);  // Found in level2 scope
            // console.log(level3Var); // Would be ReferenceError
        }
        
        level3();
    }
    
    level2();
}

level1();
```

#### Variable Shadowing

When a variable in an inner scope has the same name as a variable in an outer scope, the inner variable shadows the outer one.

```javascript
let x = "global";

function shadowingExample() {
    let x = "local";
    console.log(x); // "local" (shadows global x)
    
    if (true) {
        let x = "block";
        console.log(x); // "block" (shadows local x)
    }
    
    console.log(x); // "local"
}

shadowingExample();
console.log(x); // "global"
```

#### Hoisting

Variable and function declarations are moved to the top of their scope during compilation.

```javascript
// Function hoisting
hoistedFunction(); // Works! Function is hoisted

function hoistedFunction() {
    console.log("I'm hoisted!");
}

// Variable hoisting (var)
console.log(hoistedVar); // undefined (not ReferenceError)
var hoistedVar = "I'm hoisted";

// let and const are not hoisted in the same way
// console.log(notHoisted); // ReferenceError
let notHoisted = "I'm not hoisted";
```

### Arrow Function Challenge

Arrow functions provide a concise syntax and have different `this` behavior compared to regular functions.

#### Basic Arrow Function Syntax

```javascript
// Traditional function
function add(a, b) {
    return a + b;
}

// Arrow function
const add = (a, b) => {
    return a + b;
};

// Concise arrow function (single expression)
const add = (a, b) => a + b;
```

#### Arrow Function Variations

```javascript
// No parameters
const sayHello = () => {
    console.log("Hello!");
};

// Single parameter (parentheses optional)
const square = x => x * x;

// Multiple parameters (parentheses required)
const add = (a, b) => a + b;

// Multiple statements (braces required)
const calculate = (a, b) => {
    const sum = a + b;
    const product = a * b;
    return { sum, product };
};
```

#### Arrow Functions and `this`

Arrow functions do not have their own `this` context; they inherit `this` from the surrounding scope.

```javascript
// Regular function - this changes
const obj = {
    value: 42,
    regular: function() {
        console.log(this.value); // 42
    },
    arrow: () => {
        console.log(this.value); // undefined (this is not obj)
    }
};

obj.regular(); // 42
obj.arrow();  // undefined

// Arrow function inherits this
const obj2 = {
    value: 42,
    method: function() {
        const arrow = () => {
            console.log(this.value); // 42 (inherits from method)
        };
        arrow();
    }
};

obj2.method(); // 42
```

#### Arrow Functions as Callbacks

```javascript
const numbers = [1, 2, 3, 4, 5];

// Traditional callback
const doubled = numbers.map(function(num) {
    return num * 2;
});

// Arrow callback (concise)
const doubled = numbers.map(num => num * 2);

// Arrow callback with multiple operations
const processed = numbers.map(num => {
    const doubled = num * 2;
    const squared = doubled * doubled;
    return squared;
});
```

#### Arrow Functions with Objects

```javascript
// Need parentheses to return object literal
const createUser = (name, age) => ({
    name: name,
    age: age,
    id: Date.now()
});

// Without parentheses, it's interpreted as code block
const wrong = (name, age) => {
    name: name,  // This is a label, not object property
    age: age
};

console.log(createUser("John", 30)); // {name: "John", age: 30, id: ...}
```

#### Arrow Functions Limitations

```javascript
// Cannot be used as constructors
const Person = (name) => {
    this.name = name;
};

// const john = new Person("John"); // TypeError: Person is not a constructor

// No arguments object
const regular = function() {
    console.log(arguments);
};

const arrow = () => {
    // console.log(arguments); // ReferenceError: arguments is not defined
};

// Cannot use yield (not suitable for generators)
```

---

## 💻 Practical (1.5h)

### Exercise 1: Understanding Scope

```javascript
// Exercise 1.1: Global vs Local Scope
console.log("=== Global vs Local Scope ===");

let globalVar = "Global";

function scopeTest() {
    let localVar = "Local";
    console.log("Inside function:");
    console.log("Global:", globalVar);
    console.log("Local:", localVar);
}

scopeTest();
console.log("\nOutside function:");
console.log("Global:", globalVar);
// console.log("Local:", localVar); // ReferenceError

// Exercise 1.2: Block Scope
console.log("\n=== Block Scope ===");

function blockScopeTest() {
    if (true) {
        let blockVar = "Block";
        const blockConst = "Block Const";
        console.log("Inside block:", blockVar);
    }
    // console.log(blockVar); // ReferenceError
    console.log("Block variable not accessible outside");
}

blockScopeTest();

// Exercise 1.3: Lexical Scope
console.log("\n=== Lexical Scope ===");

function outer() {
    let outerVar = "Outer";
    
    function inner() {
        let innerVar = "Inner";
        console.log("Inner can access outer:", outerVar);
        console.log("Inner can access inner:", innerVar);
    }
    
    inner();
    // console.log(innerVar); // ReferenceError
}

outer();

// Exercise 1.4: Scope Chain
console.log("\n=== Scope Chain ===");

let level0 = "Level 0";

function level1() {
    let level1 = "Level 1";
    
    function level2() {
        let level2 = "Level 2";
        
        function level3() {
            console.log(level0); // From global
            console.log(level1); // From level1
            console.log(level2); // From level2
        }
        
        level3();
    }
    
    level2();
}

level1();
```

### Exercise 2: Arrow Functions

```javascript
// Exercise 2.1: Basic arrow functions
console.log("=== Basic Arrow Functions ===");

// Traditional to arrow conversion
const addTraditional = function(a, b) {
    return a + b;
};

const addArrow = (a, b) => a + b;

console.log("Traditional:", addTraditional(5, 3));
console.log("Arrow:", addArrow(5, 3));

// Exercise 2.2: Single parameter
console.log("\n=== Single Parameter ===");

const square = x => x * x;
const cube = x => x * x * x;

console.log("Square of 5:", square(5));
console.log("Cube of 3:", cube(3));

// Exercise 2.3: No parameters
console.log("\n=== No Parameters ===");

const getRandom = () => Math.random();
const getCurrentTime = () => new Date().toLocaleTimeString();

console.log("Random:", getRandom());
console.log("Time:", getCurrentTime());

// Exercise 2.4: Multiple statements
console.log("\n=== Multiple Statements ===");

const processNumber = num => {
    const doubled = num * 2;
    const squared = doubled * doubled;
    return {
        original: num,
        doubled: doubled,
        squared: squared
    };
};

console.log(processNumber(5));
```

### Exercise 3: Arrow Functions with `this`

```javascript
// Exercise 3.1: Regular vs Arrow functions with this
console.log("=== Regular vs Arrow with this ===");

const person = {
    name: "John",
    age: 30,
    
    regularGreet: function() {
        console.log(`Regular: Hello, I'm ${this.name}`);
    },
    
    arrowGreet: () => {
        console.log(`Arrow: Hello, I'm ${this.name}`); // this is not person
    },
    
    delayedGreet: function() {
        setTimeout(() => {
            console.log(`Delayed: Hello, I'm ${this.name}`); // this is person
        }, 100);
    }
};

person.regularGreet(); // "Regular: Hello, I'm John"
person.arrowGreet();  // "Arrow: Hello, I'm undefined"
person.delayedGreet(); // "Delayed: Hello, I'm John" (after 100ms)

// Exercise 3.2: Arrow functions in object methods
console.log("\n=== Arrow in Object Methods ===");

const calculator = {
    value: 0,
    
    add: function(num) {
        this.value += num;
        return this.value;
    },
    
    // Arrow function doesn't work well as method
    subtract: (num) => {
        // this is not calculator
        // this.value -= num; // This won't work
        return "Arrow function doesn't have correct this";
    }
};

console.log("Add 5:", calculator.add(5));
console.log("Add 3:", calculator.add(3));
console.log("Subtract:", calculator.subtract(2));
```

### Exercise 4: forEach

```javascript
// Exercise 4.1: Basic forEach
console.log("=== Basic forEach ===");

const fruits = ["apple", "banana", "orange", "grape"];

fruits.forEach((fruit, index) => {
    console.log(`${index}: ${fruit}`);
});

// Exercise 4.2: forEach with objects
console.log("\n=== forEach with Objects ===");

const users = [
    { name: "John", age: 30 },
    { name: "Jane", age: 25 },
    { name: "Bob", age: 35 }
];

users.forEach(user => {
    console.log(`${user.name} is ${user.age} years old`);
});

// Exercise 4.3: Practice - Calculate total age
console.log("\n=== Calculate Total Age ===");

let totalAge = 0;
users.forEach(user => {
    totalAge += user.age;
});
console.log("Total age:", totalAge);
console.log("Average age:", totalAge / users.length);

// Exercise 4.4: Practice - Create new array with forEach
console.log("\n=== Create Array with forEach ===");

const names = [];
users.forEach(user => {
    names.push(user.name.toUpperCase());
});
console.log("Uppercase names:", names);

// Exercise 4.5: Practice - Filter with forEach
console.log("\n=== Filter with forEach ===");

const adults = [];
users.forEach(user => {
    if (user.age >= 30) {
        adults.push(user);
    }
});
console.log("Adults:", adults);
```

### Exercise 5: map

```javascript
// Exercise 5.1: Basic map
console.log("=== Basic map ===");

const numbers = [1, 2, 3, 4, 5];

const doubled = numbers.map(num => num * 2);
console.log("Original:", numbers);
console.log("Doubled:", doubled);

// Exercise 5.2: map with objects
console.log("\n=== map with Objects ===");

const products = [
    { name: "Laptop", price: 1000 },
    { name: "Phone", price: 500 },
    { name: "Tablet", price: 300 }
];

const productNames = products.map(product => product.name);
console.log("Product names:", productNames);

// Exercise 5.3: Practice - Add tax to prices
console.log("\n=== Add Tax to Prices ===");

const withTax = products.map(product => ({
    ...product,
    priceWithTax: product.price * 1.1
}));
console.log("With tax:", withTax);

// Exercise 5.4: Practice - Extract specific properties
console.log("\n=== Extract Properties ===");

const users = [
    { id: 1, name: "John", email: "john@example.com", age: 30 },
    { id: 2, name: "Jane", email: "jane@example.com", age: 25 },
    { id: 3, name: "Bob", email: "bob@example.com", age: 35 }
];

const userSummaries = users.map(({ id, name }) => ({ id, name }));
console.log("User summaries:", userSummaries);

// Exercise 5.5: Practice - Transform array of strings
console.log("\n=== Transform Strings ===");

const words = ["hello", "world", "javascript"];
const capitalized = words.map(word => 
    word.charAt(0).toUpperCase() + word.slice(1)
);
console.log("Capitalized:", capitalized);
```

### Exercise 6: filter

```javascript
// Exercise 6.1: Basic filter
console.log("=== Basic filter ===");

const numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

const evenNumbers = numbers.filter(num => num % 2 === 0);
const oddNumbers = numbers.filter(num => num % 2 !== 0);

console.log("Even:", evenNumbers);
console.log("Odd:", oddNumbers);

// Exercise 6.2: filter with objects
console.log("\n=== filter with Objects ===");

const users = [
    { name: "John", age: 30, active: true },
    { name: "Jane", age: 25, active: false },
    { name: "Bob", age: 35, active: true },
    { name: "Alice", age: 28, active: false }
];

const activeUsers = users.filter(user => user.active);
const adults = users.filter(user => user.age >= 30);

console.log("Active users:", activeUsers);
console.log("Adults:", adults);

// Exercise 6.3: Practice - Filter by multiple conditions
console.log("\n=== Multiple Conditions ===");

const activeAdults = users.filter(user => user.active && user.age >= 30);
console.log("Active adults:", activeAdults);

// Exercise 6.4: Practice - Filter strings
console.log("\n=== Filter Strings ===");

const words = ["hello", "world", "javascript", "code", "programming"];
const longWords = words.filter(word => word.length > 5);
const wordsWithJ = words.filter(word => word.toLowerCase().includes("j"));

console.log("Long words:", longWords);
console.log("Words with 'j':", wordsWithJ);

// Exercise 6.5: Practice - Remove duplicates
console.log("\n=== Remove Duplicates ===");

const numbersWithDuplicates = [1, 2, 2, 3, 4, 4, 5, 5, 5];
const uniqueNumbers = numbersWithDuplicates.filter((num, index, arr) => 
    arr.indexOf(num) === index
);
console.log("Unique:", uniqueNumbers);
```

### Exercise 7: reduce

```javascript
// Exercise 7.1: Basic reduce - sum
console.log("=== Basic reduce - Sum ===");

const numbers = [1, 2, 3, 4, 5];

const sum = numbers.reduce((accumulator, currentValue) => 
    accumulator + currentValue, 0
);
console.log("Sum:", sum);

// Exercise 7.2: reduce - product
console.log("\n=== reduce - Product ===");

const product = numbers.reduce((acc, num) => acc * num, 1);
console.log("Product:", product);

// Exercise 7.3: reduce with objects
console.log("\n=== reduce with Objects ===");

const cart = [
    { item: "Book", price: 10, quantity: 2 },
    { item: "Pen", price: 2, quantity: 5 },
    { item: "Notebook", price: 5, quantity: 3 }
];

const total = cart.reduce((acc, item) => 
    acc + (item.price * item.quantity), 0
);
console.log("Cart total:", total);

// Exercise 7.4: Practice - Group by property
console.log("\n=== Group by Property ===");

const users = [
    { name: "John", department: "IT" },
    { name: "Jane", department: "HR" },
    { name: "Bob", department: "IT" },
    { name: "Alice", department: "HR" },
    { name: "Charlie", department: "Finance" }
];

const groupedByDept = users.reduce((acc, user) => {
    const dept = user.department;
    if (!acc[dept]) {
        acc[dept] = [];
    }
    acc[dept].push(user.name);
    return acc;
}, {});

console.log("Grouped by department:", groupedByDept);

// Exercise 7.5: Practice - Find maximum
console.log("\n=== Find Maximum ===");

const scores = [85, 92, 78, 95, 88];
const maxScore = scores.reduce((max, current) => 
    current > max ? current : max, scores[0]
);
console.log("Max score:", maxScore);

// Exercise 7.6: Practice - Count occurrences
console.log("\n=== Count Occurrences ===");

const fruits = ["apple", "banana", "apple", "orange", "banana", "apple"];
const fruitCounts = fruits.reduce((acc, fruit) => {
    acc[fruit] = (acc[fruit] || 0) + 1;
    return acc;
}, {});

console.log("Fruit counts:", fruitCounts);
```

### Exercise 8: Higher-Order Functions Practice

```javascript
// Exercise 8.1: Chaining HOFs
console.log("=== Chaining Higher-Order Functions ===");

const numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

// Filter even, double them, then sum
const result = numbers
    .filter(num => num % 2 === 0)
    .map(num => num * 2)
    .reduce((sum, num) => sum + num, 0);

console.log("Even doubled sum:", result);

// Exercise 8.2: Practice - Process user data
console.log("\n=== Process User Data ===");

const users = [
    { name: "John", age: 30, salary: 50000 },
    { name: "Jane", age: 25, salary: 60000 },
    { name: "Bob", age: 35, salary: 45000 },
    { name: "Alice", age: 28, salary: 70000 }
];

// Get names of users earning over 55000, sorted by age
const highEarners = users
    .filter(user => user.salary > 55000)
    .sort((a, b) => a.age - b.age)
    .map(user => user.name);

console.log("High earners sorted by age:", highEarners);

// Exercise 8.3: Practice - Word analysis
console.log("\n=== Word Analysis ===");

const text = "hello world hello javascript world code";
const words = text.split(" ");

// Get unique words, capitalize them
const uniqueCapitalized = [...new Set(words)]
    .map(word => word.charAt(0).toUpperCase() + word.slice(1));

console.log("Unique capitalized:", uniqueCapitalized);

// Count word lengths
const wordLengths = words.reduce((acc, word) => {
    acc[word] = word.length;
    return acc;
}, {});

console.log("Word lengths:", wordLengths);

// Exercise 8.4: Practice - Data transformation pipeline
console.log("\n=== Data Transformation Pipeline ===");

const rawData = [
    { id: 1, value: "10", status: "active" },
    { id: 2, value: "20", status: "inactive" },
    { id: 3, value: "30", status: "active" },
    { id: 4, value: "40", status: "pending" }
];

const processedData = rawData
    .filter(item => item.status === "active")
    .map(item => ({
        id: item.id,
        value: parseInt(item.value),
        doubled: parseInt(item.value) * 2
    }))
    .reduce((acc, item) => {
        acc.totalValue += item.value;
        acc.totalDoubled += item.doubled;
        acc.count++;
        return acc;
    }, { totalValue: 0, totalDoubled: 0, count: 0 });

console.log("Processed data:", processedData);
```

### Exercise 9: Complete Working Example

**Complete script.js:**
```javascript
// Session 8: Scope, Arrow Functions & Higher-Order Functions
// This script demonstrates scope, arrow functions, and HOFs

console.log("=== Session 8: Scope, Arrow Functions & HOFs ===");

// 1. Scope demonstration
console.log("\n--- Scope ---");
let globalVar = "Global";

function scopeDemo() {
    let localVar = "Local";
    console.log("Inside function - Global:", globalVar);
    console.log("Inside function - Local:", localVar);
}

scopeDemo();
console.log("Outside function - Global:", globalVar);

// 2. Arrow functions
console.log("\n--- Arrow Functions ---");
const add = (a, b) => a + b;
const square = x => x * x;
console.log("Add 5 + 3:", add(5, 3));
console.log("Square 4:", square(4));

// 3. Higher-Order Functions
console.log("\n--- Higher-Order Functions ---");
const numbers = [1, 2, 3, 4, 5];

// forEach
console.log("forEach:");
numbers.forEach(num => console.log(num));

// map
console.log("\nmap (doubled):");
const doubled = numbers.map(num => num * 2);
console.log(doubled);

// filter
console.log("\nfilter (even):");
const even = numbers.filter(num => num % 2 === 0);
console.log(even);

// reduce
console.log("\nreduce (sum):");
const sum = numbers.reduce((acc, num) => acc + num, 0);
console.log(sum);

// 4. Chaining HOFs
console.log("\n--- Chaining HOFs ---");
const result = numbers
    .filter(num => num % 2 === 0)
    .map(num => num * 2)
    .reduce((acc, num) => acc + num, 0);
console.log("Even doubled sum:", result);

console.log("\n=== Session 8 Complete ===");
```

**Complete index.html:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Session 8 - Scope, Arrow Functions & HOFs</title>
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
        .demo {
            background: #f9f9f9;
            padding: 15px;
            border-radius: 4px;
            margin-top: 10px;
        }
        .demo input, .demo select, .demo button {
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
        .hof-demo {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 20px;
            margin-top: 15px;
        }
        .hof-panel {
            background: white;
            padding: 15px;
            border-radius: 4px;
            border: 1px solid #e8e8e8;
        }
        .hof-panel h3 {
            margin-top: 0;
            color: #1890ff;
        }
    </style>
</head>
<body>
    <h1>Session 8: Scope, Arrow Functions & Higher-Order Functions</h1>
    
    <div class="section">
        <h2>Topics Covered</h2>
        <ul>
            <li>Scope (global, local, block, lexical)</li>
            <li>Arrow functions and their differences</li>
            <li>Higher-Order Functions (forEach, map, filter, reduce)</li>
            <li>Chaining HOFs for data transformation</li>
        </ul>
    </div>

    <div class="section">
        <h2>Scope Demonstration</h2>
        <div class="demo">
            <button onclick="demoScope()">Demonstrate Scope</button>
            <button onclick="demoClosure()">Demonstrate Closure</button>
            <div id="scopeOutput" class="output">
                Click to see scope demonstration...
            </div>
        </div>
    </div>

    <div class="section">
        <h2>Arrow Functions</h2>
        <div class="demo">
            <input type="number" id="arrowNum1" placeholder="Number 1">
            <input type="number" id="arrowNum2" placeholder="Number 2">
            <button onclick="demoArrowAdd()">Arrow Add</button>
            <button onclick="demoArrowSquare()">Arrow Square</button>
            <div id="arrowOutput" class="output">
                Click to see arrow function examples...
            </div>
        </div>
    </div>

    <div class="section">
        <h2>Higher-Order Functions</h2>
        <div class="hof-demo">
            <div class="hof-panel">
                <h3>forEach</h3>
                <div id="forEachOutput" class="output">
                    forEach results...
                </div>
            </div>
            <div class="hof-panel">
                <h3>map</h3>
                <div id="mapOutput" class="output">
                    map results...
                </div>
            </div>
            <div class="hof-panel">
                <h3>filter</h3>
                <div id="filterOutput" class="output">
                    filter results...
                </div>
            </div>
            <div class="hof-panel">
                <h3>reduce</h3>
                <div id="reduceOutput" class="output">
                    reduce results...
                </div>
            </div>
        </div>
        <div class="demo" style="margin-top: 15px;">
            <button onclick="runAllHOFs()">Run All HOFs</button>
            <button onclick="chainHOFs()">Chain HOFs</button>
            <div id="hofOutput" class="output">
                Click to see HOF demonstrations...
            </div>
        </div>
    </div>

    <div class="section">
        <h2>Console Output</h2>
        <p>Open the browser console (F12) to see all JavaScript examples.</p>
    </div>

    <script src="script.js" defer></script>
    <script>
        // Scope demonstration
        function demoScope() {
            let globalVar = "Global variable";
            let output = "=== Scope Demonstration ===\n\n";
            
            function outerFunction() {
                let outerVar = "Outer variable";
                
                function innerFunction() {
                    let innerVar = "Inner variable";
                    output += `Inner function can access:\n`;
                    output += `- Global: ${globalVar}\n`;
                    output += `- Outer: ${outerVar}\n`;
                    output += `- Inner: ${innerVar}\n`;
                }
                
                innerFunction();
                output += `\nOuter function can access:\n`;
                output += `- Global: ${globalVar}\n`;
                output += `- Outer: ${outerVar}\n`;
                // output += `- Inner: ${innerVar}\n`; // Would be error
            }
            
            outerFunction();
            output += `\nGlobal scope can access:\n`;
            output += `- Global: ${globalVar}\n`;
            
            document.getElementById('scopeOutput').textContent = output;
        }

        function demoClosure() {
            function createCounter() {
                let count = 0;
                
                return {
                    increment: () => {
                        count++;
                        return count;
                    },
                    getCount: () => count
                };
            }
            
            const counter = createCounter();
            let output = "=== Closure Demonstration ===\n\n";
            output += `Initial count: ${counter.getCount()}\n`;
            output += `After increment: ${counter.increment()}\n`;
            output += `After increment: ${counter.increment()}\n`;
            output += `Final count: ${counter.getCount()}\n`;
            
            document.getElementById('scopeOutput').textContent = output;
        }

        // Arrow functions
        function demoArrowAdd() {
            const num1 = parseFloat(document.getElementById('arrowNum1').value) || 0;
            const num2 = parseFloat(document.getElementById('arrowNum2').value) || 0;
            
            const add = (a, b) => a + b;
            const result = add(num1, num2);
            
            document.getElementById('arrowOutput').textContent = 
                `Arrow function add(${num1}, ${num2}) = ${result}`;
        }

        function demoArrowSquare() {
            const num = parseFloat(document.getElementById('arrowNum1').value) || 0;
            
            const square = x => x * x;
            const result = square(num);
            
            document.getElementById('arrowOutput').textContent = 
                `Arrow function square(${num}) = ${result}`;
        }

        // Higher-Order Functions
        function runAllHOFs() {
            const numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
            
            // forEach
            let forEachOutput = [];
            numbers.forEach(num => forEachOutput.push(num));
            document.getElementById('forEachOutput').textContent = 
                `Numbers: ${forEachOutput.join(', ')}`;
            
            // map
            const doubled = numbers.map(num => num * 2);
            document.getElementById('mapOutput').textContent = 
                `Doubled: ${doubled.join(', ')}`;
            
            // filter
            const even = numbers.filter(num => num % 2 === 0);
            document.getElementById('filterOutput').textContent = 
                `Even: ${even.join(', ')}`;
            
            // reduce
            const sum = numbers.reduce((acc, num) => acc + num, 0);
            document.getElementById('reduceOutput').textContent = 
                `Sum: ${sum}`;
        }

        function chainHOFs() {
            const numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
            
            const result = numbers
                .filter(num => num % 2 === 0)
                .map(num => num * 2)
                .reduce((acc, num) => acc + num, 0);
            
            let output = "=== Chaining HOFs ===\n\n";
            output += `Original: ${numbers.join(', ')}\n`;
            output += `Filtered (even): ${numbers.filter(n => n % 2 === 0).join(', ')}\n`;
            output += `Mapped (doubled): ${numbers.filter(n => n % 2 === 0).map(n => n * 2).join(', ')}\n`;
            output += `Reduced (sum): ${result}\n`;
            
            document.getElementById('hofOutput').textContent = output;
        }
    </script>
</body>
</html>
```

---

## 📝 Review (0.5h)

### HOF Challenge

```javascript
// Challenge 1: Use HOFs to process this data
const students = [
    { name: "John", score: 85, grade: "A" },
    { name: "Jane", score: 92, grade: "A" },
    { name: "Bob", score: 78, grade: "C" },
    { name: "Alice", score: 65, grade: "D" },
    { name: "Charlie", score: 88, grade: "B" }
];

// 1. Get names of students with grade "A"
// 2. Calculate average score
// 3. Find the student with the highest score
// 4. Group students by grade
// 5. Create array of objects with just name and score

// Challenge 2: Word processing
const text = "The quick brown fox jumps over the lazy dog the quick brown fox";

// 1. Split into words
// 2. Get unique words
// 3. Count occurrences of each word
// 4. Get words longer than 4 characters
// 5. Capitalize all words

// Challenge 3: Number operations
const numbers = [10, 20, 30, 40, 50, 60, 70, 80, 90, 100];

// 1. Get numbers divisible by 3
// 2. Square all numbers
// 3. Calculate sum of squares
// 4. Find the maximum number
// 5. Create an object with sum, avg, max, min

// Challenge 4: Array transformation
const data = [
    { id: 1, value: "100", active: true },
    { id: 2, value: "200", active: false },
    { id: 3, value: "300", active: true },
    { id: 4, value: "400", active: false }
];

// 1. Filter active items
// 2. Convert value strings to numbers
// 3. Add 10% to each value
// 4. Calculate total
// 5. Create summary object

// Challenge 5: Functional programming
// Create a function that takes an array and returns:
// - sum of all numbers
// - product of all numbers
// - average of all numbers
// using only reduce
```

### Challenge Solutions

```javascript
// Challenge 1: Student data
const students = [
    { name: "John", score: 85, grade: "A" },
    { name: "Jane", score: 92, grade: "A" },
    { name: "Bob", score: 78, grade: "C" },
    { name: "Alice", score: 65, grade: "D" },
    { name: "Charlie", score: 88, grade: "B" }
];

// 1. Names of students with grade "A"
const aStudents = students
    .filter(student => student.grade === "A")
    .map(student => student.name);
console.log("A students:", aStudents);

// 2. Average score
const avgScore = students.reduce((sum, student) => sum + student.score, 0) / students.length;
console.log("Average score:", avgScore);

// 3. Student with highest score
const topStudent = students.reduce((max, student) => 
    student.score > max.score ? student : max, students[0]);
console.log("Top student:", topStudent);

// 4. Group by grade
const byGrade = students.reduce((acc, student) => {
    if (!acc[student.grade]) acc[student.grade] = [];
    acc[student.grade].push(student.name);
    return acc;
}, {});
console.log("Grouped by grade:", byGrade);

// 5. Name and score only
const simplified = students.map(({ name, score }) => ({ name, score }));
console.log("Simplified:", simplified);

// Challenge 2: Word processing
const text = "The quick brown fox jumps over the lazy dog the quick brown fox";
const words = text.toLowerCase().split(" ");

// 1. Split into words (done above)
// 2. Unique words
const uniqueWords = [...new Set(words)];
console.log("Unique words:", uniqueWords);

// 3. Count occurrences
const wordCounts = words.reduce((acc, word) => {
    acc[word] = (acc[word] || 0) + 1;
    return acc;
}, {});
console.log("Word counts:", wordCounts);

// 4. Words longer than 4 characters
const longWords = words.filter(word => word.length > 4);
console.log("Long words:", longWords);

// 5. Capitalize all words
const capitalized = words.map(word => 
    word.charAt(0).toUpperCase() + word.slice(1)
);
console.log("Capitalized:", capitalized);

// Challenge 3: Number operations
const numbers = [10, 20, 30, 40, 50, 60, 70, 80, 90, 100];

// 1. Divisible by 3
const divisibleBy3 = numbers.filter(num => num % 3 === 0);
console.log("Divisible by 3:", divisibleBy3);

// 2. Square all
const squared = numbers.map(num => num * num);
console.log("Squared:", squared);

// 3. Sum of squares
const sumOfSquares = squared.reduce((sum, num) => sum + num, 0);
console.log("Sum of squares:", sumOfSquares);

// 4. Maximum
const max = Math.max(...numbers);
console.log("Maximum:", max);

// 5. Summary object
const summary = numbers.reduce((acc, num) => ({
    sum: acc.sum + num,
    count: acc.count + 1,
    max: Math.max(acc.max, num),
    min: Math.min(acc.min, num)
}), { sum: 0, count: 0, max: -Infinity, min: Infinity });

summary.avg = summary.sum / summary.count;
console.log("Summary:", summary);

// Challenge 4: Data transformation
const data = [
    { id: 1, value: "100", active: true },
    { id: 2, value: "200", active: false },
    { id: 3, value: "300", active: true },
    { id: 4, value: "400", active: false }
];

// 1. Filter active
const active = data.filter(item => item.active);
console.log("Active:", active);

// 2. Convert to numbers
const withNumbers = active.map(item => ({
    ...item,
    value: parseInt(item.value)
}));
console.log("With numbers:", withNumbers);

// 3. Add 10%
const withIncrease = withNumbers.map(item => ({
    ...item,
    value: item.value * 1.1
}));
console.log("With 10% increase:", withIncrease);

// 4. Calculate total
const total = withIncrease.reduce((sum, item) => sum + item.value, 0);
console.log("Total:", total);

// 5. Summary object
const dataSummary = {
    activeCount: active.length,
    totalValue: total,
    averageValue: total / active.length
};
console.log("Data summary:", dataSummary);

// Challenge 5: Functional programming
function arrayStats(numbers) {
    const sum = numbers.reduce((acc, num) => acc + num, 0);
    const product = numbers.reduce((acc, num) => acc * num, 1);
    const avg = sum / numbers.length;
    const max = Math.max(...numbers);
    const min = Math.min(...numbers);
    
    return { sum, product, avg, max, min };
}

console.log("Array stats:", arrayStats([1, 2, 3, 4, 5]));
```

### Review Questions

1. **What is the difference between global and local scope?**
   - [ ] No difference
   - [ ] Global scope is inside functions, local is outside
   - [ ] Global scope is accessible everywhere, local is function-specific
   - [ ] Local scope is accessible everywhere, global is function-specific

2. **What is lexical scope?**
   - [ ] Scope based on where a function is called
   - [ ] Scope based on where a function is defined
   - [ ] Scope that uses block scope only
   - [ ] Scope that's always global

3. **Which is the correct arrow function syntax for one parameter?**
   - [ ] (x) => x * 2
   - [ ] x => x * 2
   - [ ] => x * 2
   - [ ] x -> x * 2

4. **What is a higher-order function?**
   - [ ] A function that's higher in the code
   - [ ] A function that takes another function as argument or returns a function
   - [ ] A function with more parameters
   - [ ] A function that's called first

5. **What does `map()` do?**
   - [ ] Filters elements
   - [ ] Transforms each element
   - [ ] Reduces to single value
   - [ ] Executes function for each element

6. **What does `filter()` return?**
   - [ ] A single value
   - [ ] A new array with elements that pass the condition
   - [ ] The original array
   - [ ] Nothing

7. **What does `reduce()` return?**
   - [ ] A new array
   - [ ] A single value
   - [ ] The original array
   - [ ] A boolean

8. **What is the key difference between arrow functions and regular functions regarding `this`?**
   - [ ] Arrow functions have their own `this`
   - [ ] Regular functions inherit `this` from surrounding scope
   - [ ] Arrow functions inherit `this` from surrounding scope
   - [ ] No difference

9. **Which HOF would you use to calculate the sum of an array?**
   - [ ] map
   - [ ] filter
   - [ ] reduce
   - [ ] forEach

10. **What is block scope?**
    - [ ] Scope within a function
    - [ ] Scope within curly braces {}
    - [ ] Global scope
    - [ ] Scope within if statements only

### Correct Answers

1. ✅ Global scope is accessible everywhere, local is function-specific
2. ✅ Scope based on where a function is defined
3. ✅ x => x * 2
4. ✅ A function that takes another function as argument or returns a function
5. ✅ Transforms each element
6. ✅ A new array with elements that pass the condition
7. ✅ A single value
8. ✅ Arrow functions inherit `this` from surrounding scope
9. ✅ reduce
10. ✅ Scope within curly braces {}

---

## 🎯 Next Steps

1. ✅ Practice scope and understand variable accessibility
2. ✅ Master arrow function syntax and limitations
3. ✅ Understand when to use each HOF
4. ✅ Practice chaining HOFs for data transformation
5. ✅ Learn about functional programming concepts
6. ✅ Practice with real-world data processing
7. ✅ Explore advanced HOFs (find, some, every, flatMap)

---

## 📚 Additional Resources

- [MDN: Scope](https://developer.mozilla.org/en-US/docs/Glossary/Scope)
- [MDN: Arrow Functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions)
- [MDN: Array.prototype.forEach](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach)
- [MDN: Array.prototype.map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map)
- [MDN: Array.prototype.filter](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter)
- [MDN: Array.prototype.reduce](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce)
- [JavaScript.info: Scope](https://javascript.info/closure)
- [JavaScript.info: Arrow Functions](https://javascript.info/arrow-functions-basics)

**Remember:** Understanding scope and mastering higher-order functions are essential for writing clean, functional JavaScript code! 💪
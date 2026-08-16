# Session 7: Functions (Part 1)

## 📚 Theory (1h)

### Function Introduction & Basic Usage

Functions are reusable blocks of code that perform specific tasks. They help organize code, avoid repetition, and make programs more maintainable.

#### What is a Function?

A function is a block of code designed to perform a particular task. You can call (invoke) a function to execute its code.

#### Function Declaration

```javascript
function functionName(parameters) {
    // code to execute
    return result;
}
```

#### Basic Function Example

```javascript
// Function declaration
function greet(name) {
    console.log("Hello, " + name + "!");
}

// Function call
greet("John");  // "Hello, John!"
greet("Jane");  // "Hello, Jane!"
```

#### Function Components

1. **Function Name**: Identifies the function
2. **Parameters**: Input values (optional)
3. **Function Body**: Code to execute
4. **Return Statement**: Output value (optional)

#### Function with Parameters

```javascript
function add(a, b) {
    return a + b;
}

console.log(add(5, 3));  // 8
console.log(add(10, 20)); // 30
```

#### Function without Parameters

```javascript
function sayHello() {
    console.log("Hello, World!");
}

sayHello();  // "Hello, World!"
```

#### Function without Return Value

```javascript
function logMessage(message) {
    console.log(message);
    // No return statement (returns undefined)
}

let result = logMessage("Hello");
console.log(result);  // undefined
```

### Advanced Function Examples

#### Function with Multiple Parameters

```javascript
function createFullName(firstName, lastName, middleName) {
    if (middleName) {
        return `${firstName} ${middleName} ${lastName}`;
    }
    return `${firstName} ${lastName}`;
}

console.log(createFullName("John", "Doe"));                    // "John Doe"
console.log(createFullName("John", "Doe", "William"));          // "John William Doe"
```

#### Function with Conditional Logic

```javascript
function getGrade(score) {
    if (score >= 90) return "A";
    if (score >= 80) return "B";
    if (score >= 70) return "C";
    if (score >= 60) return "D";
    return "F";
}

console.log(getGrade(95));  // "A"
console.log(getGrade(75));  // "C"
```

#### Function with Array Processing

```javascript
function calculateAverage(numbers) {
    if (numbers.length === 0) return 0;
    
    let sum = 0;
    for (let num of numbers) {
        sum += num;
    }
    return sum / numbers.length;
}

console.log(calculateAverage([10, 20, 30, 40, 50]));  // 30
console.log(calculateAverage([]));                     // 0
```

#### Function with Object Parameters

```javascript
function createUser(user) {
    return {
        id: Date.now(),
        name: user.name,
        email: user.email,
        createdAt: new Date()
    };
}

const user = createUser({
    name: "John Doe",
    email: "john@example.com"
});

console.log(user);
// { id: 1234567890, name: "John Doe", email: "john@example.com", createdAt: Date }
```

#### Function Validation

```javascript
function calculateDiscount(price, discountPercentage) {
    // Validate inputs
    if (typeof price !== 'number' || price < 0) {
        return "Invalid price";
    }
    if (typeof discountPercentage !== 'number' || discountPercentage < 0 || discountPercentage > 100) {
        return "Invalid discount percentage";
    }
    
    return price * (1 - discountPercentage / 100);
}

console.log(calculateDiscount(100, 20));    // 80
console.log(calculateDiscount(-100, 20));   // "Invalid price"
console.log(calculateDiscount(100, 150));   // "Invalid discount percentage"
```

### Return Statement

The `return` statement ends function execution and returns a value to the caller.

#### Basic Return

```javascript
function add(a, b) {
    return a + b;
}

let result = add(5, 3);
console.log(result);  // 8
```

#### Multiple Return Statements

```javascript
function getNumberStatus(num) {
    if (num > 0) {
        return "positive";
    } else if (num < 0) {
        return "negative";
    } else {
        return "zero";
    }
}

console.log(getNumberStatus(5));    // "positive"
console.log(getNumberStatus(-5));   // "negative"
console.log(getNumberStatus(0));    // "zero"
```

#### Early Return Pattern

```javascript
function validateUser(user) {
    // Early return for invalid data
    if (!user) {
        return "User is required";
    }
    if (!user.name) {
        return "Name is required";
    }
    if (!user.email) {
        return "Email is required";
    }
    
    // If we get here, user is valid
    return "User is valid";
}

console.log(validateUser(null));                           // "User is required"
console.log(validateUser({name: "John"}));                 // "Email is required"
console.log(validateUser({name: "John", email: "john@ex"})); // "User is valid"
```

#### Returning Different Types

```javascript
function getValue(type) {
    switch (type) {
        case "number":
            return 42;
        case "string":
            return "Hello";
        case "boolean":
            return true;
        case "array":
            return [1, 2, 3];
        case "object":
            return { key: "value" };
        default:
            return null;
    }
}

console.log(getValue("number"));  // 42
console.log(getValue("string"));  // "Hello"
console.log(getValue("array"));   // [1, 2, 3]
```

#### No Return Statement

```javascript
function noReturn() {
    console.log("This function doesn't return anything");
}

let result = noReturn();
console.log(result);  // undefined
```

#### Returning Functions

```javascript
function createGreeter(greeting) {
    return function(name) {
        return `${greeting}, ${name}!`;
    };
}

const sayHello = createGreeter("Hello");
const sayGoodbye = createGreeter("Goodbye");

console.log(sayHello("John"));   // "Hello, John!"
console.log(sayGoodbye("John")); // "Goodbye, John!"
```

### Default Parameters

Default parameters allow you to initialize function parameters with default values if no argument is provided.

#### Basic Default Parameters

```javascript
function greet(name = "Guest") {
    console.log(`Hello, ${name}!`);
}

greet("John");   // "Hello, John!"
greet();         // "Hello, Guest!"
```

#### Multiple Default Parameters

```javascript
function createUser(name = "Anonymous", age = 0, country = "Unknown") {
    return { name, age, country };
}

console.log(createUser("John", 30, "USA"));  // {name: "John", age: 30, country: "USA"}
console.log(createUser("John", 30));         // {name: "John", age: 30, country: "Unknown"}
console.log(createUser("John"));            // {name: "John", age: 0, country: "Unknown"}
console.log(createUser());                   // {name: "Anonymous", age: 0, country: "Unknown"}
```

#### Default Parameters with Expressions

```javascript
function calculatePrice(price, tax = 0.1, discount = 0) {
    return price * (1 + tax) * (1 - discount);
}

console.log(calculatePrice(100));              // 110 (default tax 10%)
console.log(calculatePrice(100, 0.2));         // 120 (20% tax)
console.log(calculatePrice(100, 0.1, 0.1));    // 99 (10% tax, 10% discount)
```

#### Default Parameters with Previous Parameters

```javascript
function createUser(name, age = name === "Admin" ? 30 : 18) {
    return { name, age };
}

console.log(createUser("Admin"));   // {name: "Admin", age: 30}
console.log(createUser("John"));    // {name: "John", age: 18}
```

#### Default Parameters and undefined

```javascript
function test(value = "default") {
    console.log(value);
}

test();           // "default"
test(undefined);  // "default"
test(null);       // null (null is a value, not undefined)
test("");         // "" (empty string is a value)
```

### Rest Parameters

Rest parameters allow you to represent an indefinite number of arguments as an array.

#### Basic Rest Parameters

```javascript
function sumAll(...numbers) {
    let sum = 0;
    for (let num of numbers) {
        sum += num;
    }
    return sum;
}

console.log(sumAll(1, 2, 3));           // 6
console.log(sumAll(1, 2, 3, 4, 5));      // 15
console.log(sumAll());                   // 0
```

#### Rest Parameters with Regular Parameters

```javascript
function greetAll(greeting, ...names) {
    names.forEach(name => {
        console.log(`${greeting}, ${name}!`);
    });
}

greetAll("Hello", "John", "Jane", "Bob");
// "Hello, John!"
// "Hello, Jane!"
// "Hello, Bob!"
```

#### Rest Parameters and Destructuring

```javascript
function processUser({ name, ...details }) {
    console.log("Name:", name);
    console.log("Details:", details);
}

processUser({
    name: "John",
    age: 30,
    email: "john@example.com",
    city: "New York"
});
// Name: John
// Details: {age: 30, email: "john@example.com", city: "New York"}
```

#### Rest Parameters in Array Methods

```javascript
function mergeArrays(...arrays) {
    return arrays.flat();
}

console.log(mergeArrays([1, 2], [3, 4], [5, 6]));  // [1, 2, 3, 4, 5, 6]
```

#### Rest Parameters vs Arguments Object

```javascript
// Old way (arguments object)
function oldSum() {
    let sum = 0;
    for (let i = 0; i < arguments.length; i++) {
        sum += arguments[i];
    }
    return sum;
}

// New way (rest parameters)
function newSum(...numbers) {
    return numbers.reduce((sum, num) => sum + num, 0);
}

console.log(oldSum(1, 2, 3, 4, 5));  // 15
console.log(newSum(1, 2, 3, 4, 5));  // 15
```

---

## 💻 Practical (1.5h)

### Exercise 1: Basic Functions

```javascript
// Exercise 1.1: Simple function
console.log("=== Simple Function ===");

function sayHello(name) {
    return `Hello, ${name}!`;
}

console.log(sayHello("John"));
console.log(sayHello("Jane"));

// Exercise 1.2: Function with multiple parameters
console.log("\n=== Multiple Parameters ===");

function add(a, b) {
    return a + b;
}

function subtract(a, b) {
    return a - b;
}

function multiply(a, b) {
    return a * b;
}

function divide(a, b) {
    if (b === 0) return "Cannot divide by zero";
    return a / b;
}

console.log("5 + 3 =", add(5, 3));
console.log("10 - 4 =", subtract(10, 4));
console.log("6 * 7 =", multiply(6, 7));
console.log("15 / 3 =", divide(15, 3));
console.log("10 / 0 =", divide(10, 0));

// Exercise 1.3: Function without return
console.log("\n=== Function Without Return ===");

function logInfo(name, age) {
    console.log(`Name: ${name}, Age: ${age}`);
}

logInfo("John", 30);
logInfo("Jane", 25);
```

### Exercise 2: Advanced Function Examples

```javascript
// Exercise 2.1: Grade calculator
console.log("=== Grade Calculator ===");

function calculateGrade(score) {
    if (score < 0 || score > 100) {
        return "Invalid score";
    }
    
    if (score >= 90) return "A";
    if (score >= 80) return "B";
    if (score >= 70) return "C";
    if (score >= 60) return "D";
    return "F";
}

console.log("Score 95:", calculateGrade(95));
console.log("Score 85:", calculateGrade(85));
console.log("Score 75:", calculateGrade(75));
console.log("Score 55:", calculateGrade(55));
console.log("Score 105:", calculateGrade(105));

// Exercise 2.2: Array statistics
console.log("\n=== Array Statistics ===");

function getArrayStats(numbers) {
    if (numbers.length === 0) {
        return { count: 0, sum: 0, average: 0, min: null, max: null };
    }
    
    let sum = 0;
    let min = numbers[0];
    let max = numbers[0];
    
    for (let num of numbers) {
        sum += num;
        if (num < min) min = num;
        if (num > max) max = num;
    }
    
    return {
        count: numbers.length,
        sum: sum,
        average: sum / numbers.length,
        min: min,
        max: max
    };
}

let stats = getArrayStats([10, 20, 30, 40, 50]);
console.log("Stats:", stats);

// Exercise 2.3: String manipulation
console.log("\n=== String Manipulation ===");

function createEmail(firstName, lastName, domain) {
    const email = `${firstName.toLowerCase()}.${lastName.toLowerCase()}@${domain}`;
    return email;
}

console.log(createEmail("John", "Doe", "example.com"));
console.log(createEmail("Jane", "Smith", "gmail.com"));

// Exercise 2.4: Temperature conversion
console.log("\n=== Temperature Conversion ===");

function celsiusToFahrenheit(celsius) {
    return (celsius * 9/5) + 32;
}

function fahrenheitToCelsius(fahrenheit) {
    return (fahrenheit - 32) * 5/9;
}

console.log("0°C to F:", celsiusToFahrenheit(0));
console.log("32°F to C:", fahrenheitToCelsius(32));
console.log("100°C to F:", celsiusToFahrenheit(100));
```

### Exercise 3: Return Statement

```javascript
// Exercise 3.1: Multiple returns
console.log("=== Multiple Returns ===");

function getAgeGroup(age) {
    if (age < 13) return "Child";
    if (age < 20) return "Teenager";
    if (age < 65) return "Adult";
    return "Senior";
}

console.log("Age 10:", getAgeGroup(10));
console.log("Age 15:", getAgeGroup(15));
console.log("Age 30:", getAgeGroup(30));
console.log("Age 70:", getAgeGroup(70));

// Exercise 3.2: Early return
console.log("\n=== Early Return ===");

function validateEmail(email) {
    if (!email) return "Email is required";
    if (!email.includes("@")) return "Email must contain @";
    if (!email.includes(".")) return "Email must contain .";
    return "Valid email";
}

console.log(validateEmail(""));
console.log(validateEmail("invalid"));
console.log(validateEmail("invalid@com"));
console.log(validateEmail("valid@example.com"));

// Exercise 3.3: Returning objects
console.log("\n=== Returning Objects ===");

function createRectangle(width, height) {
    return {
        width: width,
        height: height,
        area: width * height,
        perimeter: 2 * (width + height)
    };
}

let rectangle = createRectangle(5, 3);
console.log("Rectangle:", rectangle);

// Exercise 3.4: Returning functions
console.log("\n=== Returning Functions ===");

function createMultiplier(multiplier) {
    return function(number) {
        return number * multiplier;
    };
}

const double = createMultiplier(2);
const triple = createMultiplier(3);

console.log("Double 5:", double(5));
console.log("Triple 5:", triple(5));
```

### Exercise 4: Default Parameters

```javascript
// Exercise 4.1: Basic default parameters
console.log("=== Basic Default Parameters ===");

function greet(name = "Guest", time = "Day") {
    return `Good ${time}, ${name}!`;
}

console.log(greet("John", "Morning"));
console.log(greet("John"));
console.log(greet());

// Exercise 4.2: Multiple default parameters
console.log("\n=== Multiple Default Parameters ===");

function configureServer(host = "localhost", port = 3000, ssl = false) {
    return {
        host: host,
        port: port,
        ssl: ssl,
        url: `${ssl ? "https" : "http"}://${host}:${port}`
    };
}

console.log(configureServer());
console.log(configureServer("example.com"));
console.log(configureServer("example.com", 8080));
console.log(configureServer("example.com", 8080, true));

// Exercise 4.3: Default with expressions
console.log("\n=== Default with Expressions ===");

function calculateTotal(price, quantity = 1, tax = 0.1) {
    const subtotal = price * quantity;
    const taxAmount = subtotal * tax;
    return subtotal + taxAmount;
}

console.log(calculateTotal(100));
console.log(calculateTotal(100, 2));
console.log(calculateTotal(100, 2, 0.2));

// Exercise 4.4: Default parameters validation
console.log("\n=== Default Parameters Validation ===");

function createUser(name = "Anonymous", age = 18, isAdmin = false) {
    if (age < 0) age = 0;
    if (age > 120) age = 120;
    
    return {
        name: name,
        age: age,
        isAdmin: isAdmin,
        canVote: age >= 18
    };
}

console.log(createUser());
console.log(createUser("John", 25, true));
console.log(createUser("Jane", -5));
console.log(createUser("Bob", 150));
```

### Exercise 5: Rest Parameters

```javascript
// Exercise 5.1: Basic rest parameters
console.log("=== Basic Rest Parameters ===");

function sumAll(...numbers) {
    return numbers.reduce((sum, num) => sum + num, 0);
}

console.log("Sum 1,2,3:", sumAll(1, 2, 3));
console.log("Sum 1,2,3,4,5:", sumAll(1, 2, 3, 4, 5));
console.log("Sum nothing:", sumAll());

// Exercise 5.2: Rest with regular parameters
console.log("\n=== Rest with Regular Parameters ===");

function logMessages(level, ...messages) {
    console.log(`[${level.toUpperCase()}]`);
    messages.forEach(msg => console.log(`  - ${msg}`));
}

logMessages("info", "Server started", "Listening on port 3000");
logMessages("error", "Connection failed", "Timeout");

// Exercise 5.3: Rest parameters manipulation
console.log("\n=== Rest Parameters Manipulation ===");

function filterNumbers(...values) {
    return values.filter(value => typeof value === 'number');
}

console.log(filterNumbers(1, "hello", 2, true, 3, null));
console.log(filterNumbers("a", "b", "c"));

// Exercise 5.4: Rest parameters with array methods
console.log("\n=== Rest with Array Methods ===");

function findMax(...numbers) {
    if (numbers.length === 0) return undefined;
    return Math.max(...numbers);
}

function findMin(...numbers) {
    if (numbers.length === 0) return undefined;
    return Math.min(...numbers);
}

console.log("Max of 1,5,3:", findMax(1, 5, 3));
console.log("Min of 1,5,3:", findMin(1, 5, 3));
console.log("Max of nothing:", findMax());
```

### Exercise 6: Function Ultimate Practice

```javascript
// Exercise 6.1: Calculator function
console.log("=== Calculator Function ===");

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

console.log("Add 1,2,3:", calculator("add", 1, 2, 3));
console.log("Multiply 2,3,4:", calculator("multiply", 2, 3, 4));
console.log("Divide 100,2,5:", calculator("divide", 100, 2, 5));

// Exercise 6.2: String builder
console.log("\n=== String Builder ===");

function buildString(...parts) {
    return parts.join(" ");
}

console.log(buildString("Hello", "World", "from", "JavaScript"));

function buildHTML(tag, ...content) {
    return `<${tag}>${content.join("")}</${tag}>`;
}

console.log(buildHTML("p", "Hello", " ", "World"));
console.log(buildHTML("div", buildHTML("p", "Content")));

// Exercise 6.3: Array processor
console.log("\n=== Array Processor ===");

function processArray(numbers, operation) {
    switch (operation) {
        case "double":
            return numbers.map(n => n * 2);
        case "square":
            return numbers.map(n => n * n);
        case "even":
            return numbers.filter(n => n % 2 === 0);
        case "odd":
            return numbers.filter(n => n % 2 !== 0);
        case "sum":
            return numbers.reduce((sum, n) => sum + n, 0);
        default:
            return "Invalid operation";
    }
}

console.log("Double:", processArray([1, 2, 3], "double"));
console.log("Square:", processArray([1, 2, 3], "square"));
console.log("Even:", processArray([1, 2, 3, 4, 5], "even"));
console.log("Sum:", processArray([1, 2, 3, 4, 5], "sum"));

// Exercise 6.4: Validator function
console.log("\n=== Validator Function ===");

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
        errors: errors
    };
}

console.log(validateUser({ name: "John", email: "john@example.com", age: 30 }));
console.log(validateUser({ name: "", email: "invalid", age: -5 }));
```

### Exercise 7: Random Arguments Challenge

```javascript
// Exercise 7.1: Handle random number of arguments
console.log("=== Random Arguments Challenge ===");

function processRandomArguments(...args) {
    const result = {
        count: args.length,
        numbers: [],
        strings: [],
        booleans: [],
        others: []
    };
    
    for (let arg of args) {
        if (typeof arg === 'number') {
            result.numbers.push(arg);
        } else if (typeof arg === 'string') {
            result.strings.push(arg);
        } else if (typeof arg === 'boolean') {
            result.booleans.push(arg);
        } else {
            result.others.push(arg);
        }
    }
    
    return result;
}

console.log(processRandomArguments(1, "hello", true, 2, "world", false, null, {key: "value"}));

// Exercise 7.2: Flexible calculator
console.log("\n=== Flexible Calculator ===");

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

// Exercise 7.3: Dynamic function caller
console.log("\n=== Dynamic Function Caller ===");

function callFunction(funcName, ...args) {
    const functions = {
        add: (...nums) => nums.reduce((sum, n) => sum + n, 0),
        multiply: (...nums) => nums.reduce((prod, n) => prod * n, 1),
        max: (...nums) => Math.max(...nums),
        min: (...nums) => Math.min(...nums),
        average: (...nums) => nums.reduce((sum, n) => sum + n, 0) / nums.length
    };
    
    if (functions[funcName]) {
        return functions[funcName](...args);
    }
    
    return "Function not found";
}

console.log(callFunction("add", 1, 2, 3));
console.log(callFunction("max", 1, 5, 3, 2));
console.log(callFunction("average", 10, 20, 30));
```

### Exercise 8: Anonymous Functions

```javascript
// Exercise 8.1: Basic anonymous function
console.log("=== Anonymous Functions ===");

// Anonymous function assigned to variable
const greet = function(name) {
    return `Hello, ${name}!`;
};

console.log(greet("John"));

// Anonymous function as callback
const numbers = [1, 2, 3, 4, 5];
const doubled = numbers.map(function(num) {
    return num * 2;
});
console.log("Doubled:", doubled);

// Exercise 8.2: Anonymous functions in arrays
console.log("\n=== Anonymous Functions in Arrays ===");

const operations = [
    function(a, b) { return a + b; },
    function(a, b) { return a - b; },
    function(a, b) { return a * b; },
    function(a, b) { return a / b; }
];

console.log("Add:", operations[0](5, 3));
console.log("Subtract:", operations[1](5, 3));
console.log("Multiply:", operations[2](5, 3));
console.log("Divide:", operations[3](6, 3));

// Exercise 8.3: Immediately Invoked Function Expression (IIFE)
console.log("\n=== IIFE ===");

(function() {
    console.log("This function runs immediately!");
})();

(function(name) {
    console.log(`Hello, ${name}!`);
})("John");

// IIFE with return value
const result = (function(a, b) {
    return a + b;
})(5, 3);
console.log("IIFE result:", result);

// Exercise 8.4: Anonymous functions as object methods
console.log("\n=== Anonymous Functions as Methods ===");

const calculator = {
    add: function(a, b) {
        return a + b;
    },
    subtract: function(a, b) {
        return a - b;
    },
    multiply: function(a, b) {
        return a * b;
    },
    divide: function(a, b) {
        return a / b;
    }
};

console.log("Calculator add:", calculator.add(5, 3));
console.log("Calculator multiply:", calculator.multiply(5, 3));
```

### Exercise 9: Returning Nested Functions

```javascript
// Exercise 9.1: Basic nested function
console.log("=== Nested Functions ===");

function outerFunction() {
    function innerFunction() {
        return "Hello from inner function!";
    }
    
    return innerFunction;
}

const inner = outerFunction();
console.log(inner());

// Exercise 9.2: Function factory
console.log("\n=== Function Factory ===");

function createGreeter(greeting) {
    return function(name) {
        return `${greeting}, ${name}!`;
    };
}

const sayHello = createGreeter("Hello");
const sayHi = createGreeter("Hi");
const sayGoodbye = createGreeter("Goodbye");

console.log(sayHello("John"));
console.log(sayHi("Jane"));
console.log(sayGoodbye("Bob"));

// Exercise 9.3: Counter with closure
console.log("\n=== Counter with Closure ===");

function createCounter() {
    let count = 0;
    
    return {
        increment: function() {
            count++;
            return count;
        },
        decrement: function() {
            count--;
            return count;
        },
        getCount: function() {
            return count;
        }
    };
}

const counter1 = createCounter();
const counter2 = createCounter();

console.log("Counter 1:", counter1.increment());
console.log("Counter 1:", counter1.increment());
console.log("Counter 2:", counter2.increment());
console.log("Counter 1:", counter1.getCount());
console.log("Counter 2:", counter2.getCount());

// Exercise 9.4: Function with private data
console.log("\n=== Function with Private Data ===");

function createBankAccount(initialBalance) {
    let balance = initialBalance;
    
    return {
        deposit: function(amount) {
            if (amount > 0) {
                balance += amount;
                return `Deposited $${amount}. New balance: $${balance}`;
            }
            return "Invalid deposit amount";
        },
        withdraw: function(amount) {
            if (amount > 0 && amount <= balance) {
                balance -= amount;
                return `Withdrew $${amount}. New balance: $${balance}`;
            }
            return "Invalid withdrawal amount";
        },
        getBalance: function() {
            return balance;
        }
    };
}

const account = createBankAccount(100);
console.log(account.deposit(50));
console.log(account.withdraw(30));
console.log(account.getBalance());
```

### Exercise 10: Complete Working Example

**Complete script.js:**
```javascript
// Session 7: Functions (Part 1)
// This script demonstrates function basics, parameters, and returns

console.log("=== Session 7: Functions (Part 1) ===");

// 1. Basic function
console.log("\n--- Basic Function ---");
function greet(name) {
    return `Hello, ${name}!`;
}
console.log(greet("John"));

// 2. Function with parameters
console.log("\n--- Function with Parameters ---");
function add(a, b) {
    return a + b;
}
console.log("5 + 3 =", add(5, 3));

// 3. Default parameters
console.log("\n--- Default Parameters ---");
function sayHello(name = "Guest") {
    return `Hello, ${name}!`;
}
console.log(sayHello());
console.log(sayHello("Jane"));

// 4. Rest parameters
console.log("\n--- Rest Parameters ---");
function sumAll(...numbers) {
    return numbers.reduce((sum, num) => sum + num, 0);
}
console.log("Sum 1,2,3:", sumAll(1, 2, 3));
console.log("Sum 1,2,3,4,5:", sumAll(1, 2, 3, 4, 5));

// 5. Return statement
console.log("\n--- Return Statement ---");
function getGrade(score) {
    if (score >= 90) return "A";
    if (score >= 80) return "B";
    if (score >= 70) return "C";
    return "F";
}
console.log("Grade for 85:", getGrade(85));

// 6. Anonymous function
console.log("\n--- Anonymous Function ---");
const multiply = function(a, b) {
    return a * b;
};
console.log("5 * 3 =", multiply(5, 3));

// 7. Nested function
console.log("\n--- Nested Function ---");
function createGreeter(greeting) {
    return function(name) {
        return `${greeting}, ${name}!`;
    };
}
const sayHi = createGreeter("Hi");
console.log(sayHi("John"));

// 8. Function with object return
console.log("\n--- Object Return ---");
function createUser(name, age) {
    return {
        name: name,
        age: age,
        createdAt: new Date()
    };
}
console.log(createUser("John", 30));

console.log("\n=== Session 7 Complete ===");
```

**Complete index.html:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Session 7 - Functions (Part 1)</title>
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
        .counter-demo {
            display: flex;
            gap: 20px;
            margin-top: 15px;
        }
        .counter {
            background: white;
            padding: 15px;
            border-radius: 4px;
            border: 1px solid #e8e8e8;
            flex: 1;
        }
        .counter-display {
            font-size: 2em;
            text-align: center;
            margin: 10px 0;
        }
        .counter-buttons {
            display: flex;
            gap: 10px;
            justify-content: center;
        }
    </style>
</head>
<body>
    <h1>Session 7: Functions (Part 1)</h1>
    
    <div class="section">
        <h2>Topics Covered</h2>
        <ul>
            <li>Function introduction & basic usage</li>
            <li>Advanced function examples</li>
            <li>Return statement</li>
            <li>Default parameters</li>
            <li>Rest parameters</li>
            <li>Anonymous functions</li>
            <li>Nested functions & closures</li>
        </ul>
    </div>

    <div class="section">
        <h2>Basic Function Demo</h2>
        <div class="demo">
            <input type="text" id="funcName" placeholder="Enter name">
            <button onclick="demoGreet()">Greet</button>
            <button onclick="demoAdd()">Add Numbers</button>
            <button onclick="demoGrade()">Get Grade</button>
            <div id="basicOutput" class="output">
                Click a button to see function demo...
            </div>
        </div>
    </div>

    <div class="section">
        <h2>Default Parameters Demo</h2>
        <div class="demo">
            <input type="text" id="defaultName" placeholder="Name (optional)">
            <input type="number" id="defaultAge" placeholder="Age (optional)">
            <button onclick="demoDefaultParams()">Create User</button>
            <div id="defaultOutput" class="output">
                Click to see default parameters...
            </div>
        </div>
    </div>

    <div class="section">
        <h2>Rest Parameters Demo</h2>
        <div class="demo">
            <input type="text" id="restNumbers" placeholder="Enter numbers (comma separated)">
            <button onclick="demoRestSum()">Sum All</button>
            <button onclick="demoRestMax()">Find Max</button>
            <button onclick="demoRestFilter()">Filter Numbers</button>
            <div id="restOutput" class="output">
                Click to see rest parameters...
            </div>
        </div>
    </div>

    <div class="section">
        <h2>Closure Counter Demo</h2>
        <div class="counter-demo">
            <div class="counter">
                <h3>Counter 1</h3>
                <div class="counter-display" id="counter1Display">0</div>
                <div class="counter-buttons">
                    <button onclick="counter1.increment()">+</button>
                    <button onclick="counter1.decrement()">-</button>
                    <button onclick="updateCounter1Display()">Show</button>
                </div>
            </div>
            <div class="counter">
                <h3>Counter 2</h3>
                <div class="counter-display" id="counter2Display">0</div>
                <div class="counter-buttons">
                    <button onclick="counter2.increment()">+</button>
                    <button onclick="counter2.decrement()">-</button>
                    <button onclick="updateCounter2Display()">Show</button>
                </div>
            </div>
        </div>
    </div>

    <div class="section">
        <h2>Console Output</h2>
        <p>Open the browser console (F12) to see all JavaScript examples.</p>
    </div>

    <script src="script.js" defer></script>
    <script>
        // Basic function demos
        function demoGreet() {
            const name = document.getElementById('funcName').value || 'Guest';
            function greet(name) {
                return `Hello, ${name}!`;
            }
            document.getElementById('basicOutput').textContent = greet(name);
        }

        function demoAdd() {
            function add(a, b) {
                return a + b;
            }
            const result = add(Math.floor(Math.random() * 10), Math.floor(Math.random() * 10));
            document.getElementById('basicOutput').textContent = `Random addition: ${result}`;
        }

        function demoGrade() {
            function getGrade(score) {
                if (score >= 90) return "A";
                if (score >= 80) return "B";
                if (score >= 70) return "C";
                if (score >= 60) return "D";
                return "F";
            }
            const score = Math.floor(Math.random() * 40) + 60; // 60-100
            document.getElementById('basicOutput').textContent = `Score ${score}: Grade ${getGrade(score)}`;
        }

        // Default parameters demo
        function demoDefaultParams() {
            function createUser(name = "Anonymous", age = 18, country = "Unknown") {
                return { name, age, country };
            }
            
            const name = document.getElementById('defaultName').value;
            const age = parseInt(document.getElementById('defaultAge').value);
            
            const user = createUser(
                name || undefined,
                isNaN(age) ? undefined : age
            );
            
            document.getElementById('defaultOutput').textContent = JSON.stringify(user, null, 2);
        }

        // Rest parameters demo
        function demoRestSum() {
            function sumAll(...numbers) {
                return numbers.reduce((sum, num) => sum + num, 0);
            }
            
            const input = document.getElementById('restNumbers').value;
            const numbers = input.split(',').map(n => parseFloat(n.trim())).filter(n => !isNaN(n));
            
            if (numbers.length === 0) {
                document.getElementById('restOutput').textContent = "Please enter valid numbers";
                return;
            }
            
            document.getElementById('restOutput').textContent = `Numbers: ${numbers.join(', ')}\nSum: ${sumAll(...numbers)}`;
        }

        function demoRestMax() {
            function findMax(...numbers) {
                return Math.max(...numbers);
            }
            
            const input = document.getElementById('restNumbers').value;
            const numbers = input.split(',').map(n => parseFloat(n.trim())).filter(n => !isNaN(n));
            
            if (numbers.length === 0) {
                document.getElementById('restOutput').textContent = "Please enter valid numbers";
                return;
            }
            
            document.getElementById('restOutput').textContent = `Numbers: ${numbers.join(', ')}\nMax: ${findMax(...numbers)}`;
        }

        function demoRestFilter() {
            function filterNumbers(...values) {
                return values.filter(value => typeof value === 'number');
            }
            
            const input = document.getElementById('restNumbers').value;
            const values = input.split(',').map(v => {
                const trimmed = v.trim();
                const num = parseFloat(trimmed);
                return isNaN(num) ? trimmed : num;
            });
            
            const numbers = filterNumbers(...values);
            document.getElementById('restOutput').textContent = `Input: ${values.join(', ')}\nNumbers only: ${numbers.join(', ')}`;
        }

        // Closure counter
        function createCounter() {
            let count = 0;
            
            return {
                increment: function() {
                    count++;
                },
                decrement: function() {
                    count--;
                },
                getCount: function() {
                    return count;
                }
            };
        }

        const counter1 = createCounter();
        const counter2 = createCounter();

        function updateCounter1Display() {
            document.getElementById('counter1Display').textContent = counter1.getCount();
        }

        function updateCounter2Display() {
            document.getElementById('counter2Display').textContent = counter2.getCount();
        }
    </script>
</body>
</html>
```

---

## 📝 Review (0.5h)

### Arrow Function Syntax Introduction

Arrow functions provide a concise syntax for writing functions. They are always anonymous and have a different `this` behavior.

#### Basic Arrow Function

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

#### Arrow Function with Single Parameter

```javascript
// Traditional
function square(num) {
    return num * num;
}

// Arrow function
const square = num => num * num;
```

#### Arrow Function with No Parameters

```javascript
// Traditional
function sayHello() {
    console.log("Hello!");
}

// Arrow function
const sayHello = () => {
    console.log("Hello!");
};

// Concise (if single expression)
const sayHello = () => console.log("Hello!");
```

#### Arrow Function with Object Return

```javascript
// Traditional
function createUser(name, age) {
    return { name: name, age: age };
}

// Arrow function (need parentheses for object)
const createUser = (name, age) => ({ name, age });
```

#### Arrow Functions as Callbacks

```javascript
const numbers = [1, 2, 3, 4, 5];

// Traditional callback
const doubled = numbers.map(function(num) {
    return num * 2;
});

// Arrow callback
const doubled = numbers.map(num => num * 2);
```

### Q&A

**Q1: What is the difference between function declaration and function expression?**
A: Function declarations are hoisted (can be called before declaration), while function expressions are not. Function expressions can be anonymous and assigned to variables.

**Q2: What are default parameters and when should I use them?**
A: Default parameters provide default values for function parameters when no argument is passed. Use them when you want to make parameters optional or provide sensible defaults.

**Q3: What is the difference between rest parameters and the arguments object?**
A: Rest parameters are a modern ES6 feature that provide a true array, while the arguments object is an array-like object. Rest parameters are more readable and work better with array methods.

**Q4: What is a closure?**
A: A closure is a function that has access to variables from its outer (enclosing) scope, even after the outer function has returned. This allows for data privacy and function factories.

**Q5: When should I use arrow functions vs regular functions?**
A: Use arrow functions for short callbacks and when you want to preserve the `this` context from the surrounding scope. Use regular functions when you need a named function, function hoisting, or a dynamic `this` context.

**Q6: What happens if I don't provide a return statement?**
A: The function returns `undefined` by default. This is important to remember when you expect a function to return a value.

**Q7: Can I have multiple rest parameters in a function?**
A: No, you can only have one rest parameter, and it must be the last parameter in the function signature.

**Q8: What is the difference between `return` and `console.log`?**
A: `return` sends a value back to the caller of the function, while `console.log` prints a value to the console. Functions should `return` values for further processing, not just log them.

**Q9: How do I handle errors in functions?**
A: You can use `try-catch` blocks within functions, return error objects/strings, or throw exceptions that can be caught by the caller.

**Q10: What is an IIFE (Immediately Invoked Function Expression)?**
A: An IIFE is a function that runs immediately after it's defined. It's commonly used to create private scopes and avoid polluting the global namespace.

### Review Questions

1. **What is the correct syntax for a function declaration?**
   - [ ] function add(a, b) { return a + b; }
   - [ ] const add = function(a, b) { return a + b; }
   - [ ] const add = (a, b) => a + b;
   - [ ] All of the above

2. **What does a function return if no return statement is provided?**
   - [ ] null
   - [ ] undefined
   - [ ] 0
   - [ ] Error

3. **How do you set a default parameter value?**
   - [ ] function name = "Guest"
   - [ ] function name(default = "Guest")
   - [ ] function name = "Guest" {}
   - [ ] function name("Guest")

4. **What symbol is used for rest parameters?**
   - [ ] *
   - [ ] ...
   - [ ] &
   - [ ] #

5. **What is a closure?**
   - [ ] A way to close functions
   - [ ] A function with access to outer scope variables
   - [ ] A type of loop
   - [ ] A method to end execution

6. **Which arrow function syntax is correct for a single parameter?**
   - [ ] (num) => num * 2
   - [ ] num => num * 2
   - [ ] => num * 2
   - [ ] num -> num * 2

7. **Can you have multiple rest parameters in one function?**
   - [ ] Yes
   - [ ] No
   - [ ] Only if they're the same type
   - [ ] Only in arrow functions

8. **What is an IIFE?**
   - [ ] A function that returns immediately
   - [ ] A function that runs immediately after definition
   - [ ] A function inside another function
   - [ ] A function with no parameters

9. **How do you return an object in a concise arrow function?**
   - [ ] => { key: value }
   - [ ] => ({ key: value })
   - [ ] => key: value
   - [ ] => return { key: value }

10. **What is the main advantage of arrow functions?**
    - [ ] They're always faster
    - [ ] They have a shorter syntax and lexical `this`
    - [ ] They can be hoisted
    - [ ] They support more features

### Correct Answers

1. ✅ function add(a, b) { return a + b; }
2. ✅ undefined
3. ✅ function name = "Guest"
4. ✅ ...
5. ✅ A function with access to outer scope variables
6. ✅ num => num * 2
7. ✅ No
8. ✅ A function that runs immediately after definition
9. ✅ => ({ key: value })
10. ✅ They have a shorter syntax and lexical `this`

---

## 🎯 Next Steps

1. ✅ Practice function declarations and expressions
2. ✅ Master default parameters and rest parameters
3. ✅ Understand closures and their use cases
4. ✅ Practice arrow function syntax
5. ✅ Build applications using functions
6. ✅ Learn about function scope and hoisting
7. ✅ Explore recursion and advanced patterns

---

## 📚 Additional Resources

- [MDN: Functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Functions)
- [MDN: Default Parameters](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Default_parameters)
- [MDN: Rest Parameters](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/rest_parameters)
- [MDN: Arrow Functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions)
- [JavaScript.info: Functions](https://javascript.info/function-basics)
- [JavaScript.info: Arrow Functions](https://javascript.info/arrow-functions-basics)

**Remember:** Functions are the building blocks of JavaScript applications. Master them to write clean, reusable, and maintainable code! 💪
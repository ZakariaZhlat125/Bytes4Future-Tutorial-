# Session 13: Regex, OOP, Date, Generators & Modules

**⚠️ Note:** This session is compressed and covers multiple advanced topics. Regex quantifiers and Generators are shown conceptually—practice these separately using tools like [regex101.com](https://regex101.com).

---

## 📚 Theory (1h)

### Regex Basics (Condensed Overview)

Regular expressions (regex) are patterns used to match character combinations in strings.

#### Basic Syntax

```javascript
// Creating regex
const pattern1 = /hello/;              // Literal notation
const pattern2 = new RegExp("hello");  // Constructor

// Testing
const text = "hello world";
console.log(/hello/.test(text));  // true
console.log(text.match(/hello/)); // ["hello"]
```

#### Modifiers (Flags)

```javascript
// i - case insensitive
/hello/i.test("HELLO");  // true

// g - global (find all matches)
/hello/g.test("hello hello");  // true

// m - multiline
/^hello/m.test("hello\nworld");  // true

// Combined
const pattern = /hello/gi;
```

#### Character Classes

```javascript
// Character set
/[abc]/.test("a");  // true (matches a, b, or c)

// Range
/[a-z]/.test("a");  // true (any lowercase letter)
/[0-9]/.test("5");  // true (any digit)

// Negated set
/[^abc]/.test("d");  // true (not a, b, or c)

// Shorthand classes
/\d/.test("5");     // digit
/\w/.test("a");     // word character
/\s/.test(" ");    // whitespace
```

#### Quantifiers (Condensed)

```javascript
// Common quantifiers
/a*/.test("aaa");   // 0 or more
/a+/.test("aaa");   // 1 or more
/a?/.test("a");     // 0 or 1
/a{3}/.test("aaa"); // exactly 3
/a{2,4}/.test("aaa"); // 2 to 4
```

---

## 💻 Practical (1.5h)

### Exercise 1: OOP - Constructor Functions

```javascript
// Exercise 1.1: Basic constructor function
console.log("=== Constructor Functions ===");

function Person(name, age) {
    this.name = name;
    this.age = age;
    
    this.greet = function() {
        return `Hello, I'm ${this.name}`;
    };
}

const person1 = new Person("John", 30);
const person2 = new Person("Jane", 25);

console.log(person1.greet()); // "Hello, I'm John"
console.log(person2.greet()); // "Hello, I'm Jane"

// Exercise 1.2: this keyword in constructors
console.log("\n=== this in Constructors ===");

function Car(make, model) {
    this.make = make;
    this.model = model;
    this.speed = 0;
    
    this.accelerate = function(amount) {
        this.speed += amount;
        return `${this.make} ${this.model} speed: ${this.speed}`;
    };
    
    this.brake = function(amount) {
        this.speed = Math.max(0, this.speed - amount);
        return `${this.make} ${this.model} speed: ${this.speed}`;
    };
}

const car = new Car("Toyota", "Camry");
console.log(car.accelerate(50)); // "Toyota Camry speed: 50"
console.log(car.brake(20));      // "Toyota Camry speed: 30"

// Exercise 1.3: Static properties
console.log("\n=== Static Properties ===");

function User(name) {
    this.name = name;
    this.id = User.nextId++;
}

User.nextId = 1; // Static property
User.userCount = 0; // Static property

User.getCount = function() { // Static method
    return User.userCount;
};

const user1 = new User("John");
const user2 = new User("Jane");

console.log("User 1 ID:", user1.id); // 1
console.log("User 2 ID:", user2.id); // 2
console.log("Total users:", User.getCount()); // 2

// Exercise 1.4: Prototype methods
console.log("\n=== Prototype Methods ===");

function Animal(name, species) {
    this.name = name;
    this.species = species;
}

Animal.prototype.speak = function() {
    return `${this.name} the ${this.species} makes a sound`;
};

Animal.prototype.eat = function(food) {
    return `${this.name} eats ${food}`;
};

const dog = new Animal("Buddy", "Dog");
const cat = new Animal("Whiskers", "Cat");

console.log(dog.speak());  // "Buddy the Dog makes a sound"
console.log(cat.eat("fish")); // "Whiskers eats fish"
```

### Exercise 2: OOP - Class Inheritance

```javascript
// Exercise 2.1: ES6 Classes
console.log("=== ES6 Classes ===");

class Person {
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }
    
    greet() {
        return `Hello, I'm ${this.name}`;
    }
    
    getAge() {
        return this.age;
    }
}

const person = new Person("John", 30);
console.log(person.greet()); // "Hello, I'm John"

// Exercise 2.2: Class inheritance
console.log("\n=== Class Inheritance ===");

class Animal {
    constructor(name) {
        this.name = name;
    }
    
    speak() {
        return `${this.name} makes a sound`;
    }
}

class Dog extends Animal {
    constructor(name, breed) {
        super(name); // Call parent constructor
        this.breed = breed;
    }
    
    speak() {
        return `${this.name} barks`;
    }
    
    fetch() {
        return `${this.name} fetches the ball`;
    }
}

const dog = new Dog("Buddy", "Golden Retriever");
console.log(dog.speak()); // "Buddy barks"
console.log(dog.fetch()); // "Buddy fetches the ball"

// Exercise 2.3: Method overriding
console.log("\n=== Method Overriding ===");

class Vehicle {
    constructor(type) {
        this.type = type;
    }
    
    move() {
        return `${this.type} is moving`;
    }
}

class Car extends Vehicle {
    constructor(type, model) {
        super(type);
        this.model = model;
    }
    
    move() {
        return `${this.model} is driving`;
    }
}

const car = new Car("Car", "Toyota");
console.log(car.move()); // "Toyota is driving"

// Exercise 2.4: Super keyword
console.log("\n=== Super Keyword ===");

class Shape {
    constructor(color) {
        this.color = color;
    }
    
    getArea() {
        return 0;
    }
    
    getInfo() {
        return `Color: ${this.color}, Area: ${this.getArea()}`;
    }
}

class Circle extends Shape {
    constructor(color, radius) {
        super(color);
        this.radius = radius;
    }
    
    getArea() {
        return Math.PI * this.radius * this.radius;
    }
    
    getInfo() {
        return super.getInfo() + `, Radius: ${this.radius}`;
    }
}

const circle = new Circle("red", 5);
console.log(circle.getInfo()); // "Color: red, Area: 78.54..., Radius: 5"
```

### Exercise 3: OOP - Encapsulation

```javascript
// Exercise 3.1: Private fields (ES2022)
console.log("=== Private Fields ===");

class BankAccount {
    #balance; // Private field
    #accountNumber;
    
    constructor(accountNumber, initialBalance) {
        this.#accountNumber = accountNumber;
        this.#balance = initialBalance;
    }
    
    deposit(amount) {
        if (amount > 0) {
            this.#balance += amount;
            return `Deposited: $${amount}. New balance: $${this.#balance}`;
        }
        return "Invalid deposit amount";
    }
    
    withdraw(amount) {
        if (amount > 0 && amount <= this.#balance) {
            this.#balance -= amount;
            return `Withdrew: $${amount}. New balance: $${this.#balance}`;
        }
        return "Invalid withdrawal amount";
    }
    
    getBalance() {
        return this.#balance;
    }
    
    getAccountNumber() {
        return this.#accountNumber;
    }
}

const account = new BankAccount("12345", 1000);
console.log(account.deposit(500));  // "Deposited: $500. New balance: $1500"
console.log(account.withdraw(200)); // "Withdrew: $200. New balance: $1300"
console.log(account.getBalance());  // 1300
// console.log(account.#balance); // SyntaxError - private field

// Exercise 3.2: Traditional encapsulation (closures)
console.log("\n=== Encapsulation with Closures ===");

function createCounter() {
    let count = 0; // Private variable
    
    return {
        increment() {
            count++;
            return count;
        },
        decrement() {
            count--;
            return count;
        },
        getCount() {
            return count;
        }
    };
}

const counter = createCounter();
console.log(counter.increment()); // 1
console.log(counter.increment()); // 2
console.log(counter.getCount());  // 2
// count is not accessible from outside

// Exercise 3.3: Getters and Setters
console.log("\n=== Getters and Setters ===");

class Temperature {
    constructor(celsius) {
        this._celsius = celsius;
    }
    
    get celsius() {
        return this._celsius;
    }
    
    set celsius(value) {
        this._celsius = value;
    }
    
    get fahrenheit() {
        return (this._celsius * 9/5) + 32;
    }
    
    set fahrenheit(value) {
        this._celsius = (value - 32) * 5/9;
    }
}

const temp = new Temperature(25);
console.log(temp.celsius);     // 25
console.log(temp.fahrenheit);  // 77

temp.fahrenheit = 100;
console.log(temp.celsius);     // 37.78

// Exercise 3.4: Protected pattern (convention)
console.log("\n=== Protected Pattern ===");

class Employee {
    constructor(name, salary) {
        this.name = name;
        this._salary = salary; // Convention: underscore for protected
    }
    
    getSalary() {
        return this._salary;
    }
    
    setSalary(newSalary) {
        if (newSalary > 0) {
            this._salary = newSalary;
        }
    }
    
    getInfo() {
        return `${this.name} earns $${this._salary}`;
    }
}

const employee = new Employee("John", 50000);
console.log(employee.getInfo()); // "John earns $50000"
// employee._salary is accessible by convention, not enforcement
```

### Exercise 4: OOP - Prototype Chain Basics

```javascript
// Exercise 4.1: Understanding prototype chain
console.log("=== Prototype Chain ===");

function Person(name) {
    this.name = name;
}

Person.prototype.greet = function() {
    return `Hello, I'm ${this.name}`;
};

const person = new Person("John");

console.log(person.greet()); // "Hello, I'm John"
console.log(person.__proto__ === Person.prototype); // true
console.log(Person.prototype.__proto__ === Object.prototype); // true
console.log(Object.prototype.__proto__ === null); // true

// Exercise 4.2: Prototype inheritance
console.log("\n=== Prototype Inheritance ===");

function Animal(name) {
    this.name = name;
}

Animal.prototype.speak = function() {
    return `${this.name} makes a sound`;
};

function Dog(name, breed) {
    Animal.call(this, name); // Call parent constructor
    this.breed = breed;
}

// Inherit prototype
Dog.prototype = Object.create(Animal.prototype);
Dog.prototype.constructor = Dog;

Dog.prototype.bark = function() {
    return `${this.name} barks`;
};

const dog = new Dog("Buddy", "Golden Retriever");
console.log(dog.speak()); // "Buddy makes a sound" (inherited)
console.log(dog.bark());  // "Buddy barks" (own method)

// Exercise 4.3: hasOwnProperty
console.log("\n=== hasOwnProperty ===");

console.log(dog.hasOwnProperty("name"));  // true
console.log(dog.hasOwnProperty("speak")); // false (from prototype)
console.log(dog.hasOwnProperty("bark"));  // true

// Exercise 4.4: Object.create() for inheritance
console.log("\n=== Object.create() ===");

const animalPrototype = {
    speak: function() {
        return `${this.name} makes a sound`;
    }
};

const dog = Object.create(animalPrototype);
dog.name = "Buddy";
dog.bark = function() {
    return `${this.name} barks`;
};

console.log(dog.speak()); // "Buddy makes a sound"
console.log(dog.bark());  // "Buddy barks"
```

### Exercise 5: Date/Time Methods (Quick Tour)

```javascript
// Exercise 5.1: Creating dates
console.log("=== Creating Dates ===");

const now = new Date();
console.log("Current date:", now);

const specificDate = new Date("2024-01-15");
console.log("Specific date:", specificDate);

const dateWithParams = new Date(2024, 0, 15, 10, 30, 0);
console.log("Date with params:", dateWithParams);

// Exercise 5.2: Getting date components
console.log("\n=== Getting Date Components ===");

const date = new Date();
console.log("FullYear:", date.getFullYear());
console.log("Month:", date.getMonth()); // 0-11
console.log("Date:", date.getDate());
console.log("Day:", date.getDay()); // 0-6 (Sunday-Saturday)
console.log("Hours:", date.getHours());
console.log("Minutes:", date.getMinutes());
console.log("Seconds:", date.getSeconds());
console.log("Milliseconds:", date.getMilliseconds());

// Exercise 5.3: Setting date components
console.log("\n=== Setting Date Components ===");

const date2 = new Date();
date2.setFullYear(2025);
date2.setMonth(5); // June
date2.setDate(15);
console.log("Modified date:", date2);

// Exercise 5.4: Date formatting
console.log("\n=== Date Formatting ===");

const date3 = new Date();
console.log("toLocaleDateString:", date3.toLocaleDateString());
console.log("toLocaleTimeString:", date3.toLocaleTimeString());
console.log("toLocaleString:", date3.toLocaleString());
console.log("toISOString:", date3.toISOString());
console.log("toString:", date3.toString());

// Exercise 5.5: Date arithmetic
console.log("\n=== Date Arithmetic ===");

const start = new Date();
const end = new Date(start.getTime() + 24 * 60 * 60 * 1000); // Add 1 day
console.log("Start:", start);
console.log("End (+1 day):", end);

const diff = end - start;
console.log("Difference in ms:", diff);
console.log("Difference in hours:", diff / (1000 * 60 * 60));
```

### Exercise 6: Generators (Concept Only)

```javascript
// Exercise 6.1: Basic generator
console.log("=== Basic Generator ===");

function* simpleGenerator() {
    yield 1;
    yield 2;
    yield 3;
}

const gen = simpleGenerator();
console.log(gen.next().value); // 1
console.log(gen.next().value); // 2
console.log(gen.next().value); // 3
console.log(gen.next().value); // undefined

// Exercise 6.2: Generator with loop
console.log("\n=== Generator with Loop ===");

function* countGenerator() {
    let count = 0;
    while (count < 5) {
        yield count;
        count++;
    }
}

const counter = countGenerator();
for (const value of counter) {
    console.log(value); // 0, 1, 2, 3, 4
}

// Exercise 6.3: Generator for range
console.log("\n=== Range Generator ===");

function* range(start, end) {
    for (let i = start; i <= end; i++) {
        yield i;
    }
}

for (const num of range(1, 5)) {
    console.log(num); // 1, 2, 3, 4, 5
}

// Exercise 6.4: Practical use case - infinite sequence
console.log("\n=== Infinite Sequence ===");

function* fibonacci() {
    let [a, b] = [0, 1];
    while (true) {
        yield a;
        [a, b] = [b, a + b];
    }
}

const fib = fibonacci();
console.log(fib.next().value); // 0
console.log(fib.next().value); // 1
console.log(fib.next().value); // 1
console.log(fib.next().value); // 2
console.log(fib.next().value); // 3
```

### Exercise 7: Module Import/Export

```javascript
// Exercise 7.1: Named exports
console.log("=== Named Exports ===");

// In math.js (export file)
/*
export const add = (a, b) => a + b;
export const subtract = (a, b) => a - b;
export const multiply = (a, b) => a * b;
export const divide = (a, b) => a / b;
*/

// In main.js (import file)
/*
import { add, subtract } from './math.js';
console.log(add(5, 3)); // 8
console.log(subtract(10, 4)); // 6
*/

// Exercise 7.2: Default export
console.log("\n=== Default Export ===");

// In user.js (export file)
/*
export default class User {
    constructor(name) {
        this.name = name;
    }
    greet() {
        return `Hello, ${this.name}`;
    }
}
*/

// In main.js (import file)
/*
import User from './user.js';
const user = new User("John");
console.log(user.greet()); // "Hello, John"
*/

// Exercise 7.3: Mixed exports
console.log("\n=== Mixed Exports ===");

// In utils.js (export file)
/*
export const PI = 3.14159;
export function calculateArea(radius) {
    return PI * radius * radius;
}
export default function calculateCircumference(radius) {
    return 2 * PI * radius;
}
*/

// In main.js (import file)
/*
import calculateCircumference, { PI, calculateArea } from './utils.js';
console.log(PI); // 3.14159
console.log(calculateArea(5)); // 78.54
console.log(calculateCircumference(5)); // 31.42
*/

// Exercise 7.4: Renaming imports
console.log("\n=== Renaming Imports ===");

/*
import { add as sum, subtract as difference } from './math.js';
console.log(sum(5, 3)); // 8
console.log(difference(10, 4)); // 6
*/

// Exercise 7.5: Import all
console.log("\n=== Import All ===");

/*
import * as math from './math.js';
console.log(math.add(5, 3)); // 8
console.log(math.subtract(10, 4)); // 6
*/
```

### Exercise 8: Complete Working Example

**Complete script.js:**
```javascript
// Session 13: Regex, OOP, Date, Generators & Modules
// This script demonstrates advanced JavaScript features

console.log("=== Session 13: Advanced Features ===");

// 1. Regex basics
console.log("\n--- Regex ---");
const text = "Hello World";
console.log("Test /hello/i:", /hello/i.test(text));
console.log("Match /World/:", text.match(/World/));

// 2. OOP - Classes
console.log("\n--- OOP Classes ---");

class Person {
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }
    
    greet() {
        return `Hello, I'm ${this.name}`;
    }
}

class Student extends Person {
    constructor(name, age, grade) {
        super(name, age);
        this.grade = grade;
    }
    
    study() {
        return `${this.name} is studying`;
    }
}

const student = new Student("John", 20, "A");
console.log(student.greet());
console.log(student.study());

// 3. Date
console.log("\n--- Date ---");
const now = new Date();
console.log("Current date:", now.toLocaleDateString());
console.log("Current time:", now.toLocaleTimeString());

// 4. Generators
console.log("\n--- Generators ---");

function* countGenerator() {
    let count = 0;
    while (count < 3) {
        yield count;
        count++;
    }
}

const counter = countGenerator();
console.log(counter.next().value);
console.log(counter.next().value);
console.log(counter.next().value);

console.log("\n=== Session 13 Complete ===");
```

**Complete index.html:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Session 13 - Advanced Features</title>
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
        .demo input, .demo button {
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
        .warning {
            background: #fff3cd;
            border: 1px solid #ffc107;
            border-radius: 4px;
            padding: 15px;
            margin-bottom: 20px;
        }
        .warning h3 {
            color: #856404;
            margin-top: 0;
        }
    </style>
</head>
<body>
    <h1>Session 13: Regex, OOP, Date, Generators & Modules</h1>
    
    <div class="warning">
        <h3>⚠️ Compressed Session</h3>
        <p>This session covers multiple advanced topics. For deep regex practice, use tools like <a href="https://regex101.com" target="_blank">regex101.com</a>. Generators are shown conceptually—practice separately.</p>
    </div>
    
    <div class="section">
        <h2>Topics Covered</h2>
        <ul>
            <li>Regex basics (modifiers, ranges, character classes, quantifiers)</li>
            <li>OOP (constructor functions, this, static, inheritance, encapsulation, prototype)</li>
            <li>Date/Time methods (quick tour)</li>
            <li>Generators (concept only)</li>
            <li>Modules (import/export, named vs default)</li>
        </ul>
    </div>

    <div class="section">
        <h2>Regex Demo</h2>
        <div class="demo">
            <input type="text" id="regexInput" placeholder="Enter text to test">
            <input type="text" id="regexPattern" placeholder="Regex pattern (e.g., /hello/i)">
            <button onclick="testRegex()">Test Regex</button>
            <div id="regexOutput" class="output">
                Enter text and pattern to test...
            </div>
        </div>
    </div>

    <div class="section">
        <h2>OOP Demo</h2>
        <div class="demo">
            <input type="text" id="personName" placeholder="Name">
            <input type="number" id="personAge" placeholder="Age">
            <button onclick="createPerson()">Create Person</button>
            <button onclick="createStudent()">Create Student</button>
            <div id="oopOutput" class="output">
                OOP results will appear here...
            </div>
        </div>
    </div>

    <div class="section">
        <h2>Date Demo</h2>
        <div class="demo">
            <button onclick="showCurrentDate()">Current Date/Time</button>
            <button onclick="dateArithmetic()">Date Arithmetic</button>
            <button onclick="dateFormatting()">Date Formatting</button>
            <div id="dateOutput" class="output">
                Click to see date operations...
            </div>
        </div>
    </div>

    <div class="section">
        <h2>Generator Demo</h2>
        <div class="demo">
            <button onclick="runGenerator()">Run Generator</button>
            <button onclick="runRangeGenerator()">Range Generator</button>
            <div id="generatorOutput" class="output">
                Click to see generator examples...
            </div>
        </div>
    </div>

    <div class="section">
        <h2>Module Pattern Demo</h2>
        <div class="demo">
            <p>Modules require separate files. Here's a demonstration of the module pattern using IIFE:</p>
            <button onclick="demoModulePattern()">Module Pattern</button>
            <div id="moduleOutput" class="output">
                Click to see module pattern example...
            </div>
        </div>
    </div>

    <div class="section">
        <h2>Console Output</h2>
        <p>Open the browser console (F12) to see all JavaScript examples.</p>
    </div>

    <script src="script.js" defer></script>
    <script>
        // Regex demo
        function testRegex() {
            const text = document.getElementById("regexInput").value;
            const patternStr = document.getElementById("regexPattern").value;
            
            try {
                // Remove surrounding slashes if present
                let pattern = patternStr;
                let flags = "g";
                
                if (patternStr.startsWith("/") && patternStr.includes("/")) {
                    const parts = patternStr.split("/");
                    pattern = parts[1];
                    flags = parts[2] || "g";
                }
                
                const regex = new RegExp(pattern, flags);
                const matches = text.match(regex);
                
                let output = `Pattern: ${patternStr}\n`;
                output += `Text: ${text}\n`;
                output += `Test result: ${regex.test(text)}\n`;
                output += `Matches: ${JSON.stringify(matches)}`;
                
                document.getElementById("regexOutput").textContent = output;
            } catch (error) {
                document.getElementById("regexOutput").textContent = "Invalid regex pattern: " + error.message;
            }
        }

        // OOP demo
        class Person {
            constructor(name, age) {
                this.name = name;
                this.age = age;
            }
            
            greet() {
                return `Hello, I'm ${this.name}, ${this.age} years old`;
            }
        }

        class Student extends Person {
            constructor(name, age, grade) {
                super(name, age);
                this.grade = grade;
            }
            
            study() {
                return `${this.name} is studying for grade ${this.grade}`;
            }
        }

        function createPerson() {
            const name = document.getElementById("personName").value || "John";
            const age = parseInt(document.getElementById("personAge").value) || 30;
            
            const person = new Person(name, age);
            document.getElementById("oopOutput").textContent = person.greet();
        }

        function createStudent() {
            const name = document.getElementById("personName").value || "John";
            const age = parseInt(document.getElementById("personAge").value) || 20;
            
            const student = new Student(name, age, "A");
            document.getElementById("oopOutput").textContent = 
                student.greet() + "\n" + student.study();
        }

        // Date demo
        function showCurrentDate() {
            const now = new Date();
            let output = "=== Current Date/Time ===\n";
            output += `Date: ${now.toLocaleDateString()}\n`;
            output += `Time: ${now.toLocaleTimeString()}\n`;
            output += `Full: ${now.toLocaleString()}\n`;
            output += `ISO: ${now.toISOString()}`;
            
            document.getElementById("dateOutput").textContent = output;
        }

        function dateArithmetic() {
            const now = new Date();
            const tomorrow = new Date(now.getTime() + 24 * 60 * 60 * 1000);
            const nextWeek = new Date(now.getTime() + 7 * 24 * 60 * 60 * 1000);
            
            let output = "=== Date Arithmetic ===\n";
            output += `Now: ${now.toLocaleDateString()}\n`;
            output += `Tomorrow: ${tomorrow.toLocaleDateString()}\n`;
            output += `Next week: ${nextWeek.toLocaleDateString()}\n`;
            output += `Days until next week: ${Math.floor((nextWeek - now) / (1000 * 60 * 60 * 24))}`;
            
            document.getElementById("dateOutput").textContent = output;
        }

        function dateFormatting() {
            const now = new Date();
            let output = "=== Date Formatting ===\n";
            output += `toString(): ${now.toString()}\n`;
            output += `toDateString(): ${now.toDateString()}\n`;
            output += `toTimeString(): ${now.toTimeString()}\n`;
            output += `toLocaleString(): ${now.toLocaleString()}\n`;
            output += `toISOString(): ${now.toISOString()}`;
            
            document.getElementById("dateOutput").textContent = output;
        }

        // Generator demo
        function* simpleGenerator() {
            yield 1;
            yield 2;
            yield 3;
        }

        function runGenerator() {
            const gen = simpleGenerator();
            let output = "=== Simple Generator ===\n";
            output += `next().value: ${gen.next().value}\n`;
            output += `next().value: ${gen.next().value}\n`;
            output += `next().value: ${gen.next().value}\n`;
            output += `next().value: ${gen.next().value} (done)`;
            
            document.getElementById("generatorOutput").textContent = output;
        }

        function* range(start, end) {
            for (let i = start; i <= end; i++) {
                yield i;
            }
        }

        function runRangeGenerator() {
            const r = range(1, 5);
            let output = "=== Range Generator (1-5) ===\n";
            for (const value of r) {
                output += `${value}\n`;
            }
            
            document.getElementById("generatorOutput").textContent = output;
        }

        // Module pattern demo (IIFE)
        function demoModulePattern() {
            const MathModule = (function() {
                const PI = 3.14159;
                
                function add(a, b) {
                    return a + b;
                }
                
                function multiply(a, b) {
                    return a * b;
                }
                
                function calculateArea(radius) {
                    return PI * radius * radius;
                }
                
                return {
                    add,
                    multiply,
                    calculateArea,
                    PI
                };
            })();
            
            let output = "=== Module Pattern (IIFE) ===\n";
            output += `MathModule.add(5, 3): ${MathModule.add(5, 3)}\n`;
            output += `MathModule.multiply(4, 6): ${MathModule.multiply(4, 6)}\n`;
            output += `MathModule.calculateArea(5): ${MathModule.calculateArea(5)}\n`;
            output += `MathModule.PI: ${MathModule.PI}`;
            
            document.getElementById("moduleOutput").textContent = output;
        }
    </script>
</body>
</html>
```

---

## 📝 Review (0.5h)

### Quick Reference

#### Regex Quick Reference
```javascript
// Modifiers
/pattern/i  // Case insensitive
/pattern/g  // Global
/pattern/m  // Multiline

// Character Classes
[abc]       // a, b, or c
[a-z]       // Lowercase letters
[0-9]       // Digits
[^abc]      // Not a, b, or c

// Shorthands
\d          // Digit
\w          // Word character
\s          // Whitespace

// Quantifiers
a*          // 0 or more
a+          // 1 or more
a?          // 0 or 1
a{3}        // Exactly 3
a{2,4}      // 2 to 4
```

#### OOP Quick Reference
```javascript
// Constructor function
function Person(name) {
    this.name = name;
}
Person.prototype.greet = function() {
    return `Hello, ${this.name}`;
};

// ES6 Class
class Person {
    constructor(name) {
        this.name = name;
    }
    greet() {
        return `Hello, ${this.name}`;
    }
}

// Inheritance
class Student extends Person {
    constructor(name, grade) {
        super(name);
        this.grade = grade;
    }
}

// Private fields (ES2022)
class Example {
    #privateField;
    constructor() {
        this.#privateField = "private";
    }
}
```

#### Date Quick Reference
```javascript
const now = new Date();

// Getters
now.getFullYear()
now.getMonth()     // 0-11
now.getDate()
now.getDay()       // 0-6
now.getHours()
now.getMinutes()
now.getSeconds()

// Setters
now.setFullYear(2024)
now.setMonth(0)
now.setDate(15)

// Formatting
now.toLocaleDateString()
now.toLocaleTimeString()
now.toISOString()
```

#### Generator Quick Reference
```javascript
function* generator() {
    yield 1;
    yield 2;
}

const gen = generator();
gen.next().value  // 1
gen.next().value  // 2

// for...of
for (const value of generator()) {
    console.log(value);
}
```

#### Module Quick Reference
```javascript
// Named export
export const add = (a, b) => a + b;
export function subtract(a, b) { return a - b; }

// Default export
export default class MyClass {}

// Named import
import { add, subtract } from './math.js';

// Default import
import MyClass from './myclass.js';

// Mixed
import MyClass, { add } from './myclass.js';

// Import all
import * as math from './math.js';
```

### Review Questions

1. **What does the regex modifier 'i' do?**
   - [ ] Global search
   - [ ] Case insensitive
   - [ ] Multiline
   - [ ] Case sensitive

2. **What is a constructor function?**
   - [ ] A function that constructs strings
   - [ ] A function used with 'new' to create objects
   - [ ] A function that deletes objects
   - [ ] A function that validates input

3. **What does 'extends' do in ES6 classes?**
   - [ ] Extends array length
   - [ ] Creates a subclass that inherits from a parent class
   - [ ] Extends string length
   - [ ] Copies object properties

4. **What is encapsulation in OOP?**
   - [ ] Making everything public
   - [ ] Hiding internal state and requiring interaction through methods
   - [ ] Creating multiple objects
   - [ ] Inheriting from parent classes

5. **What does getFullYear() return?**
   - [ ] Year as 2 digits
   - [ ] Year as 4 digits
   - [ ] Month
   - [ ] Day

6. **What is a generator function?**
   - [ ] A function that generates random numbers
   - [ ] A function that can pause and resume execution
   - [ ] A function that generates HTML
   - [ ] A function that creates objects

7. **What does 'yield' do in a generator?**
   - [ ] Stops the function permanently
   - [ ] Pauses execution and returns a value
   - [ ] Throws an error
   - [ ] Returns from the function

8. **What is a named export?**
   - [ ] Export with no name
   - [ ] Export with a specific name that must be imported with that name
   - [ ] Export that exports everything
   - [ ] Export that only works in Node.js

9. **What is a default export?**
   - [ ] The first export in a file
   - [ ] A single export that can be imported with any name
   - [ ] An export that is automatically imported
   - [ ] An export that cannot be imported

10. **What does 'super()' do in a subclass?**
    - [ ] Calls the parent class constructor
    - [ ] Creates a super object
    - [ ] Deletes the parent class
    - [ ] Exports the class

### Correct Answers

1. ✅ Case insensitive
2. ✅ A function used with 'new' to create objects
3. ✅ Creates a subclass that inherits from a parent class
4. ✅ Hiding internal state and requiring interaction through methods
5. ✅ Year as 4 digits
6. ✅ A function that can pause and resume execution
7. ✅ Pauses execution and returns a value
8. ✅ Export with a specific name that must be imported with that name
9. ✅ A single export that can be imported with any name
10. ✅ Calls the parent class constructor

---

## 🎯 Next Steps

1. ✅ Practice regex using regex101.com (essential for mastery)
2. ✅ Build OOP applications with classes and inheritance
3. ✅ Practice date manipulation for real-world scenarios
4. ✅ Explore generators for async patterns (async/await)
5. ✅ Build modular applications with ES6 modules
6. ✅ Learn about async/await and Promises
7. ✅ Explore TypeScript for type safety

---

## 📚 Additional Resources

- [MDN: Regular Expressions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions)
- [Regex101](https://regex101.com) - Interactive regex tester
- [MDN: Classes](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes)
- [MDN: Date](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date)
- [MDN: Generators](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Generator)
- [MDN: Modules](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules)
- [JavaScript.info: Classes](https://javascript.info/classes)
- [JavaScript.info: Generators](https://javascript.info/generators)

**⚠️ Important:** This session covers a lot of ground. Use regex101.com for regex practice and focus on OOP concepts for your JavaScript projects. Generators are advanced—learn them when you encounter async patterns or large data processing needs. 💪
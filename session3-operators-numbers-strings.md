# Session 3: Operators, Numbers & Strings

## 📚 Theory (1h)

### Arithmetic Operators

Arithmetic operators perform mathematical operations on numbers.

#### Basic Arithmetic Operators

| Operator | Name | Description | Example |
|----------|------|-------------|---------|
| `+` | Addition | Adds two numbers | `5 + 3` = `8` |
| `-` | Subtraction | Subtracts one number from another | `5 - 3` = `2` |
| `*` | Multiplication | Multiplies two numbers | `5 * 3` = `15` |
| `/` | Division | Divides one number by another | `6 / 3` = `2` |
| `%` | Modulus | Returns remainder of division | `5 % 3` = `2` |
| `**` | Exponentiation | Raises to the power of | `2 ** 3` = `8` |

**Examples:**
```javascript
// Basic operations
console.log(5 + 3);    // 8
console.log(10 - 4);   // 6
console.log(6 * 7);    // 42
console.log(15 / 3);   // 5
console.log(17 % 5);   // 2 (remainder)
console.log(2 ** 3);   // 8

// Order of operations (PEMDAS)
console.log(2 + 3 * 4);     // 14 (multiplication first)
console.log((2 + 3) * 4);   // 20 (parentheses first)
console.log(10 / 2 + 3 * 2); // 10 (division and multiplication first)
```

#### Special Cases with Arithmetic

**Division by Zero:**
```javascript
console.log(10 / 0);      // Infinity
console.log(-10 / 0);     // -Infinity
console.log(0 / 0);       // NaN (Not a Number)
```

**Working with Decimals:**
```javascript
console.log(0.1 + 0.2);   // 0.30000000000000004 (floating point precision issue)
console.log((0.1 + 0.2).toFixed(2)); // "0.30" (fixed precision)
```

### Unary Plus & Negation

Unary operators work on a single operand.

#### Unary Plus (`+`)

Converts a value to a number (similar to `Number()`).

```javascript
// Converting strings to numbers
console.log(+"42");        // 42
console.log(+"3.14");      // 3.14
console.log(+"Hello");     // NaN

// Converting booleans
console.log(+true);        // 1
console.log(+false);       // 0

// Converting null and undefined
console.log(+null);        // 0
console.log(+undefined);   // NaN

// Converting dates
console.log(+new Date());  // timestamp (number)
```

#### Unary Negation (`-`)

Converts a value to a number and negates it.

```javascript
// Negating numbers
console.log(-42);          // -42
console.log(-(-42));       // 42

// Converting and negating
console.log(-"42");        // -42
console.log(-"3.14");      // -3.14
console.log(-"Hello");     // NaN

// With unary plus
console.log(-+"42");       // -42 (first +, then -)
```

**Practical Uses:**
```javascript
// Quick string to number conversion
let strNumber = "100";
let num = +strNumber;      // 100

// Calculating difference
let x = 10;
let y = 5;
let difference = x - y;    // 5
let negatedDiff = -(x - y); // -5
```

### Type Coercion in Operations

JavaScript automatically converts types during operations.

#### String Coercion with `+`

When one operand is a string, `+` performs concatenation.

```javascript
// Number + String = String
console.log(5 + "3");      // "53"
console.log("10" + 20);    // "1020"
console.log(1 + 2 + "3");  // "33" (1+2=3, then "3"+"3")
console.log("1" + 2 + 3);  // "123" (string concatenation)

// Boolean + String = String
console.log(true + "Hello"); // "trueHello"
console.log(false + "Bye");  // "falseBye"
```

#### Numeric Coercion with `-`, `*`, `/`, `%`

These operators convert strings to numbers.

```javascript
// String - Number = Number
console.log("10" - 5);     // 5
console.log("10" - "5");   // 5

// String * Number = Number
console.log("3" * 4);      // 12
console.log("2" * "3");    // 6

// String / Number = Number
console.log("20" / 4);     // 5
console.log("20" / "4");   // 5

// String % Number = Number
console.log("10" % 3);     // 1
console.log("10" % "3");   // 1
```

#### Coercion Edge Cases

```javascript
// NaN propagation
console.log(NaN + 5);      // NaN
console.log(NaN * 5);      // NaN

// Null coercion
console.log(null + 5);     // 5 (null becomes 0)
console.log(null - 5);     // -5
console.log(null * 5);     // 0

// Undefined coercion
console.log(undefined + 5); // NaN
console.log(undefined - 5); // NaN

// Boolean coercion
console.log(true + 5);     // 6 (true becomes 1)
console.log(false + 5);    // 5 (false becomes 0)
console.log(true * 5);     // 5
console.log(false * 5);    // 0
```

### Assignment Operators

Assignment operators assign values to variables.

#### Basic Assignment

```javascript
let x = 10;  // Simple assignment
```

#### Compound Assignment Operators

| Operator | Name | Equivalent | Example |
|----------|------|------------|---------|
| `+=` | Add and assign | `x = x + y` | `x += 5` |
| `-=` | Subtract and assign | `x = x - y` | `x -= 3` |
| `*=` | Multiply and assign | `x = x * y` | `x *= 2` |
| `/=` | Divide and assign | `x = x / y` | `x /= 4` |
| `%=` | Modulus and assign | `x = x % y` | `x %= 3` |
| `**=` | Exponent and assign | `x = x ** y` | `x **= 2` |

**Examples:**
```javascript
let x = 10;

x += 5;   // x = 10 + 5 = 15
console.log(x); // 15

x -= 3;   // x = 15 - 3 = 12
console.log(x); // 12

x *= 2;   // x = 12 * 2 = 24
console.log(x); // 24

x /= 4;   // x = 24 / 4 = 6
console.log(x); // 6

x %= 4;   // x = 6 % 4 = 2
console.log(x); // 2

x **= 3;  // x = 2 ** 3 = 8
console.log(x); // 8
```

#### String Concatenation Assignment

```javascript
let message = "Hello";
message += " ";        // "Hello "
message += "World";    // "Hello World"
console.log(message);  // "Hello World"
```

#### Assignment with Type Coercion

```javascript
let x = "10";
x += 5;   // "105" (string concatenation)
console.log(x);

let y = "10";
y -= 5;   // 5 (numeric conversion)
console.log(y);
```

---

## 💻 Practical (1.5h)

### Exercise 1: Arithmetic Operators

```javascript
// Exercise 1.1: Basic arithmetic operations
console.log("=== Basic Arithmetic Operations ===");

let a = 15;
let b = 4;

console.log("a =", a, "b =", b);
console.log("a + b =", a + b);    // 19
console.log("a - b =", a - b);    // 11
console.log("a * b =", a * b);    // 60
console.log("a / b =", a / b);    // 3.75
console.log("a % b =", a % b);    // 3
console.log("a ** b =", a ** b);  // 50625

// Exercise 1.2: Order of operations
console.log("\n=== Order of Operations ===");

console.log("2 + 3 * 4 =", 2 + 3 * 4);       // 14
console.log("(2 + 3) * 4 =", (2 + 3) * 4);   // 20
console.log("10 + 6 / 2 =", 10 + 6 / 2);     // 13
console.log("(10 + 6) / 2 =", (10 + 6) / 2); // 8

// Exercise 1.3: Complex expression
console.log("\n=== Complex Expression ===");

let result = ((10 + 5) * 2 - 8) / 4;
console.log("((10 + 5) * 2 - 8) / 4 =", result); // 5.5

// Exercise 1.4: Practical calculator
console.log("\n=== Practical Calculator ===");

function calculate(price, quantity, taxRate, discount) {
    let subtotal = price * quantity;
    let taxAmount = subtotal * (taxRate / 100);
    let discountAmount = subtotal * (discount / 100);
    let total = subtotal + taxAmount - discountAmount;
    
    return {
        subtotal: subtotal.toFixed(2),
        taxAmount: taxAmount.toFixed(2),
        discountAmount: discountAmount.toFixed(2),
        total: total.toFixed(2)
    };
}

let purchase = calculate(19.99, 3, 8.5, 10);
console.log("Purchase calculation:", purchase);
```

### Exercise 2: Unary Plus & Negation

```javascript
// Exercise 2.1: Unary plus conversion
console.log("=== Unary Plus Conversion ===");

console.log(+"42");           // 42
console.log(+"3.14");         // 3.14
console.log(+"Hello");        // NaN
console.log(+true);          // 1
console.log(+false);         // 0
console.log(+null);          // 0
console.log(+undefined);     // NaN

// Exercise 2.2: Unary negation
console.log("\n=== Unary Negation ===");

console.log(-42);            // -42
console.log(-3.14);          // -3.14
console.log(-(-42));         // 42
console.log(-"100");         // -100
console.log(-"Hello");       // NaN

// Exercise 2.3: Practical usage
console.log("\n=== Practical Usage ===");

// String to number conversion
let strPrice = "19.99";
let price = +strPrice;
console.log("String price:", strPrice, "Number price:", price);

// Calculating differences
let initialScore = 100;
let finalScore = 85;
let scoreChange = finalScore - initialScore;
console.log("Score change:", scoreChange);
console.log("Absolute change:", Math.abs(scoreChange));

// Temperature conversion
let celsius = 25;
let fahrenheit = (celsius * 9/5) + 32;
console.log(`${celsius}°C = ${fahrenheit}°F`);

let kelvin = celsius + 273.15;
console.log(`${celsius}°C = ${kelvin}K`);
```

### Exercise 3: Type Coercion

```javascript
// Exercise 3.1: String coercion with +
console.log("=== String Coercion with + ===");

console.log(5 + "3");         // "53"
console.log("10" + 20);       // "1020"
console.log(1 + 2 + "3");     // "33"
console.log("1" + 2 + 3);     // "123"
console.log(true + "Hello");  // "trueHello"

// Exercise 3.2: Numeric coercion with other operators
console.log("\n=== Numeric Coercion ===");

console.log("10" - 5);        // 5
console.log("10" - "5");      // 5
console.log("3" * 4);         // 12
console.log("20" / 4);        // 5
console.log("10" % 3);        // 1

// Exercise 3.3: Edge cases
console.log("\n=== Coercion Edge Cases ===");

console.log(NaN + 5);         // NaN
console.log(null + 5);        // 5
console.log(undefined + 5);   // NaN
console.log(true + 5);        // 6
console.log(false + 5);       // 5

// Exercise 3.4: Coercion challenges
console.log("\n=== Coercion Challenges ===");

// Challenge 1: What's the result?
console.log("1" + 1);         // "11"
console.log("1" - 1);         // 0
console.log("1" * 1);         // 1
console.log("1" / 1);         // 1

// Challenge 2: Complex coercion
console.log("10" + 5 + 2);    // "1052"
console.log(10 + 5 + "2");    // "152"
console.log("10" - 5 + 2);    // 7
console.log(10 - "5" + 2);    // 7

// Challenge 3: Boolean coercion
console.log(true + true);     // 2
console.log(false + false);   // 0
console.log(true + false);    // 1
console.log(true * true);     // 1
console.log(true * false);    // 0
```

### Exercise 4: Assignment Operators

```javascript
// Exercise 4.1: Compound assignment
console.log("=== Compound Assignment Operators ===");

let x = 10;
console.log("Initial x:", x);

x += 5;
console.log("After x += 5:", x);  // 15

x -= 3;
console.log("After x -= 3:", x);  // 12

x *= 2;
console.log("After x *= 2:", x);  // 24

x /= 4;
console.log("After x /= 4:", x);  // 6

x %= 4;
console.log("After x %= 4:", x);  // 2

x **= 3;
console.log("After x **= 3:", x);  // 8

// Exercise 4.2: String concatenation assignment
console.log("\n=== String Concatenation Assignment ===");

let message = "Hello";
message += " ";
message += "World";
message += "!";
console.log("Message:", message);  // "Hello World!"

// Exercise 4.3: Counter pattern
console.log("\n=== Counter Pattern ===");

let counter = 0;
console.log("Initial counter:", counter);

counter += 1;  // Increment
console.log("After increment:", counter);

counter += 5;  // Add multiple
console.log("After adding 5:", counter);

counter -= 2;  // Decrement
console.log("After subtracting 2:", counter);

counter *= 3;  // Multiply
console.log("After multiplying by 3:", counter);

// Exercise 4.4: Practical example - shopping cart
console.log("\n=== Shopping Cart Example ===");

let cartTotal = 0;
console.log("Initial cart total:", cartTotal);

cartTotal += 19.99;  // Add item
console.log("After adding $19.99:", cartTotal.toFixed(2));

cartTotal += 5.50;   // Add another item
console.log("After adding $5.50:", cartTotal.toFixed(2));

cartTotal *= 0.9;    // Apply 10% discount
console.log("After 10% discount:", cartTotal.toFixed(2));

cartTotal += 2.00;   // Add shipping
console.log("After adding $2.00 shipping:", cartTotal.toFixed(2));
```

### Exercise 5: Number & Number Methods

```javascript
// Exercise 5.1: Number properties
console.log("=== Number Properties ===");

console.log("Maximum safe integer:", Number.MAX_SAFE_INTEGER);
console.log("Minimum safe integer:", Number.MIN_SAFE_INTEGER);
console.log("Maximum value:", Number.MAX_VALUE);
console.log("Minimum value:", Number.MIN_VALUE);
console.log("Positive infinity:", Number.POSITIVE_INFINITY);
console.log("Negative infinity:", Number.NEGATIVE_INFINITY);
console.log("Not a Number:", Number.NaN);

// Exercise 5.2: Number conversion methods
console.log("\n=== Number Conversion Methods ===");

console.log("Number('42'):", Number("42"));           // 42
console.log("Number('3.14'):", Number("3.14"));       // 3.14
console.log("Number('Hello'):", Number("Hello"));     // NaN
console.log("Number(true):", Number(true));           // 1
console.log("Number(false):", Number(false));         // 0
console.log("Number(null):", Number(null));           // 0
console.log("Number(undefined):", Number(undefined)); // NaN

// Exercise 5.3: Number parsing methods
console.log("\n=== Number Parsing Methods ===");

console.log("parseInt('42'):", parseInt("42"));           // 42
console.log("parseInt('42px'):", parseInt("42px"));       // 42
console.log("parseInt('3.14'):", parseInt("3.14"));       // 3
console.log("parseInt('Hello'):", parseInt("Hello"));     // NaN

console.log("parseFloat('3.14'):", parseFloat("3.14"));   // 3.14
console.log("parseFloat('3.14px'):", parseFloat("3.14px")); // 3.14
console.log("parseFloat('Hello'):", parseFloat("Hello"));   // NaN

// Exercise 5.4: Number validation methods
console.log("\n=== Number Validation Methods ===");

console.log("Number.isInteger(42):", Number.isInteger(42));       // true
console.log("Number.isInteger(3.14):", Number.isInteger(3.14));   // false
console.log("Number.isInteger('42'):", Number.isInteger("42"));   // false

console.log("Number.isFinite(42):", Number.isFinite(42));         // true
console.log("Number.isFinite(Infinity):", Number.isFinite(Infinity)); // false
console.log("Number.isFinite(NaN):", Number.isFinite(NaN));       // false

console.log("Number.isNaN(NaN):", Number.isNaN(NaN));             // true
console.log("Number.isNaN(42):", Number.isNaN(42));               // false
console.log("Number.isNaN('Hello'):", Number.isNaN("Hello"));     // false

// Exercise 5.5: Number formatting
console.log("\n=== Number Formatting ===");

let num = 1234.56789;

console.log("toFixed(2):", num.toFixed(2));           // "1234.57"
console.log("toFixed(0):", num.toFixed(0));           // "1235"
console.log("toPrecision(4):", num.toPrecision(4));   // "1235"
console.log("toExponential(2):", num.toExponential(2)); // "1.23e+3"

console.log("toString():", num.toString());           // "1234.56789"
console.log("toString(2):", num.toString(2));        // Binary
console.log("toString(8):", num.toString(8));        // Octal
console.log("toString(16):", num.toString(16));       // Hexadecimal
```

### Exercise 6: Math Object

```javascript
// Exercise 6.1: Basic Math methods
console.log("=== Basic Math Methods ===");

console.log("Math.abs(-5):", Math.abs(-5));           // 5
console.log("Math.round(4.7):", Math.round(4.7));     // 5
console.log("Math.floor(4.9):", Math.floor(4.9));     // 4
console.log("Math.ceil(4.1):", Math.ceil(4.1));       // 5
console.log("Math.trunc(4.9):", Math.trunc(4.9));     // 4

// Exercise 6.2: Power and root methods
console.log("\n=== Power and Root Methods ===");

console.log("Math.pow(2, 3):", Math.pow(2, 3));       // 8
console.log("Math.sqrt(16):", Math.sqrt(16));         // 4
console.log("Math.cbrt(27):", Math.cbrt(27));         // 3

// Exercise 6.3: Trigonometric methods
console.log("\n=== Trigonometric Methods ===");

console.log("Math.sin(Math.PI/2):", Math.sin(Math.PI/2));  // 1
console.log("Math.cos(0):", Math.cos(0));                  // 1
console.log("Math.tan(Math.PI/4):", Math.tan(Math.PI/4));  // 1

// Exercise 6.4: Random numbers
console.log("\n=== Random Numbers ===");

console.log("Math.random():", Math.random());             // 0 to 1
console.log("Math.random() * 10:", Math.random() * 10);   // 0 to 10
console.log("Math.floor(Math.random() * 10):", Math.floor(Math.random() * 10)); // 0 to 9

// Random integer in range
function getRandomInt(min, max) {
    return Math.floor(Math.random() * (max - min + 1)) + min;
}

console.log("Random 1-10:", getRandomInt(1, 10));
console.log("Random 1-100:", getRandomInt(1, 100));

// Exercise 6.5: Math constants
console.log("\n=== Math Constants ===");

console.log("Math.PI:", Math.PI);
console.log("Math.E:", Math.E);
console.log("Math.LN2:", Math.LN2);
console.log("Math.LN10:", Math.LN10);
console.log("Math.LOG2E:", Math.LOG2E);
console.log("Math.LOG10E:", Math.LOG10E);
console.log("Math.SQRT2:", Math.SQRT2);
console.log("Math.SQRT1_2:", Math.SQRT1_2);

// Exercise 6.6: Max and Min
console.log("\n=== Max and Min ===");

console.log("Math.max(1, 5, 3):", Math.max(1, 5, 3));     // 5
console.log("Math.min(1, 5, 3):", Math.min(1, 5, 3));     // 1
console.log("Math.max(...[1, 5, 3]):", Math.max(...[1, 5, 3])); // 5

// Exercise 6.7: Practical examples
console.log("\n=== Practical Examples ===");

// Calculate circle area
function calculateCircleArea(radius) {
    return Math.PI * Math.pow(radius, 2);
}

console.log("Circle area (r=5):", calculateCircleArea(5).toFixed(2));

// Calculate hypotenuse
function calculateHypotenuse(a, b) {
    return Math.sqrt(Math.pow(a, 2) + Math.pow(b, 2));
}

console.log("Hypotenuse (3, 4):", calculateHypotenuse(3, 4));

// Round to nearest 0.5
function roundToHalf(num) {
    return Math.round(num * 2) / 2;
}

console.log("Round to 0.5 (2.3):", roundToHalf(2.3));  // 2.5
console.log("Round to 0.5 (2.7):", roundToHalf(2.7));  // 3.0
```

### Exercise 7: String Methods (Part 1-3)

#### Part 1: Basic String Methods

```javascript
// Exercise 7.1: String length and access
console.log("=== String Length and Access ===");

let text = "Hello, World!";

console.log("Text:", text);
console.log("Length:", text.length);
console.log("First character:", text[0]);
console.log("Last character:", text[text.length - 1]);
console.log("Character at index 7:", text.charAt(7));

// Exercise 7.2: Case conversion
console.log("\n=== Case Conversion ===");

console.log("Uppercase:", text.toUpperCase());
console.log("Lowercase:", text.toLowerCase());

// Exercise 7.3: Searching within strings
console.log("\n=== Searching Within Strings ===");

console.log("indexOf('World'):", text.indexOf("World"));      // 7
console.log("indexOf('world'):", text.indexOf("world"));      // -1 (case sensitive)
console.log("lastIndexOf('o'):", text.lastIndexOf("o"));      // 8
console.log("includes('World'):", text.includes("World"));   // true
console.log("includes('world'):", text.includes("world"));   // false
console.log("startsWith('Hello'):", text.startsWith("Hello")); // true
console.log("endsWith('!'):", text.endsWith("!"));           // true

// Exercise 7.4: Extracting parts of strings
console.log("\n=== Extracting Parts ===");

console.log("slice(0, 5):", text.slice(0, 5));           // "Hello"
console.log("slice(7):", text.slice(7));                 // "World!"
console.log("slice(-6):", text.slice(-6));               // "World!"
console.log("slice(0, -1):", text.slice(0, -1));         // "Hello, World"

console.log("substring(0, 5):", text.substring(0, 5));   // "Hello"
console.log("substring(7):", text.substring(7));         // "World!"

console.log("substr(0, 5):", text.substr(0, 5));         // "Hello"
console.log("substr(7, 5):", text.substr(7, 5));         // "World"
```

#### Part 2: String Modification Methods

```javascript
// Exercise 7.5: Replacing content
console.log("\n=== Replacing Content ===");

let newText = text.replace("World", "JavaScript");
console.log("Replace 'World' with 'JavaScript':", newText);

// Replace all occurrences
let repeatText = "Hello World, Hello World";
let allReplaced = repeatText.replace(/Hello/g, "Hi");
console.log("Replace all 'Hello' with 'Hi':", allReplaced);

// Case-insensitive replace
let caseText = "Hello WORLD, hello world";
let caseReplaced = caseText.replace(/hello/gi, "Hi");
console.log("Case-insensitive replace:", caseReplaced);

// Exercise 7.6: Splitting strings
console.log("\n=== Splitting Strings ===");

let csv = "apple,banana,orange,grape";
let fruits = csv.split(",");
console.log("Split CSV:", fruits);

let sentence = "This is a sentence";
let words = sentence.split(" ");
console.log("Split sentence:", words);

let chars = text.split("");
console.log("Split into characters:", chars);

// Exercise 7.7: Trimming strings
console.log("\n=== Trimming Strings ===");

let paddedText = "   Hello, World!   ";
console.log("Original:", `"${paddedText}"`);
console.log("Trim:", `"${paddedText.trim()}"`);
console.log("TrimStart:", `"${paddedText.trimStart()}"`);
console.log("TrimEnd:", `"${paddedText.trimEnd()}"`);

// Exercise 7.8: Padding strings
console.log("\n=== Padding Strings ===");

let num = "42";
console.log("PadStart(5, '0'):", num.padStart(5, "0"));    // "00042"
console.log("PadEnd(5, '*'):", num.padEnd(5, "*"));        // "42***"

let name = "John";
console.log("PadStart(10, '-'):", name.padStart(10, "-")); // "------John"
```

#### Part 3: Advanced String Methods

```javascript
// Exercise 7.9: Repeating strings
console.log("\n=== Repeating Strings ===");

console.log("'Ha'.repeat(3):", "Ha".repeat(3));           // "HaHaHa"
console.log("'abc'.repeat(2):", "abc".repeat(2));         // "abcabc"

// Exercise 7.10: String comparison
console.log("\n=== String Comparison ===");

let str1 = "apple";
let str2 = "banana";
let str3 = "Apple";

console.log("'apple' < 'banana':", str1 < str2);          // true
console.log("'apple' > 'Apple':", str1 > str3);          // true (uppercase comes first)
console.log("'apple'.localeCompare('banana'):", str1.localeCompare(str2)); // -1

// Exercise 7.11: Checking string types
console.log("\n=== Checking String Types ===");

console.log("''.length === 0:", "".length === 0);          // true (empty string)
console.log("'   '.trim().length === 0:", "   ".trim().length === 0); // true (whitespace only)

// Exercise 7.12: Practical string manipulation
console.log("\n=== Practical String Manipulation ===");

// Format a name
function formatName(firstName, lastName) {
    return (firstName.charAt(0).toUpperCase() + firstName.slice(1).toLowerCase()) + " " + 
           (lastName.charAt(0).toUpperCase() + lastName.slice(1).toLowerCase());
}

console.log("Format 'john DOE':", formatName("john", "DOE")); // "John Doe"

// Create initials
function getInitials(firstName, lastName) {
    return (firstName.charAt(0) + lastName.charAt(0)).toUpperCase();
}

console.log("Initials of 'John Doe':", getInitials("John", "Doe")); // "JD"

// Truncate text
function truncateText(text, maxLength) {
    if (text.length <= maxLength) return text;
    return text.slice(0, maxLength - 3) + "...";
}

console.log("Truncate 'Hello World' to 8:", truncateText("Hello World", 8)); // "Hello..."

// Convert to title case
function toTitleCase(str) {
    return str.toLowerCase().split(' ').map(word => 
        word.charAt(0).toUpperCase() + word.slice(1)
    ).join(' ');
}

console.log("Title case 'hello world':", toTitleCase("hello world")); // "Hello World"
```

### Exercise 8: Complete Working Example

**Complete script.js:**
```javascript
// Session 3: Operators, Numbers & Strings
// This script demonstrates operators, number operations, and string methods

console.log("=== Session 3: Operators, Numbers & Strings ===");

// 1. Arithmetic operators
console.log("\n--- Arithmetic Operators ---");
let a = 15, b = 4;
console.log(`${a} + ${b} = ${a + b}`);
console.log(`${a} - ${b} = ${a - b}`);
console.log(`${a} * ${b} = ${a * b}`);
console.log(`${a} / ${b} = ${a / b}`);
console.log(`${a} % ${b} = ${a % b}`);
console.log(`${a} ** ${b} = ${a ** b}`);

// 2. Unary operators
console.log("\n--- Unary Operators ---");
console.log("+'42':", +"42");
console.log(-"100":, -"100");
console.log("+'Hello':", +"Hello");

// 3. Type coercion
console.log("\n--- Type Coercion ---");
console.log("'10' + 5:", "10" + 5);
console.log("'10' - 5:", "10" - 5);
console.log("true + 5:", true + 5);
console.log("null + 5:", null + 5);

// 4. Assignment operators
console.log("\n--- Assignment Operators ---");
let x = 10;
console.log("Initial x:", x);
x += 5;
console.log("After x += 5:", x);
x *= 2;
console.log("After x *= 2:", x);

// 5. Number methods
console.log("\n--- Number Methods ---");
let num = 1234.56789;
console.log("toFixed(2):", num.toFixed(2));
console.log("toPrecision(4):", num.toPrecision(4));
console.log("parseInt('42px'):", parseInt("42px"));
console.log("parseFloat('3.14px'):", parseFloat("3.14px"));

// 6. Math object
console.log("\n--- Math Object ---");
console.log("Math.round(4.7):", Math.round(4.7));
console.log("Math.floor(4.9):", Math.floor(4.9));
console.log("Math.ceil(4.1):", Math.ceil(4.1));
console.log("Math.random():", Math.random());
console.log("Math.max(1, 5, 3):", Math.max(1, 5, 3));

// 7. String methods
console.log("\n--- String Methods ---");
let text = "Hello, World!";
console.log("toUpperCase():", text.toUpperCase());
console.log("indexOf('World'):", text.indexOf("World"));
console.log("slice(0, 5):", text.slice(0, 5));
console.log("replace('World', 'JavaScript'):", text.replace("World", "JavaScript"));
console.log("split(','):", "apple,banana,orange".split(","));

// 8. Practical example: Shopping calculator
console.log("\n--- Shopping Calculator ---");

function calculateTotal(price, quantity, taxRate, discount) {
    const subtotal = price * quantity;
    const taxAmount = subtotal * (taxRate / 100);
    const discountAmount = subtotal * (discount / 100);
    const total = subtotal + taxAmount - discountAmount;
    
    return {
        subtotal: subtotal.toFixed(2),
        taxAmount: taxAmount.toFixed(2),
        discountAmount: discountAmount.toFixed(2),
        total: total.toFixed(2)
    };
}

const cart = calculateTotal(19.99, 3, 8.5, 10);
console.log("Cart calculation:", cart);

console.log("\n=== Session 3 Complete ===");
```

**Complete index.html:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Session 3 - Operators, Numbers & Strings</title>
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
        .calculator {
            background: #f9f9f9;
            padding: 15px;
            border-radius: 4px;
            margin-top: 10px;
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
    <h1>Session 3: Operators, Numbers & Strings</h1>
    
    <div class="section">
        <h2>Topics Covered</h2>
        <ul>
            <li>Arithmetic operators (+, -, *, /, %, **)</li>
            <li>Unary plus and negation</li>
            <li>Type coercion in operations</li>
            <li>Assignment operators (+=, -=, *=, etc.)</li>
            <li>Number methods and parsing</li>
            <li>Math object functions</li>
            <li>String methods (basic, modification, advanced)</li>
        </ul>
    </div>

    <div class="section">
        <h2>Interactive Calculator</h2>
        <div class="calculator">
            <label for="price">Price: $</label>
            <input type="number" id="price" value="19.99" step="0.01">
            
            <label for="quantity">Quantity:</label>
            <input type="number" id="quantity" value="3" min="1">
            
            <label for="tax">Tax Rate (%):</label>
            <input type="number" id="tax" value="8.5" step="0.1">
            
            <label for="discount">Discount (%):</label>
            <input type="number" id="discount" value="10" step="0.1">
            
            <button onclick="calculateCart()">Calculate</button>
            
            <div id="result" class="output" style="margin-top: 10px;">
                Click Calculate to see results...
            </div>
        </div>
    </div>

    <div class="section">
        <h2>Console Output</h2>
        <p>Open the browser console (F12) to see all JavaScript examples.</p>
    </div>

    <script src="script.js" defer></script>
    <script>
        function calculateCart() {
            const price = parseFloat(document.getElementById('price').value);
            const quantity = parseInt(document.getElementById('quantity').value);
            const taxRate = parseFloat(document.getElementById('tax').value);
            const discount = parseFloat(document.getElementById('discount').value);
            
            const subtotal = price * quantity;
            const taxAmount = subtotal * (taxRate / 100);
            const discountAmount = subtotal * (discount / 100);
            const total = subtotal + taxAmount - discountAmount;
            
            const result = `
                Subtotal: $${subtotal.toFixed(2)}
                Tax: $${taxAmount.toFixed(2)}
                Discount: -$${discountAmount.toFixed(2)}
                Total: $${total.toFixed(2)}
            `;
            
            document.getElementById('result').textContent = result;
        }
    </script>
</body>
</html>
```

---

## 📝 Review (0.5h)

### Operators Challenge

```javascript
// Challenge 1: What's the output?
console.log("10" + 5);        // ?
console.log("10" - 5);        // ?
console.log("10" * 5);        // ?
console.log("10" / 5);        // ?
console.log("10" % 3);        // ?

// Challenge 2: Complex expressions
console.log(2 + 3 * 4);      // ?
console.log((2 + 3) * 4);    // ?
console.log(10 / 2 + 3 * 2); // ?
console.log(20 % 6 + 4 ** 2); // ?

// Challenge 3: Assignment operators
let x = 10;
x += 5;
x *= 2;
x -= 10;
console.log(x);              // ?

// Challenge 4: Type coercion
console.log(true + 1);       // ?
console.log(false + 1);      // ?
console.log(null + 5);       // ?
console.log(undefined + 5);  // ?
console.log(NaN + 5);        // ?
```

### Number Challenge

```javascript
// Challenge 1: Number conversion
console.log(Number("42"));          // ?
console.log(Number("Hello"));       // ?
console.log(Number(true));          // ?
console.log(Number(null));          // ?
console.log(Number(undefined));     // ?

// Challenge 2: Parsing
console.log(parseInt("42px"));      // ?
console.log(parseInt("Hello"));     // ?
console.log(parseFloat("3.14px"));  // ?
console.log(parseFloat("Hello"));   // ?

// Challenge 3: Number methods
let num = 1234.56789;
console.log(num.toFixed(2));        // ?
console.log(num.toPrecision(4));    // ?
console.log(num.toExponential(2)); // ?

// Challenge 4: Math functions
console.log(Math.round(4.7));      // ?
console.log(Math.floor(4.9));      // ?
console.log(Math.ceil(4.1));      // ?
console.log(Math.abs(-5));         // ?
console.log(Math.pow(2, 3));       // ?
console.log(Math.sqrt(16));        // ?

// Challenge 5: Random number
// Generate a random number between 1 and 10
// Your code here
```

### String Challenge

```javascript
// Challenge 1: String methods
let text = "Hello, World!";
console.log(text.length);          // ?
console.log(text.toUpperCase());   // ?
console.log(text.indexOf("World")); // ?
console.log(text.slice(0, 5));     // ?
console.log(text.replace("World", "JavaScript")); // ?

// Challenge 2: String manipulation
let name = "john doe";
// Convert to title case (John Doe)
// Your code here

// Challenge 3: String operations
let str1 = "Hello";
let str2 = "World";
// Concatenate with a space: "Hello World"
// Your code here

// Challenge 4: String parsing
let csv = "apple,banana,orange";
// Split into array: ["apple", "banana", "orange"]
// Your code here

// Challenge 5: String validation
let email = "user@example.com";
// Check if it contains "@"
// Your code here
```

### Challenge Solutions

**Operators Challenge Solutions:**
```javascript
// Challenge 1
console.log("10" + 5);        // "105"
console.log("10" - 5);        // 5
console.log("10" * 5);        // 50
console.log("10" / 5);        // 2
console.log("10" % 3);        // 1

// Challenge 2
console.log(2 + 3 * 4);      // 14
console.log((2 + 3) * 4);    // 20
console.log(10 / 2 + 3 * 2); // 10
console.log(20 % 6 + 4 ** 2); // 18

// Challenge 3
let x = 10;
x += 5;  // 15
x *= 2;  // 30
x -= 10; // 20
console.log(x);              // 20

// Challenge 4
console.log(true + 1);       // 2
console.log(false + 1);      // 1
console.log(null + 5);       // 5
console.log(undefined + 5);  // NaN
console.log(NaN + 5);        // NaN
```

**Number Challenge Solutions:**
```javascript
// Challenge 1
console.log(Number("42"));          // 42
console.log(Number("Hello"));       // NaN
console.log(Number(true));          // 1
console.log(Number(null));          // 0
console.log(Number(undefined));     // NaN

// Challenge 2
console.log(parseInt("42px"));      // 42
console.log(parseInt("Hello"));     // NaN
console.log(parseFloat("3.14px"));  // 3.14
console.log(parseFloat("Hello"));   // NaN

// Challenge 3
let num = 1234.56789;
console.log(num.toFixed(2));        // "1234.57"
console.log(num.toPrecision(4));    // "1235"
console.log(num.toExponential(2)); // "1.23e+3"

// Challenge 4
console.log(Math.round(4.7));      // 5
console.log(Math.floor(4.9));      // 4
console.log(Math.ceil(4.1));      // 5
console.log(Math.abs(-5));         // 5
console.log(Math.pow(2, 3));       // 8
console.log(Math.sqrt(16));        // 4

// Challenge 5
console.log(Math.floor(Math.random() * 10) + 1); // 1-10
```

**String Challenge Solutions:**
```javascript
// Challenge 1
let text = "Hello, World!";
console.log(text.length);          // 13
console.log(text.toUpperCase());   // "HELLO, WORLD!"
console.log(text.indexOf("World")); // 7
console.log(text.slice(0, 5));     // "Hello"
console.log(text.replace("World", "JavaScript")); // "Hello, JavaScript!"

// Challenge 2
let name = "john doe";
let titleCase = name.toLowerCase().split(' ').map(word => 
    word.charAt(0).toUpperCase() + word.slice(1)
).join(' ');
console.log(titleCase); // "John Doe"

// Challenge 3
let str1 = "Hello";
let str2 = "World";
console.log(str1 + " " + str2); // "Hello World"
// Or: console.log(`${str1} ${str2}`);

// Challenge 4
let csv = "apple,banana,orange";
console.log(csv.split(",")); // ["apple", "banana", "orange"]

// Challenge 5
let email = "user@example.com";
console.log(email.includes("@")); // true
```

### Review Questions

1. **What is the result of `"10" + 5`?**
   - [ ] 15
   - [ ] "105"
   - [ ] NaN
   - [ ] Error

2. **Which operator converts a string to a number?**
   - [ ] `-`
   - [ ] `+`
   - [ ] `*`
   - [ ] `/`

3. **What does `Math.floor(4.9)` return?**
   - [ ] 4
   - [ ] 5
   - [ ] 4.9
   - [ ] 4.5

4. **What is the result of `parseInt("42px")`?**
   - [ ] 42
   - [ ] NaN
   - [ ] "42px"
   - [ ] Error

5. **Which method rounds a number to a specified decimal places?**
   - [ ] `Math.round()`
   - [ ] `toFixed()`
   - [ ] `Math.floor()`
   - [ ] `toPrecision()`

6. **What does `"Hello".slice(0, 3)` return?**
   - [ ] "Hel"
   - [ ] "ello"
   - [ ] "llo"
   - [ ] "Hello"

7. **What is the result of `true + 5`?**
   - [ ] 6
   - [ ] "true5"
   - [ ] NaN
   - [ ] 5

8. **Which method removes whitespace from both ends of a string?**
   - [ ] `trimStart()`
   - [ ] `trimEnd()`
   - [ ] `trim()`
   - [ ] `strip()`

9. **What does `Math.random()` return?**
   - [ ] A random integer
   - [ ] A number between 0 and 1
   - [ ] A number between 1 and 10
   - [ ] A random string

10. **What is the result of `"10" - 5`?**
    - [ ] "105"
    - [ ] 5
    - [ ] NaN
    - [ ] Error

### Correct Answers

1. ✅ "105"
2. ✅ `+`
3. ✅ 4
4. ✅ 42
5. ✅ `toFixed()`
6. ✅ "Hel"
7. ✅ 6
8. ✅ `trim()`
9. ✅ A number between 0 and 1
10. ✅ 5

---

## 🎯 Next Steps

1. ✅ Practice all arithmetic operators
2. ✅ Master type coercion rules
3. ✅ Learn all Number methods
4. ✅ Explore Math object functions
5. ✅ Practice string manipulation
6. ✅ Complete all challenge exercises
7. ✅ Build practical calculators and formatters

---

## 📚 Additional Resources

- [MDN: Arithmetic Operators](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Arithmetic_Operators)
- [MDN: Number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number)
- [MDN: Math](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math)
- [MDN: String Methods](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)
- [JavaScript.info: Operators](https://javascript.info/operators)
- [JavaScript.info: Numbers](https://javascript.info/number)
- [JavaScript.info: Strings](https://javascript.info/string)

**Remember:** Understanding operators and type coercion is crucial for writing bug-free JavaScript code. Practice these concepts regularly! 💪
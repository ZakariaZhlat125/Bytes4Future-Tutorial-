# Session 3: Operators, Numbers & Strings — Active Learning Redesign

## Session Plan for the Instructor

- **Total time:** approximately 90 to 120 minutes
- **Pacing rule:** never explain theory continuously for more than 15–20 minutes. After every concept, students must write or predict code.
- **Pedagogical pattern for every topic:**
  1. **Problem** — a real mini-situation
  2. **Guess** — ask: "What do you expect to happen?"
  3. **Explain** — the shortest rule that fixes the problem
  4. **Code** — live code written in front of the students, step by step
  5. **Challenge** — students solve a small task on their own or in groups
  6. **Review** — discuss the answer and the most common mistake

### Competition and Points

- Award 1 point per correct prediction in the "Guess" phase.
- Award 1–3 points per completed challenge, depending on difficulty.
- Students can earn a "Bug Hunter" badge for each found intentional error.
- Keep a simple tally on a shared board or in the chat.

### Instructor Questions to Ask During the Session

- "What do you expect to see in the console?"
- "Why did JavaScript choose this result?"
- "Which value is a string and which is a number?"
- "What would you change to make this work correctly?"
- "Can you say the rule in your own words?"
- "What is the smallest code that proves your answer?"

---

## Part 0: Warm-Up — The Broken Pizza Receipt (5 minutes)

### Problem

A restaurant website calculates a receipt but the output is wrong.

```javascript
let price = 19.99;
let quantity = 3;
let tax = 8.5;
let discount = 10;

let total = price * quantity + tax - discount;
console.log(total);
```

### Guess

Ask the students: "What do you think the total will be? Is it correct for a real receipt?"

### Explain

The code adds the tax percent (`8.5`) and subtracts the discount percent (`10`) as numbers, not as percentages of the subtotal. We need arithmetic operators **and** the right formula.

### Live Code

```javascript
let price = 19.99;
let quantity = 3;
let taxPercent = 8.5;
let discountPercent = 10;

let subtotal = price * quantity;
let taxAmount = subtotal * taxPercent / 100;
let discountAmount = subtotal * discountPercent / 100;
let total = subtotal + taxAmount - discountAmount;

console.log("Subtotal:", subtotal.toFixed(2));
console.log("Tax:", taxAmount.toFixed(2));
console.log("Discount:", discountAmount.toFixed(2));
console.log("Total:", total.toFixed(2));
```

### Review

- `*` multiplies first because of operator precedence.
- `toFixed(2)` is a preview of what we will see in the Number section.
- The warm-up introduces the core Mini Project at the end: a correct receipt generator.

---

## Part 1: Arithmetic Operators

### 1.1 The Six Basic Operators

#### Problem

You are building a calculator. A student wrote this, but the results are confusing:

```javascript
console.log(10 + 2);
console.log(10 - 2);
console.log(10 * 2);
console.log(10 / 2);
console.log(10 % 2);
console.log(10 ** 2);
```

#### Guess

Ask: "Predict the output of each line. Which one gives the remainder? Which one is the power?"

#### Explain

JavaScript has six arithmetic operators:

| Operator | Name | Example | Result |
| --- | --- | --- | --- |
| `+` | addition | `5 + 3` | `8` |
| `-` | subtraction | `5 - 3` | `2` |
| `*` | multiplication | `5 * 3` | `15` |
| `/` | division | `6 / 3` | `2` |
| `%` | modulus (remainder) | `5 % 3` | `2` |
| `**` | exponentiation | `2 ** 3` | `8` |

#### Live Code

```javascript
// Basic operations
console.log(5 + 3);    // 8
console.log(10 - 4);   // 6
console.log(6 * 7);    // 42
console.log(15 / 3);   // 5
console.log(17 % 5);   // 2
console.log(2 ** 3);   // 8

// Order of operations (PEMDAS)
console.log(2 + 3 * 4);      // 14, multiplication before addition
console.log((2 + 3) * 4);    // 20, parentheses first
console.log(10 / 2 + 3 * 2); // 10, division and multiplication before addition
```

#### Challenge 1.1 — Basic Arithmetic (individual, 3 minutes)

- **Requirement:** Declare `let a = 15` and `let b = 4`. Print all six operations between them.
- **Time limit:** 3 minutes
- **Hints (optional):**
  - Use `console.log(a + b);`
  - For power, the operator is `**`.
  - `a / b` will give `3.75` in JavaScript.

#### Review

Expected output:

```
19
11
60
3.75
3
50625
```

Common mistake: forgetting that `**` is power, not `*` twice. Award 1 point for correct full output.

### 1.2 Division by Zero and Floating-Point Surprises

#### Problem

A beginner writes a discount calculator:

```javascript
console.log(10 / 0);
console.log(0 / 0);
console.log(0.1 + 0.2);
```

#### Guess

Ask: "Will `10 / 0` crash? Will `0.1 + 0.2` be exactly `0.3`?"

#### Explain

- Division by zero does **not** crash in JavaScript; it returns `Infinity`, `-Infinity`, or `NaN`.
- Decimal numbers are stored as binary floating-point values, so some calculations are not perfectly exact.
- `toFixed(n)` turns a number into a string with `n` decimal places to hide the imprecision.

#### Live Code

```javascript
// Division by zero
console.log(10 / 0);       // Infinity
console.log(-10 / 0);      // -Infinity
console.log(0 / 0);        // NaN (Not a Number)

// Floating-point precision
console.log(0.1 + 0.2);                 // 0.30000000000000004
console.log((0.1 + 0.2).toFixed(2));    // "0.30"
```

#### Challenge 1.2 — Price Precision (individual, 3 minutes)

- **Requirement:** Show `0.1 + 0.2 + 0.3` and then show the same result rounded to 2 decimal places.
- **Time limit:** 3 minutes
- **Hints (optional):**
  - You can group the sum in parentheses.
  - Use `.toFixed(2)` on the final result.

#### Review

Expected output looks like `0.6000000000000001` and then `"0.60"`. Emphasize that `toFixed` returns a **string**, not a number.

### 1.3 Order of Operations

#### Problem

A quiz app must score a formula:

```javascript
let result = 10 + 6 / 2 * 3 - 1;
```

#### Guess

Ask: "What is the result? Try to compute it by hand before I run it."

#### Explain

JavaScript follows the standard math order: parentheses, exponents, multiplication and division (left to right), addition and subtraction (left to right).

#### Live Code

```javascript
console.log(10 + 6 / 2 * 3 - 1);     // 18
console.log((10 + 6) / 2 * (3 - 1)); // 16
console.log(((10 + 5) * 2 - 8) / 4); // 5.5
```

#### Challenge 1.3 — Add Parentheses (individual, 4 minutes)

- **Requirement:** Write an expression using the numbers `10`, `5`, `2`, and `4` and the operators `+`, `*`, `-`, and `/` that gives the result `5`.
- **Time limit:** 4 minutes
- **Hints (optional):**
  - One possible answer is `((10 + 5) * 2 - 8) / 4`.
  - Use parentheses to control the order.

#### Review

Accept any correct expression. Discuss the importance of parentheses for clarity and correctness.

---

## Part 2: Unary Plus and Negation

### 2.1 Converting a String to a Number

#### Problem

A user types a price as a string, and the code does not subtract correctly:

```javascript
let price = "100";
let discount = 20;
console.log(price - discount);
console.log(price + discount);
```

#### Guess

Ask: "Both lines use `price` and `discount`. Will both give the same result?"

#### Explain

- Unary plus `+value` converts a value to a number.
- Unary minus `-value` converts a value to a number and then negates it.
- Minus, times, divide, and modulus automatically try to convert strings to numbers.
- Plus does **not** convert: if one side is a string, it concatenates.

#### Live Code

```javascript
// Unary plus
console.log(+"42");        // 42
console.log(+"3.14");      // 3.14
console.log(+"Hello");     // NaN
console.log(+true);        // 1
console.log(+false);       // 0
console.log(+null);        // 0
console.log(+undefined);   // NaN

// Unary negation
console.log(-"42");        // -42
console.log(-"3.14");      // -3.14
console.log(-"Hello");     // NaN
console.log(-(-42));       // 42
```

#### Challenge 2.1 — Quick Converter (individual, 3 minutes)

- **Requirement:** Declare `let input = "25"` and use the unary plus to convert it, then multiply by `4` and log the result.
- **Time limit:** 3 minutes
- **Hints (optional):**
  - `let num = +input;`
  - Then `console.log(num * 4);`

#### Review

Expected output: `100`. The key concept is that `+"25"` is a quick conversion.

### 2.2 Practical Use: Temperature Converter

#### Problem

A weather app receives temperature as a string and must convert Celsius to Fahrenheit.

#### Live Code

```javascript
let celsiusString = "25";
let celsius = +celsiusString;
let fahrenheit = (celsius * 9 / 5) + 32;
console.log(celsius + "C = " + fahrenheit + "F");
// 25C = 77F

let kelvin = celsius + 273.15;
console.log(celsius + "C = " + kelvin + "K");
// 25C = 298.15K
```

#### Challenge 2.2 — Celsius to Fahrenheit (individual, 4 minutes)

- **Requirement:** Write code that takes any Celsius string (for example `"0"`) and prints both the Fahrenheit and Kelvin values.
- **Time limit:** 4 minutes
- **Hints (optional):**
  - Use `+` to convert the string.
  - Formula: `F = C * 9 / 5 + 32`.
  - Formula: `K = C + 273.15`.

#### Review

For `"0"`, expected: `0C = 32F` and `0C = 273.15K`. Award 1 point for correct output.

---

## Part 3: Type Coercion — The Detective Section

### 3.1 Plus with a String

#### Problem

A form sends the age as a string. The developer tries to add `5`:

```javascript
let age = "20";
console.log(age + 5);
```

#### Guess

Ask: "Will the result be `25`, `"25"`, `205`, or an error?"

#### Explain

If one operand of `+` is a string, JavaScript converts the other side to a string and concatenates.

#### Live Code

```javascript
// Number + String = String
console.log(5 + "3");        // "53"
console.log("10" + 20);      // "1020"
console.log(1 + 2 + "3");    // "33" (1+2 first, then concatenation)
console.log("1" + 2 + 3);    // "123" (first "1"+2 becomes "12", then +3)

// Boolean + String = String
console.log(true + "Hello"); // "trueHello"
console.log(false + "Bye");  // "falseBye"
```

#### Challenge 3.1 — Predict the String (individual, 3 minutes)

- **Requirement:** Without running, write the expected output for each line. Then run and check.

```javascript
console.log(10 + "10");
console.log("10" + 10);
console.log(5 + 5 + "5");
console.log("5" + 5 + 5);
```

- **Time limit:** 3 minutes
- **Hints (optional):**
  - Work left to right.
  - As soon as a string appears with `+`, the rest becomes string concatenation.

#### Review

Expected:

```
"1010"
"1010"
"105"
"555"
```

### 3.2 Minus, Times, Divide, and Modulus with Strings

#### Problem

The same developer now uses `-`:

```javascript
console.log("10" - 5);
console.log("10" * "5");
```

#### Guess

Ask: "Now what happens? Does the minus also concatenate?"

#### Explain

For `-`, `*`, `/`, and `%`, JavaScript tries to convert both sides to numbers. This is why `"10" - "5"` becomes `5`.

#### Live Code

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

#### Challenge 3.2 — Coercion Pairs (individual, 4 minutes)

- **Requirement:** For each expression, predict the result and then check it.

```javascript
console.log("8" / "2");
console.log("8" - 2);
console.log("8" + 2);
console.log("8" * "2");
```

- **Time limit:** 4 minutes
- **Hints (optional):**
  - Only `+` with a string causes concatenation.
  - All other operators convert to numbers.

#### Review

Expected: `4`, `6`, `"82"`, `16`.

### 3.3 Coercion Edge Cases

#### Problem

Some values are not what they seem. What is the result of `null + 5` or `undefined + 5`?

#### Guess

Ask students to vote or raise hands for each value: `null`, `undefined`, `true`, `false`, `NaN`.

#### Explain

| Value | Coerced to number | Example |
| --- | --- | --- |
| `null` | `0` | `null + 5` = `5` |
| `undefined` | `NaN` | `undefined + 5` = `NaN` |
| `true` | `1` | `true + 5` = `6` |
| `false` | `0` | `false + 5` = `5` |
| `NaN` | stays `NaN` | `NaN + 5` = `NaN` |

#### Live Code

```javascript
console.log(NaN + 5);         // NaN
console.log(NaN * 5);         // NaN

console.log(null + 5);        // 5
console.log(null - 5);        // -5
console.log(null * 5);        // 0

console.log(undefined + 5);   // NaN
console.log(undefined - 5);   // NaN

console.log(true + 5);        // 6
console.log(false + 5);       // 5
console.log(true * 5);        // 5
console.log(false * 5);       // 0
```

#### Challenge 3.3 — Coercion Table (group, 6 minutes)

- **Requirement:** In groups of 2 or 3, complete a table with the results of `value + 5` and `value * 5` for `null`, `undefined`, `true`, `false`, `NaN`, `"10"`, `"hello"`, `""`.
- **Time limit:** 6 minutes
- **Points:** 2 points for the group with the most correct entries.
- **Hints (optional):**
  - Use `console.log(value + 5, value * 5);` in the browser console.
  - Remember that `undefined` becomes `NaN`.

#### Review

Show the full table on the board. The group with the most correct entries wins 2 points.

---

## Part 4: Assignment Operators

### 4.1 Simple and Compound Assignment

#### Problem

A game score starts at `10`. You must add `5`, multiply by `2`, then subtract `10`. Writing `x = x + 5` every time is long.

#### Live Code

```javascript
let x = 10;

x += 5;   // same as x = x + 5
console.log(x); // 15

x *= 2;   // same as x = x * 2
console.log(x); // 30

x -= 10;  // same as x = x - 10
console.log(x); // 20
```

#### Explain

| Operator | Meaning | Example |
| --- | --- | --- |
| `+=` | add then assign | `x += 5` |
| `-=` | subtract then assign | `x -= 3` |
| `*=` | multiply then assign | `x *= 2` |
| `/=` | divide then assign | `x /= 4` |
| `%=` | modulus then assign | `x %= 3` |
| `**=` | power then assign | `x **= 2` |

#### Live Code — Full Example

```javascript
let y = 10;

y += 5;   // 15
y -= 3;   // 12
y *= 2;   // 24
y /= 4;   // 6
y %= 4;   // 2
y **= 3;  // 8

console.log(y); // 8
```

#### Challenge 4.1 — Assignment Trail (individual, 4 minutes)

- **Requirement:** Start with `let score = 0`. Use only compound assignment operators to reach `score = 100`. You may use `+=`, `-=`, `*=`, `/=`, `%=`, `**=`.
- **Time limit:** 4 minutes
- **Example solution (do not show students):**

```javascript
let score = 0;
score += 100;
```

- **Optional harder path:** use more than one operator.

#### Review

Accept any valid sequence. The simplest is `score += 100;`. Discuss how shorter code is easier to read.

### 4.2 String Concatenation Assignment

#### Problem

Build a message one piece at a time.

#### Live Code

```javascript
let message = "Hello";
message += " ";
message += "World";
message += "!";
console.log(message); // "Hello World!"
```

#### Challenge 4.2 — Build a Sentence (individual, 3 minutes)

- **Requirement:** Start with an empty string and use `+=` to build the sentence `"I love JavaScript."`
- **Time limit:** 3 minutes
- **Hints (optional):**
  - `let sentence = "";`
  - Add words with `+= "word "`.

#### Review

Expected code:

```javascript
let sentence = "";
sentence += "I ";
sentence += "love ";
sentence += "JavaScript.";
console.log(sentence);
```

### 4.3 Assignment with Type Coercion

#### Problem

What happens when the starting value is a string?

```javascript
let x = "10";
x += 5;
console.log(x);

let y = "10";
y -= 5;
console.log(y);
```

#### Guess

Ask: "Will `x` and `y` both be numbers?"

#### Explain

- `+=` with a string uses concatenation.
- `-=` converts the string to a number first.

#### Live Code

```javascript
let a = "10";
a += 5;
console.log(a); // "105"

let b = "10";
b -= 5;
console.log(b); // 5
```

#### Challenge 4.3 — String or Number (individual, 3 minutes)

- **Requirement:** Predict and then run the following. Explain why each result is different.

```javascript
let a = "5";
a *= 2;
console.log(a);

let b = "5";
b += 2;
console.log(b);
```

- **Time limit:** 3 minutes

#### Review

`a` becomes `10` (number), because `*=` converts to number. `b` becomes `"52"` (string), because `+=` with a string concatenates.

---

## Bug Hunt 1

### Problem

The following code has three deliberate JavaScript bugs or surprising results. Ask students to find them.

```javascript
let price = "19.99";
let quantity = 3;
let tax = 8.5;

let subtotal = price * quantity;
let taxAmount = subtotal + tax / 100;
let total = subtotal - taxAmount;

console.log("Subtotal: " + subtotal);
console.log("Tax: " + taxAmount);
console.log("Total: " + total.toFixed(2));

let message = "Total: $" + total;
message += 10;
console.log(message);
```

### What to Ask

- "Why is `taxAmount` wrong?"
- "What should `+= 10` do?"
- "Is `message` a number or a string?"

### Expected Issues

1. `taxAmount` uses `+ tax / 100` instead of `* tax / 100`.
2. `total` uses `-` instead of `+ taxAmount`.
3. `message += 10` concatenates `10` as a string, not adds it.

### Fixed Version (for the instructor)

```javascript
let price = 19.99;
let quantity = 3;
let taxPercent = 8.5;

let subtotal = price * quantity;
let taxAmount = subtotal * taxPercent / 100;
let total = subtotal + taxAmount;

console.log("Subtotal: " + subtotal);
console.log("Tax: " + taxAmount);
console.log("Total: " + total.toFixed(2));

let message = "Total: $" + total.toFixed(2);
console.log(message);
```

### Points

1 point for each found bug. Award a "Bug Hunter" badge to the first student who finds all three.

---

## Part 5: Number and Number Methods

### 5.1 Number Properties

#### Problem

A quiz app must handle very large numbers. What are the limits JavaScript can safely use?

#### Explain

JavaScript numbers are stored as 64-bit floating-point values. There are constants for limits.

#### Live Code

```javascript
console.log(Number.MAX_SAFE_INTEGER);
console.log(Number.MIN_SAFE_INTEGER);
console.log(Number.MAX_VALUE);
console.log(Number.MIN_VALUE);
console.log(Number.POSITIVE_INFINITY);
console.log(Number.NEGATIVE_INFINITY);
console.log(Number.NaN);
```

### 5.2 Number Conversion

#### Problem

User input is always a string. How do we turn it into a number safely?

#### Explain

- `Number(value)` converts the whole value.
- `parseInt(string)` reads the integer from the start of the string.
- `parseFloat(string)` reads the decimal number from the start.
- `parseInt` and `parseFloat` are more forgiving than `Number`.

#### Live Code

```javascript
// Number conversion
console.log(Number("42"));       // 42
console.log(Number("3.14"));     // 3.14
console.log(Number("Hello"));    // NaN
console.log(Number(true));       // 1
console.log(Number(false));      // 0
console.log(Number(null));       // 0
console.log(Number(undefined));  // NaN

// Parsing
console.log(parseInt("42"));      // 42
console.log(parseInt("42px"));    // 42
console.log(parseInt("3.14"));    // 3
console.log(parseInt("Hello"));   // NaN

console.log(parseFloat("3.14"));     // 3.14
console.log(parseFloat("3.14px"));   // 3.14
console.log(parseFloat("Hello"));    // NaN
```

#### Challenge 5.1 — Parse the Data (individual, 4 minutes)

- **Requirement:** Convert these strings to numbers and print them.

```javascript
let width = "150px";
let height = "200.5px";
let opacity = "0.75";
let count = "42";
```

- **Expected outputs:**
  - `width` as integer `150`
  - `height` as float `200.5`
  - `opacity` as float `0.75`
  - `count` as integer `42`

- **Time limit:** 4 minutes
- **Hints (optional):**
  - `parseInt` for `width` and `count`.
  - `parseFloat` for `height` and `opacity`.

#### Review

```javascript
console.log(parseInt(width));
console.log(parseFloat(height));
console.log(parseFloat(opacity));
console.log(parseInt(count));
```

### 5.3 Number Validation

#### Problem

A validator must check if a value is a real integer, a finite number, or NaN.

#### Live Code

```javascript
console.log(Number.isInteger(42));      // true
console.log(Number.isInteger(3.14));    // false
console.log(Number.isInteger("42"));    // false

console.log(Number.isFinite(42));       // true
console.log(Number.isFinite(Infinity)); // false
console.log(Number.isFinite(NaN));      // false

console.log(Number.isNaN(NaN));         // true
console.log(Number.isNaN(42));          // false
console.log(Number.isNaN("Hello"));     // false
```

#### Challenge 5.2 — Is It a Number? (individual, 4 minutes)

- **Requirement:** Test the following values with `Number.isInteger`, `Number.isFinite`, and `Number.isNaN`.

```javascript
let a = 100;
let b = 3.5;
let c = Infinity;
let d = NaN;
let e = "100";
```

- **Time limit:** 4 minutes
- **Hints (optional):**
  - `Number.isNaN` only returns `true` for `NaN`.
  - A string is not an integer even if it looks like one.

#### Review

Discuss that `Number.isInteger` does not coerce strings. `Number.isNaN` is stricter than the global `isNaN`.

### 5.4 Number Formatting

#### Problem

A shop must show prices with exactly two decimals and round grades to a fixed number of digits.

#### Live Code

```javascript
let num = 1234.56789;

console.log(num.toFixed(2));          // "1234.57"
console.log(num.toFixed(0));          // "1235"
console.log(num.toPrecision(4));      // "1235"
console.log(num.toExponential(2));    // "1.23e+3"
console.log(num.toString());          // "1234.56789"
console.log(num.toString(2));         // binary
console.log(num.toString(8));         // octal
console.log(num.toString(16));        // hexadecimal
```

#### Challenge 5.3 — Format a Price (individual, 4 minutes)

- **Requirement:** Format `let price = 9.9876` as:
  1. A dollar amount with 2 decimals.
  2. A number with 3 significant digits.
  3. A string.

- **Time limit:** 4 minutes
- **Hints (optional):**
  - `toFixed(2)` for dollars.
  - `toPrecision(3)` for significant digits.
  - `toString()` for a plain string.

#### Review

```javascript
let price = 9.9876;
console.log(price.toFixed(2));       // "9.99"
console.log(price.toPrecision(3));   // "9.99"
console.log(price.toString());       // "9.9876"
```

---

## Part 6: The Math Object

### 6.1 Rounding and Absolute Value

#### Problem

A student needs to round grades and distances correctly.

#### Live Code

```javascript
console.log(Math.abs(-5));       // 5
console.log(Math.round(4.7));    // 5
console.log(Math.round(4.2));    // 4
console.log(Math.floor(4.9));    // 4
console.log(Math.ceil(4.1));     // 5
console.log(Math.trunc(4.9));    // 4
console.log(Math.trunc(-4.9));   // -4
```

#### Challenge 6.1 — Round This (individual, 4 minutes)

- **Requirement:** For `let score = 78.4`, print the value rounded to the nearest integer, rounded down, and rounded up.
- **Time limit:** 4 minutes

### 6.2 Powers, Roots, and Trigonometry

#### Live Code

```javascript
console.log(Math.pow(2, 3));     // 8
console.log(Math.sqrt(16));      // 4
console.log(Math.cbrt(27));      // 3

console.log(Math.sin(Math.PI / 2)); // 1
console.log(Math.cos(0));           // 1
console.log(Math.tan(Math.PI / 4)); // close to 1
```

### 6.3 Random Numbers

#### Problem

Build a dice roller and a random number picker.

#### Explain

- `Math.random()` returns a decimal from `0` (inclusive) to `1` (exclusive).
- To get a random integer in a range, use `Math.floor`.

#### Live Code

```javascript
console.log(Math.random());                         // 0 to <1
console.log(Math.random() * 10);                    // 0 to <10
console.log(Math.floor(Math.random() * 10));        // 0 to 9

function getRandomInt(min, max) {
  return Math.floor(Math.random() * (max - min + 1)) + min;
}

console.log(getRandomInt(1, 6));   // dice roll 1-6
console.log(getRandomInt(1, 100)); // 1-100
```

#### Challenge 6.2 — Random Integer Function (individual, 5 minutes)

- **Requirement:** Write a function `dice()` that returns a random integer from `1` to `6`.
- **Time limit:** 5 minutes
- **Hints (optional):**
  - `Math.random()` gives `0` to less than `1`.
  - Multiply by `6`, add `1`, then use `Math.floor`.

#### Review

```javascript
function dice() {
  return Math.floor(Math.random() * 6) + 1;
}
console.log(dice());
```

### 6.4 Constants, Max, and Min

#### Live Code

```javascript
console.log(Math.PI);
console.log(Math.E);
console.log(Math.SQRT2);

console.log(Math.max(1, 5, 3));    // 5
console.log(Math.min(1, 5, 3));    // 1
console.log(Math.max(...[1, 5, 3])); // 5
```

#### Challenge 6.3 — Circle Area (individual, 4 minutes)

- **Requirement:** Write a function `circleArea(radius)` that returns the area of a circle rounded to 2 decimals.
- **Time limit:** 4 minutes
- **Hints (optional):**
  - Formula: `Math.PI * radius * radius`.
  - Use `.toFixed(2)` and remember it returns a string.

#### Review

```javascript
function circleArea(radius) {
  return (Math.PI * radius * radius).toFixed(2);
}
console.log(circleArea(5)); // "78.54"
```

---

## Part 7: String Methods

### 7.1 Length, Access, and Basic Search

#### Problem

A user types `"Hello, World!"`. The app needs to know how long it is, what the first and last characters are, and whether it contains `"World"`.

#### Live Code

```javascript
let text = "Hello, World!";

console.log(text.length);              // 13
console.log(text[0]);                  // "H"
console.log(text[text.length - 1]);    // "!"
console.log(text.charAt(7));           // "W"

console.log(text.toUpperCase());       // "HELLO, WORLD!"
console.log(text.toLowerCase());       // "hello, world!"

console.log(text.indexOf("World"));    // 7
console.log(text.indexOf("world"));    // -1
console.log(text.lastIndexOf("o"));    // 8
console.log(text.includes("World"));   // true
console.log(text.startsWith("Hello")); // true
console.log(text.endsWith("!"));       // true
```

#### Challenge 7.1 — Inspect a String (individual, 4 minutes)

- **Requirement:** Declare `let name = "Ada Lovelace"` and print:
  1. The length.
  2. The first and last characters.
  3. The index of the first space.
  4. Whether the name includes `"Love"`.

- **Time limit:** 4 minutes
- **Hints (optional):**
  - `name.indexOf(" ")` finds the space.
  - `name.includes("Love")` is a yes/no check.

#### Review

```javascript
let name = "Ada Lovelace";
console.log(name.length);
console.log(name[0], name[name.length - 1]);
console.log(name.indexOf(" "));
console.log(name.includes("Love"));
```

### 7.2 Extracting Parts

#### Problem

You need to cut the first name and last name from a full name string.

#### Live Code

```javascript
let text = "Hello, World!";

console.log(text.slice(0, 5));      // "Hello"
console.log(text.slice(7));         // "World!"
console.log(text.slice(-6));        // "World!"
console.log(text.slice(0, -1));     // "Hello, World"

console.log(text.substring(0, 5));  // "Hello"
console.log(text.substring(7));     // "World!"

console.log(text.substr(0, 5));     // "Hello"
console.log(text.substr(7, 5));     // "World"
```

#### Challenge 7.2 — First and Last Name (individual, 4 minutes)

- **Requirement:** From `let full = "Grace Hopper"`, extract the first name (`"Grace"`) and the last name (`"Hopper"`) and print them.
- **Time limit:** 4 minutes
- **Hints (optional):**
  - Find the space index.
  - Use `slice` from `0` to the space for the first name.
  - Use `slice(space + 1)` for the last name.

#### Review

```javascript
let full = "Grace Hopper";
let space = full.indexOf(" ");
let first = full.slice(0, space);
let last = full.slice(space + 1);
console.log(first, last);
```

### 7.3 Replacing, Splitting, and Trimming

#### Live Code

```javascript
let text = "Hello, World!";
let newText = text.replace("World", "JavaScript");
console.log(newText); // "Hello, JavaScript!"

let repeatText = "Hello World, Hello World";
let allReplaced = repeatText.replace(/Hello/g, "Hi");
console.log(allReplaced);

let caseText = "Hello WORLD, hello world";
let caseReplaced = caseText.replace(/hello/gi, "Hi");
console.log(caseReplaced);

let csv = "apple,banana,orange,grape";
console.log(csv.split(",")); // ["apple", "banana", "orange", "grape"]

let sentence = "This is a sentence";
console.log(sentence.split(" ")); // ["This", "is", "a", "sentence"]

let padded = "   Hello, World!   ";
console.log("[" + padded.trim() + "]");
console.log("[" + padded.trimStart() + "]");
console.log("[" + padded.trimEnd() + "]");
```

#### Challenge 7.3 — Clean and Split (individual, 5 minutes)

- **Requirement:** Take `let data = "  apple,banana,orange  "` and produce a clean array `["apple", "banana", "orange"]`.
- **Time limit:** 5 minutes
- **Hints (optional):**
  - Trim first, then split.
  - `data.trim().split(",");`

### 7.4 Padding and Repeating

#### Live Code

```javascript
let num = "42";
console.log(num.padStart(5, "0")); // "00042"
console.log(num.padEnd(5, "*"));   // "42***"

let name = "John";
console.log(name.padStart(10, "-")); // "------John"

console.log("Ha".repeat(3)); // "HaHaHa"
```

#### Challenge 7.4 — Receipt ID (individual, 4 minutes)

- **Requirement:** Given `let id = 42`, create an order number string with exactly 6 digits, padded with zeros on the left.
- **Time limit:** 4 minutes
- **Hints (optional):**
  - Use `id.toString().padStart(6, "0");`.

#### Review

```javascript
let id = 42;
let order = id.toString().padStart(6, "0");
console.log(order); // "000042"
```

### 7.5 Practical String Manipulation

#### Live Code

```javascript
// Title case a name
function toTitleCase(str) {
  return str.toLowerCase().split(" ").map(word =>
    word.charAt(0).toUpperCase() + word.slice(1)
  ).join(" ");
}
console.log(toTitleCase("john doe")); // "John Doe"

// Initials
function getInitials(first, last) {
  return (first.charAt(0) + last.charAt(0)).toUpperCase();
}
console.log(getInitials("John", "Doe")); // "JD"

// Truncate text
function truncateText(text, maxLength) {
  if (text.length <= maxLength) return text;
  return text.slice(0, maxLength - 3) + "...";
}
console.log(truncateText("Hello World", 8)); // "Hello..."
```

#### Challenge 7.5 — Name Badge (individual, 5 minutes)

- **Requirement:** Write a function `badge(name)` that takes a messy name like `"  aDA lovelace  "` and returns a clean badge string like `"Ada Lovelace"`.
- **Time limit:** 5 minutes
- **Hints (optional):**
  - `trim()` to remove spaces.
  - `toLowerCase()` and then `split(" ")`.
  - Uppercase the first letter of each word.

#### Review

```javascript
function badge(name) {
  return name.trim().toLowerCase().split(" ").map(word =>
    word.charAt(0).toUpperCase() + word.slice(1)
  ).join(" ");
}
console.log(badge("  aDA lovelace  ")); // "Ada Lovelace"
```

---

## Bug Hunt 2

### Problem

The following code has four deliberate mistakes or surprises. Ask students to find them.

```javascript
let userName = "  john doe  ";
let cleaned = userName.trimStart();
let parts = cleaned.split(" ");
let firstName = parts[0].toUpperCase();
let lastName = parts[1].toLowerCase();
let full = firstName + " " + lastName;

console.log(full);

let price = "19.99$";
let amount = parseFloat(price);
let total = amount + 5;
console.log(total.toFixed(2) + 0);
```

### Issues

1. `trimStart()` only removes leading spaces, so the trailing space remains and `lastName` becomes `"doe "`.
2. `firstName.toUpperCase()` and `lastName.toLowerCase()` are probably not the desired formatting.
3. `parseFloat("19.99$")` returns `19.99`, so this one is fine, but `total.toFixed(2)` returns a string.
4. `"19.99" + 0` concatenates to `"19.990"`.

### Fixed Version (for the instructor)

```javascript
let userName = "  john doe  ";
let cleaned = userName.trim();
let parts = cleaned.split(" ");
let firstName = parts[0].charAt(0).toUpperCase() + parts[0].slice(1).toLowerCase();
let lastName = parts[1].charAt(0).toUpperCase() + parts[1].slice(1).toLowerCase();
let full = firstName + " " + lastName;
console.log(full);

let price = "19.99";
let amount = parseFloat(price);
let total = amount + 5;
console.log(Number(total.toFixed(2)));
```

### Points

1 point per found issue.

---

## Group Challenge: Coercion Detective

- **Time:** 8 minutes
- **Teams:** 2 or 3 students per team
- **Task:** Each team receives a list of 10 expressions. They must write the predicted result and then check it in the console.
- **Scoring:** 1 point per correct prediction. The team with the most points wins.

### Expressions for the Teams

```javascript
console.log("5" + 3);
console.log("5" - 3);
console.log("5" * "2");
console.log(5 + "5" + 5);
console.log("10" / "2" + 1);
console.log(null + 5);
console.log(undefined - 5);
console.log(true + true);
console.log(false * 10);
console.log("hello" * 2);
```

### Instructor Answer Key

```
"53"
2
10
"555"
6
5
NaN
2
0
NaN
```

### Hints (optional)

- Only `+` with a string concatenates.
- `null` is `0`, `undefined` is `NaN`, `true` is `1`, `false` is `0`.

---

## Individual Challenges — Progressive Difficulty

### Level 1: Arithmetic (3 minutes)

- **Requirement:** Print the result of `((10 + 5) * 2 - 8) / 4` and predict it first.
- **Expected:** `5.5`

### Level 2: Coercion (4 minutes)

- **Requirement:** Explain and print the result of `console.log("10" + 5 - 2);`.
- **Expected:** `103` (`"10" + 5` becomes `"105"`, then `"105" - 2` becomes `103` as a number).
- **Hint:** Work left to right and watch when a string appears.

### Level 3: Parsing and Formatting (5 minutes)

- **Requirement:** A user enters `"$29.99"`. Extract the number, add a 10% tax, and print the final price with exactly 2 decimals.
- **Expected:** `"32.99"` (approximately; `29.99 * 1.10 = 32.989`, rounded to `32.99`).
- **Hint:** Use `parseFloat` and `toFixed(2)`.

### Level 4: String Detective (5 minutes)

- **Requirement:** Given `let code = "ORD-2024-00042"`, extract the year (`"2024"`) and the order number (`"00042"`) using `split` and `slice`.
- **Hint:** Use `code.split("-")`.

### Level 5: Random Score (5 minutes)

- **Requirement:** Generate a random exam score between `50` and `100` and print it.
- **Hint:** `Math.floor(Math.random() * (max - min + 1)) + min`.

---

## Mini Project: Receipt and Badge Generator

### Time

20 minutes

### Goal

Combine arithmetic operators, number parsing and formatting, string methods, and user interaction in one small HTML page.

### Requirements for the Students

1. Create a small HTML page with inputs for:
   - Item price
   - Quantity
   - Tax percent
   - Discount percent
   - Customer full name (possibly messy, e.g., "  john DOE  ")

2. When the user clicks a button:
   - Calculate the correct subtotal, tax amount, discount amount, and total.
   - Format all prices to 2 decimals.
   - Clean the customer name to title case.
   - Generate a 6-digit order number padded with zeros.
   - Show a receipt in the page.

3. The receipt must look like this:

```
Order: 000012
Customer: John Doe
Subtotal: $59.97
Tax: $5.10
Discount: -$6.00
Total: $59.07
```

### Time Limit

20 minutes

### Hints (optional)

- Use `parseFloat` for all number inputs.
- `subtotal = price * quantity`
- `taxAmount = subtotal * taxPercent / 100`
- `discountAmount = subtotal * discountPercent / 100`
- `total = subtotal + taxAmount - discountAmount`
- Use `toFixed(2)` for money.
- Use `trim`, `toLowerCase`, `split`, `charAt`, `toUpperCase`, `slice`, and `join` for the name.
- Generate the order number with `Math.floor(Math.random() * 900000) + 100000` or a simple counter.

### Live-Coding Starting Point

Type the HTML and JS in front of the students, stopping after each step so they can copy and understand.

### Starter `index.html`

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Receipt and Badge Generator</title>
  <style>
    body { font-family: Arial, sans-serif; max-width: 500px; margin: 20px auto; }
    label { display: block; margin-top: 10px; }
    input { width: 100%; padding: 5px; }
    button { margin-top: 15px; padding: 10px 20px; }
    #receipt { background: #f5f5f5; padding: 15px; margin-top: 20px; border-radius: 4px; }
  </style>
</head>
<body>
  <h1>Receipt and Badge Generator</h1>
  <label>Price: <input type="number" id="price" value="19.99" step="0.01"></label>
  <label>Quantity: <input type="number" id="quantity" value="3"></label>
  <label>Tax %: <input type="number" id="tax" value="8.5" step="0.1"></label>
  <label>Discount %: <input type="number" id="discount" value="10" step="0.1"></label>
  <label>Customer name: <input type="text" id="name" value="  john DOE  "></label>
  <button id="calc">Generate Receipt</button>
  <div id="receipt"></div>

  <script>
    document.getElementById("calc").addEventListener("click", function () {
      let price = parseFloat(document.getElementById("price").value);
      let quantity = parseFloat(document.getElementById("quantity").value);
      let taxPercent = parseFloat(document.getElementById("tax").value);
      let discountPercent = parseFloat(document.getElementById("discount").value);
      let rawName = document.getElementById("name").value;

      let subtotal = price * quantity;
      let taxAmount = subtotal * taxPercent / 100;
      let discountAmount = subtotal * discountPercent / 100;
      let total = subtotal + taxAmount - discountAmount;

      let orderId = Math.floor(Math.random() * 900000) + 100000;
      let orderIdString = orderId.toString().padStart(6, "0");

      let cleanName = rawName.trim().toLowerCase().split(" ").map(word =>
        word.charAt(0).toUpperCase() + word.slice(1)
      ).join(" ");

      let receipt = `
        <p><strong>Order:</strong> ${orderIdString}</p>
        <p><strong>Customer:</strong> ${cleanName}</p>
        <p><strong>Subtotal:</strong> $${subtotal.toFixed(2)}</p>
        <p><strong>Tax:</strong> $${taxAmount.toFixed(2)}</p>
        <p><strong>Discount:</strong> -$${discountAmount.toFixed(2)}</p>
        <p><strong>Total:</strong> $${total.toFixed(2)}</p>
      `;

      document.getElementById("receipt").innerHTML = receipt;
    });
  </script>
</body>
</html>
```

### Review Questions for the Mini Project

- "What happens if the customer types a name with three words?"
- "Why did we use `parseFloat` for `quantity` instead of `parseInt`?"
- "What does `toFixed(2)` return: a number or a string?"

---

## Trainer Solutions — Do Not Show Until Students Try

These are the reference answers for all challenges in this session.

### Challenge 1.1

```javascript
let a = 15;
let b = 4;
console.log(a + b);   // 19
console.log(a - b);   // 11
console.log(a * b);   // 60
console.log(a / b);   // 3.75
console.log(a % b);   // 3
console.log(a ** b);  // 50625
```

### Challenge 1.2

```javascript
let result = 0.1 + 0.2 + 0.3;
console.log(result);
console.log(result.toFixed(2));
```

### Challenge 1.3

Any valid expression giving `5`, for example:

```javascript
console.log(((10 + 5) * 2 - 8) / 4); // 5.5? No, this is 5.5. Use ((10 + 5) * 2 - 20) / 4)
```

A valid one is:

```javascript
console.log((10 + 5) / 3); // 5
```

### Challenge 2.1

```javascript
let input = "25";
let num = +input;
console.log(num * 4); // 100
```

### Challenge 2.2

```javascript
let celsiusString = "0";
let celsius = +celsiusString;
let fahrenheit = celsius * 9 / 5 + 32;
let kelvin = celsius + 273.15;
console.log(celsius + "C = " + fahrenheit + "F");
console.log(celsius + "C = " + kelvin + "K");
```

### Challenge 3.1

```
"1010"
"1010"
"105"
"555"
```

### Challenge 3.2

```
4
6
"82"
16
```

### Challenge 4.1

```javascript
let score = 0;
score += 100;
console.log(score); // 100
```

### Challenge 4.2

```javascript
let sentence = "";
sentence += "I ";
sentence += "love ";
sentence += "JavaScript.";
console.log(sentence);
```

### Challenge 4.3

```javascript
let a = "5";
a *= 2;
console.log(a); // 10

let b = "5";
b += 2;
console.log(b); // "52"
```

### Challenge 5.1

```javascript
let width = "150px";
let height = "200.5px";
let opacity = "0.75";
let count = "42";
console.log(parseInt(width));
console.log(parseFloat(height));
console.log(parseFloat(opacity));
console.log(parseInt(count));
```

### Challenge 6.2

```javascript
function dice() {
  return Math.floor(Math.random() * 6) + 1;
}
console.log(dice());
```

### Challenge 6.3

```javascript
function circleArea(radius) {
  return (Math.PI * radius * radius).toFixed(2);
}
console.log(circleArea(5)); // "78.54"
```

### Challenge 7.1

```javascript
let name = "Ada Lovelace";
console.log(name.length);
console.log(name[0], name[name.length - 1]);
console.log(name.indexOf(" "));
console.log(name.includes("Love"));
```

### Challenge 7.2

```javascript
let full = "Grace Hopper";
let space = full.indexOf(" ");
let first = full.slice(0, space);
let last = full.slice(space + 1);
console.log(first, last);
```

### Challenge 7.5

```javascript
function badge(name) {
  return name.trim().toLowerCase().split(" ").map(word =>
    word.charAt(0).toUpperCase() + word.slice(1)
  ).join(" ");
}
console.log(badge("  aDA lovelace  "));
```

---

## Review Questions

1. What is the result of `"10" + 5`?
   - [ ] 15
   - [x] "105"
   - [ ] NaN
   - [ ] Error

2. Which operator converts a string to a number?
   - [ ] `-`
   - [x] `+`
   - [ ] `*`
   - [ ] `/`

3. What does `Math.floor(4.9)` return?
   - [x] 4
   - [ ] 5
   - [ ] 4.9
   - [ ] 4.5

4. What is the result of `parseInt("42px")`?
   - [x] 42
   - [ ] NaN
   - [ ] "42px"
   - [ ] Error

5. Which method rounds a number to a specified number of decimal places?
   - [ ] Math.round()
   - [x] toFixed()
   - [ ] Math.floor()
   - [ ] toPrecision()

6. What does `"Hello".slice(0, 3)` return?
   - [x] "Hel"
   - [ ] "ello"
   - [ ] "llo"
   - [ ] "Hello"

7. What is the result of `true + 5`?
   - [x] 6
   - [ ] "true5"
   - [ ] NaN
   - [ ] 5

8. Which method removes whitespace from both ends of a string?
   - [ ] trimStart()
   - [ ] trimEnd()
   - [x] trim()
   - [ ] strip()

9. What does `Math.random()` return?
   - [ ] A random integer
   - [x] A number between 0 and 1
   - [ ] A number between 1 and 10
   - [ ] A random string

10. What is the result of `"10" - 5`?
    - [ ] "105"
    - [x] 5
    - [ ] NaN
    - [ ] Error

---

## Additional Resources

- [MDN: Arithmetic Operators](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Arithmetic_Operators)
- [MDN: Number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number)
- [MDN: Math](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math)
- [MDN: String Methods](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)
- [JavaScript.info: Operators](https://javascript.info/operators)
- [JavaScript.info: Numbers](https://javascript.info/number)
- [JavaScript.info: Strings](https://javascript.info/string)

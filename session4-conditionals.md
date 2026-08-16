# Session 4: Conditionals

## 📚 Theory (1h)

### Comparison Operators

Comparison operators compare two values and return a boolean result (`true` or `false`).

#### Equality Operators

| Operator | Name | Description | Example |
|----------|------|-------------|---------|
| `==` | Loose equality | Compares values after type coercion | `5 == "5"` → `true` |
| `===` | Strict equality | Compares values and types | `5 === "5"` → `false` |
| `!=` | Loose inequality | Returns true if values are not equal (with coercion) | `5 != "5"` → `false` |
| `!==` | Strict inequality | Returns true if values or types are not equal | `5 !== "5"` → `true` |

**Examples:**
```javascript
// Loose equality (with type coercion)
console.log(5 == "5");        // true
console.log(5 == 5);          // true
console.log(null == undefined); // true
console.log(0 == false);      // true
console.log("" == false);     // true

// Strict equality (no type coercion)
console.log(5 === "5");       // false
console.log(5 === 5);         // true
console.log(null === undefined); // false
console.log(0 === false);     // false
console.log("" === false);    // false

// Inequality operators
console.log(5 != "5");        // false
console.log(5 !== "5");       // true
```

**Best Practice:** Always use `===` and `!==` unless you specifically need type coercion.

#### Relational Operators

| Operator | Name | Description | Example |
|----------|------|-------------|---------|
| `>` | Greater than | Returns true if left operand is greater | `5 > 3` → `true` |
| `<` | Less than | Returns true if left operand is smaller | `5 < 3` → `false` |
| `>=` | Greater than or equal | Returns true if left operand is greater or equal | `5 >= 5` → `true` |
| `<=` | Less than or equal | Returns true if left operand is smaller or equal | `5 <= 5` → `true` |

**Examples:**
```javascript
console.log(5 > 3);           // true
console.log(5 < 3);           // false
console.log(5 >= 5);          // true
console.log(5 <= 5);          // true
console.log("10" > 5);        // true (string converted to number)
console.log("a" > "b");       // false (alphabetical comparison)
console.log("apple" > "banana"); // false
```

#### String Comparison

Strings are compared character by character using Unicode values.

```javascript
console.log("a" < "b");       // true
console.log("A" < "a");       // true (uppercase comes before lowercase)
console.log("apple" < "banana"); // true
console.log("10" < "2");      // true (string comparison, not numeric)
console.log("10" > 2);        // true (string "10" converted to number 10)
```

### Logical Operators

Logical operators are used to combine multiple conditions.

| Operator | Name | Description | Example |
|----------|------|-------------|---------|
| `&&` | Logical AND | Returns true if both operands are true | `true && true` → `true` |
| `\|\|` | Logical OR | Returns true if at least one operand is true | `true \|\| false` → `true` |
| `!` | Logical NOT | Returns the opposite boolean value | `!true` → `false` |

#### Logical AND (`&&`)

Returns the first falsy value or the last truthy value.

```javascript
// Boolean logic
console.log(true && true);    // true
console.log(true && false);   // false
console.log(false && true);   // false
console.log(false && false);  // false

// Short-circuit evaluation
console.log(true && "Hello"); // "Hello"
console.log(false && "Hello"); // false (second operand not evaluated)
console.log(5 && 10);         // 10
console.log(0 && 10);         // 0

// Practical use
let user = { name: "John", age: 30 };
if (user && user.name) {
    console.log("User name:", user.name);
}
```

#### Logical OR (`||`)

Returns the first truthy value or the last falsy value.

```javascript
// Boolean logic
console.log(true || true);    // true
console.log(true || false);   // true
console.log(false || true);   // true
console.log(false || false);  // false

// Short-circuit evaluation
console.log(true || "Hello"); // true (second operand not evaluated)
console.log(false || "Hello"); // "Hello"
console.log(5 || 10);         // 5
console.log(0 || 10);         // 10

// Practical use - default values
let name = userName || "Anonymous";
let count = itemCount || 0;
```

#### Logical NOT (`!`)

Returns the opposite boolean value.

```javascript
console.log(!true);           // false
console.log(!false);          // true
console.log(!0);              // true
console.log(!"");             // true
console.log(!null);           // true
console.log(!undefined);      // true
console.log(!"Hello");        // false
console.log(!10);             // false

// Double negation (converts to boolean)
console.log(!!"Hello");       // true
console.log(!!0);             // false
console.log(!!null);          // false
```

#### Operator Precedence

Logical operators have specific precedence: `!` > `&&` > `||`

```javascript
console.log(true || false && false);  // true (&& evaluated first)
console.log((true || false) && false); // false (parentheses change order)
console.log(!true && false);           // false (! evaluated first)
```

### if/else Statements

Conditional statements execute different code based on different conditions.

#### Basic if Statement

```javascript
if (condition) {
    // code to execute if condition is true
}

let age = 18;
if (age >= 18) {
    console.log("You are an adult");
}
```

#### if/else Statement

```javascript
if (condition) {
    // code to execute if condition is true
} else {
    // code to execute if condition is false
}

let age = 15;
if (age >= 18) {
    console.log("You are an adult");
} else {
    console.log("You are a minor");
}
```

#### if/else if/else Statement

```javascript
if (condition1) {
    // code if condition1 is true
} else if (condition2) {
    // code if condition2 is true
} else {
    // code if all conditions are false
}

let score = 85;
if (score >= 90) {
    console.log("Grade: A");
} else if (score >= 80) {
    console.log("Grade: B");
} else if (score >= 70) {
    console.log("Grade: C");
} else if (score >= 60) {
    console.log("Grade: D");
} else {
    console.log("Grade: F");
}
```

#### Multiple Conditions

```javascript
let age = 25;
let hasLicense = true;

if (age >= 18 && hasLicense) {
    console.log("You can drive");
} else if (age >= 18 && !hasLicense) {
    console.log("You need a license to drive");
} else {
    console.log("You are too young to drive");
}
```

### Nested if Statements

You can nest if statements inside other if statements for more complex logic.

```javascript
let age = 25;
let hasLicense = true;
let hasCar = false;

if (age >= 18) {
    if (hasLicense) {
        if (hasCar) {
            console.log("You can drive your car");
        } else {
            console.log("You have a license but no car");
        }
    } else {
        console.log("You need to get a license");
    }
} else {
    console.log("You are too young to drive");
}
```

**Best Practice:** Avoid deep nesting. Use early returns or logical operators to simplify.

```javascript
// Better approach using early returns
function checkDrivingEligibility(age, hasLicense, hasCar) {
    if (age < 18) {
        return "You are too young to drive";
    }
    
    if (!hasLicense) {
        return "You need to get a license";
    }
    
    if (!hasCar) {
        return "You have a license but no car";
    }
    
    return "You can drive your car";
}
```

### Ternary Operator

The ternary operator is a concise way to write simple if/else statements.

#### Syntax

```javascript
condition ? expressionIfTrue : expressionIfFalse
```

#### Basic Usage

```javascript
let age = 18;
let message = age >= 18 ? "You are an adult" : "You are a minor";
console.log(message);
```

#### Multiple Ternary Operators (Nested)

```javascript
let score = 85;
let grade = score >= 90 ? "A" : 
           score >= 80 ? "B" : 
           score >= 70 ? "C" : 
           score >= 60 ? "D" : "F";
console.log("Grade:", grade);
```

#### Ternary with Function Calls

```javascript
function greet(name) {
    return name ? `Hello, ${name}!` : "Hello, Guest!";
}

console.log(greet("John"));  // "Hello, John!"
console.log(greet(null));    // "Hello, Guest!"
```

#### Best Practices

```javascript
// Good - simple and readable
let max = a > b ? a : b;

// Avoid - too complex
let result = condition1 ? (condition2 ? value1 : value2) : (condition3 ? value3 : value4);

// Better to use if/else for complex logic
if (condition1) {
    if (condition2) {
        result = value1;
    } else {
        result = value2;
    }
} else {
    if (condition3) {
        result = value3;
    } else {
        result = value4;
    }
}
```

### Nullish Coalescing Operator

The nullish coalescing operator (`??`) returns the right-hand operand when the left-hand operand is `null` or `undefined`.

#### Syntax

```javascript
leftOperand ?? rightOperand
```

#### Basic Usage

```javascript
let name = null;
let displayName = name ?? "Anonymous";
console.log(displayName); // "Anonymous"

let age = 0;
let displayAge = age ?? 18;
console.log(displayAge); // 0 (not 18, because 0 is not null/undefined)
```

#### Comparison with Logical OR (`||`)

```javascript
// Logical OR - returns first truthy value
let count1 = 0;
let result1 = count1 || 10;  // 10 (0 is falsy)

// Nullish coalescing - returns right side only for null/undefined
let count2 = 0;
let result2 = count2 ?? 10;  // 0 (0 is not null/undefined)

// More examples
let value1 = null;
console.log(value1 || "default");  // "default"
console.log(value1 ?? "default");  // "default"

let value2 = "";
console.log(value2 || "default");  // "default" (empty string is falsy)
console.log(value2 ?? "default");  // "" (empty string is not null/undefined)

let value3 = false;
console.log(value3 || "default");  // "default" (false is falsy)
console.log(value3 ?? "default");  // false (false is not null/undefined)
```

#### Practical Use Cases

```javascript
// Function parameters with default values
function createUser(name, age) {
    return {
        name: name ?? "Anonymous",
        age: age ?? 0
    };
}

console.log(createUser("John", 30));  // {name: "John", age: 30}
console.log(createUser(null, null)); // {name: "Anonymous", age: 0}

// API response handling
let userData = {
    name: "John",
    email: null,
    age: 0
};

let userEmail = userData.email ?? "No email provided";
let userAge = userData.age ?? "Age not provided";

console.log("Email:", userEmail); // "No email provided"
console.log("Age:", userAge);     // 0
```

#### Optional Chaining with Nullish Coalescing

```javascript
let user = {
    profile: {
        name: "John"
    }
};

let userName = user?.profile?.name ?? "Anonymous";
let userEmail = user?.profile?.email ?? "No email";

console.log(userName); // "John"
console.log(userEmail); // "No email"
```

---

## 💻 Practical (1.5h)

### Exercise 1: Comparison Operators

```javascript
// Exercise 1.1: Equality operators
console.log("=== Equality Operators ===");

console.log("5 == '5':", 5 == "5");         // true
console.log("5 === '5':", 5 === "5");       // false
console.log("5 != '5':", 5 != "5");         // false
console.log("5 !== '5':", 5 !== "5");       // true

console.log("null == undefined:", null == undefined); // true
console.log("null === undefined:", null === undefined); // false

console.log("0 == false:", 0 == false);     // true
console.log("0 === false:", 0 === false);   // false

// Exercise 1.2: Relational operators
console.log("\n=== Relational Operators ===");

console.log("5 > 3:", 5 > 3);               // true
console.log("5 < 3:", 5 < 3);               // false
console.log("5 >= 5:", 5 >= 5);             // true
console.log("5 <= 5:", 5 <= 5);             // true

console.log("'10' > 5:", "10" > 5);         // true
console.log("'a' > 'b':", "a" > "b");       // false
console.log("'apple' > 'banana':", "apple" > "banana"); // false

// Exercise 1.3: String comparison
console.log("\n=== String Comparison ===");

let word1 = "apple";
let word2 = "banana";
let word3 = "Apple";

console.log(`"${word1}" < "${word2}":`, word1 < word2); // true
console.log(`"${word1}" > "${word3}":`, word1 > word3); // true
console.log(`"${word3}" < "${word1}":`, word3 < word1); // true
```

### Exercise 2: Logical Operators

```javascript
// Exercise 2.1: Logical AND
console.log("=== Logical AND ===");

console.log("true && true:", true && true);     // true
console.log("true && false:", true && false);   // false
console.log("false && true:", false && true);   // false
console.log("false && false:", false && false); // false

console.log("true && 'Hello':", true && "Hello"); // "Hello"
console.log("false && 'Hello':", false && "Hello"); // false
console.log("5 && 10:", 5 && 10);               // 10
console.log("0 && 10:", 0 && 10);               // 0

// Exercise 2.2: Logical OR
console.log("\n=== Logical OR ===");

console.log("true || true:", true || true);     // true
console.log("true || false:", true || false);   // true
console.log("false || true:", false || true);   // true
console.log("false || false:", false || false); // false

console.log("true || 'Hello':", true || "Hello"); // true
console.log("false || 'Hello':", false || "Hello"); // "Hello"
console.log("5 || 10:", 5 || 10);               // 5
console.log("0 || 10:", 0 || 10);               // 10

// Exercise 2.3: Logical NOT
console.log("\n=== Logical NOT ===");

console.log("!true:", !true);                   // false
console.log("!false:", !false);                 // true
console.log("!0:", !0);                         // true
console.log("!10:", !10);                       // false
console.log("!!'Hello':", !!"Hello");           // true
console.log("!!0:", !!0);                       // false

// Exercise 2.4: Practical examples
console.log("\n=== Practical Examples ===");

let user = { name: "John", age: 30, isAdmin: false };

if (user && user.name) {
    console.log("User name:", user.name);
}

let displayName = user.name || "Anonymous";
console.log("Display name:", displayName);

let canAccess = user.isAdmin || user.age >= 18;
console.log("Can access:", canAccess);
```

### Exercise 3: if/else Statements

```javascript
// Exercise 3.1: Basic if/else
console.log("=== Basic if/else ===");

let age = 20;
if (age >= 18) {
    console.log("You are an adult");
} else {
    console.log("You are a minor");
}

// Exercise 3.2: if/else if/else
console.log("\n=== if/else if/else ===");

let score = 75;
if (score >= 90) {
    console.log("Grade: A");
} else if (score >= 80) {
    console.log("Grade: B");
} else if (score >= 70) {
    console.log("Grade: C");
} else if (score >= 60) {
    console.log("Grade: D");
} else {
    console.log("Grade: F");
}

// Exercise 3.3: Multiple conditions
console.log("\n=== Multiple Conditions ===");

let temperature = 25;
let isRaining = false;

if (temperature > 30 && !isRaining) {
    console.log("It's hot and sunny - perfect for the beach!");
} else if (temperature > 30 && isRaining) {
    console.log("It's hot but raining - stay indoors!");
} else if (temperature <= 30 && !isRaining) {
    console.log("It's pleasant weather - great for a walk!");
} else {
    console.log("It's cool and raining - bring an umbrella!");
}

// Exercise 3.4: Nested if
console.log("\n=== Nested if ===");

let hasTicket = true;
let hasID = false;
let isVIP = true;

if (hasTicket) {
    if (hasID) {
        console.log("Welcome to the event!");
    } else if (isVIP) {
        console.log("VIP entry without ID - welcome!");
    } else {
        console.log("You need ID to enter");
    }
} else {
    console.log("You need a ticket to enter");
}
```

### Exercise 4: Ternary Operator

```javascript
// Exercise 4.1: Basic ternary
console.log("=== Basic Ternary ===");

let age = 20;
let message = age >= 18 ? "Adult" : "Minor";
console.log("Age:", age, "- Status:", message);

// Exercise 4.2: Nested ternary
console.log("\n=== Nested Ternary ===");

let score = 85;
let grade = score >= 90 ? "A" : 
           score >= 80 ? "B" : 
           score >= 70 ? "C" : 
           score >= 60 ? "D" : "F";
console.log("Score:", score, "- Grade:", grade);

// Exercise 4.3: Ternary with functions
console.log("\n=== Ternary with Functions ===");

function getGreeting(timeOfDay) {
    return timeOfDay === "morning" ? "Good morning!" :
           timeOfDay === "afternoon" ? "Good afternoon!" :
           timeOfDay === "evening" ? "Good evening!" :
           "Hello!";
}

console.log(getGreeting("morning"));
console.log(getGreeting("afternoon"));
console.log(getGreeting("evening"));
console.log(getGreeting("night"));

// Exercise 4.4: Practical examples
console.log("\n=== Practical Examples ===");

let num = 15;
let parity = num % 2 === 0 ? "even" : "odd";
console.log(`${num} is ${parity}`);

let price = 100;
let discount = price > 50 ? price * 0.1 : 0;
console.log(`Price: $${price}, Discount: $${discount}`);

let isLoggedIn = true;
let buttonLabel = isLoggedIn ? "Logout" : "Login";
console.log("Button label:", buttonLabel);
```

### Exercise 5: Nullish Coalescing

```javascript
// Exercise 5.1: Basic nullish coalescing
console.log("=== Basic Nullish Coalescing ===");

let name1 = null;
let displayName1 = name1 ?? "Anonymous";
console.log("null ?? 'Anonymous':", displayName1);

let name2 = undefined;
let displayName2 = name2 ?? "Anonymous";
console.log("undefined ?? 'Anonymous':", displayName2);

let name3 = "John";
let displayName3 = name3 ?? "Anonymous";
console.log("'John' ?? 'Anonymous':", displayName3);

// Exercise 5.2: Comparison with logical OR
console.log("\n=== Comparison with Logical OR ===");

let value1 = 0;
console.log("0 || 10:", value1 || 10);    // 10
console.log("0 ?? 10:", value1 ?? 10);    // 0

let value2 = "";
console.log("'' || 'default':", value2 || "default");  // "default"
console.log("'' ?? 'default':", value2 ?? "default");  // ""

let value3 = false;
console.log("false || 'default':", value3 || "default");  // "default"
console.log("false ?? 'default':", value3 ?? "default");  // false

// Exercise 5.3: Practical use cases
console.log("\n=== Practical Use Cases ===");

function processUserData(user) {
    return {
        name: user.name ?? "Anonymous",
        email: user.email ?? "No email",
        age: user.age ?? 0,
        isActive: user.isActive ?? false
    };
}

let user1 = { name: "John", email: "john@example.com", age: 30, isActive: true };
let user2 = { name: null, email: null, age: null, isActive: null };

console.log("User 1:", processUserData(user1));
console.log("User 2:", processUserData(user2));

// Exercise 5.4: Optional chaining with nullish coalescing
console.log("\n=== Optional Chaining ===");

let data = {
    user: {
        profile: {
            name: "John",
            email: null
        }
    }
};

let userName = data?.user?.profile?.name ?? "No name";
let userEmail = data?.user?.profile?.email ?? "No email";
let userPhone = data?.user?.profile?.phone ?? "No phone";

console.log("Name:", userName);
console.log("Email:", userEmail);
console.log("Phone:", userPhone);
```

### Exercise 6: Switch Statement

```javascript
// Exercise 6.1: Basic switch
console.log("=== Basic Switch ===");

let day = "Monday";
let dayType;

switch (day) {
    case "Monday":
    case "Tuesday":
    case "Wednesday":
    case "Thursday":
    case "Friday":
        dayType = "Weekday";
        break;
    case "Saturday":
    case "Sunday":
        dayType = "Weekend";
        break;
    default:
        dayType = "Invalid day";
}

console.log(`${day} is a ${dayType}`);

// Exercise 6.2: Switch with default
console.log("\n=== Switch with Default ===");

let grade = "B";
let message;

switch (grade) {
    case "A":
        message = "Excellent!";
        break;
    case "B":
        message = "Good job!";
        break;
    case "C":
        message = " satisfactory";
        break;
    case "D":
        message = "Needs improvement";
        break;
    case "F":
        message = "Fail";
        break;
    default:
        message = "Invalid grade";
}

console.log(`Grade ${grade}: ${message}`);

// Exercise 6.3: Switch without break (fallthrough)
console.log("\n=== Switch Fallthrough ===");

let month = 2;
let season;

switch (month) {
    case 12:
    case 1:
    case 2:
        season = "Winter";
        break;
    case 3:
    case 4:
    case 5:
        season = "Spring";
        break;
    case 6:
    case 7:
    case 8:
        season = "Summer";
        break;
    case 9:
    case 10:
    case 11:
        season = "Fall";
        break;
    default:
        season = "Invalid month";
}

console.log(`Month ${month} is in ${season}`);
```

### Exercise 7: Decision-Based Mini Programs

#### Grade Calculator

```javascript
// Exercise 7.1: Grade Calculator
console.log("=== Grade Calculator ===");

function calculateGrade(score) {
    if (score < 0 || score > 100) {
        return "Invalid score (must be 0-100)";
    }
    
    if (score >= 90) return "A";
    if (score >= 80) return "B";
    if (score >= 70) return "C";
    if (score >= 60) return "D";
    return "F";
}

function getGradeDetails(score) {
    let grade = calculateGrade(score);
    let message;
    
    switch (grade) {
        case "A":
            message = "Outstanding performance!";
            break;
        case "B":
            message = "Very good performance!";
            break;
        case "C":
            message = "Satisfactory performance.";
            break;
        case "D":
            message = "Needs improvement.";
            break;
        case "F":
            message = "Fail - needs significant improvement.";
            break;
        default:
            message = "Invalid score.";
    }
    
    return { grade, message };
}

// Test the grade calculator
let testScores = [95, 85, 75, 65, 55, 105, -5];
testScores.forEach(score => {
    let result = getGradeDetails(score);
    console.log(`Score ${score}: ${result.grade} - ${result.message}`);
});
```

#### Temperature Converter

```javascript
// Exercise 7.2: Temperature Converter
console.log("\n=== Temperature Converter ===");

function convertTemperature(value, fromUnit, toUnit) {
    if (fromUnit === toUnit) {
        return value;
    }
    
    let celsius;
    
    // Convert to Celsius first
    switch (fromUnit) {
        case "C":
            celsius = value;
            break;
        case "F":
            celsius = (value - 32) * 5/9;
            break;
        case "K":
            celsius = value - 273.15;
            break;
        default:
            return "Invalid fromUnit";
    }
    
    // Convert from Celsius to target unit
    switch (toUnit) {
        case "C":
            return celsius.toFixed(2);
        case "F":
            return (celsius * 9/5 + 32).toFixed(2);
        case "K":
            return (celsius + 273.15).toFixed(2);
        default:
            return "Invalid toUnit";
    }
}

// Test the converter
console.log("0°C to F:", convertTemperature(0, "C", "F"));
console.log("32°F to C:", convertTemperature(32, "F", "C"));
console.log("273.15K to C:", convertTemperature(273.15, "K", "C"));
console.log("100°C to K:", convertTemperature(100, "C", "K"));
```

#### Shopping Discount Calculator

```javascript
// Exercise 7.3: Shopping Discount Calculator
console.log("\n=== Shopping Discount Calculator ===");

function calculateDiscount(totalAmount, customerType) {
    let discountRate;
    
    switch (customerType) {
        case "VIP":
            discountRate = 0.20; // 20% discount
            break;
        case "Premium":
            discountRate = 0.15; // 15% discount
            break;
        case "Regular":
            discountRate = 0.10; // 10% discount
            break;
        case "Guest":
            discountRate = 0.05; // 5% discount
            break;
        default:
            discountRate = 0;
    }
    
    // Additional discount for large purchases
    if (totalAmount > 1000) {
        discountRate += 0.05; // Additional 5%
    }
    
    // Cap discount at 25%
    discountRate = Math.min(discountRate, 0.25);
    
    let discountAmount = totalAmount * discountRate;
    let finalAmount = totalAmount - discountAmount;
    
    return {
        originalAmount: totalAmount,
        discountRate: (discountRate * 100).toFixed(1) + "%",
        discountAmount: discountAmount.toFixed(2),
        finalAmount: finalAmount.toFixed(2)
    };
}

// Test the discount calculator
let purchases = [
    { amount: 500, type: "Regular" },
    { amount: 1500, type: "VIP" },
    { amount: 800, type: "Premium" },
    { amount: 200, type: "Guest" }
];

purchases.forEach(purchase => {
    let result = calculateDiscount(purchase.amount, purchase.type);
    console.log(`${purchase.type} ($${purchase.amount}):`);
    console.log(`  Discount: ${result.discountRate}`);
    console.log(`  Final: $${result.finalAmount}`);
});
```

#### Traffic Light Simulator

```javascript
// Exercise 7.4: Traffic Light Simulator
console.log("\n=== Traffic Light Simulator ===");

function trafficLightAction(light) {
    let action;
    
    switch (light.toLowerCase()) {
        case "red":
            action = "STOP";
            break;
        case "yellow":
            action = "SLOW DOWN";
            break;
        case "green":
            action = "GO";
            break;
        case "flashing red":
            action = "STOP (then proceed when safe)";
            break;
        case "flashing yellow":
            action = "PROCEED WITH CAUTION";
            break;
        default:
            action = "INVALID LIGHT";
    }
    
    return action;
}

function getTrafficLightAdvice(light) {
    let action = trafficLightAction(light);
    let advice;
    
    if (action === "STOP") {
        advice = "Come to a complete stop before the intersection.";
    } else if (action === "SLOW DOWN") {
        advice = "Prepare to stop. Do not enter the intersection if you can stop safely.";
    } else if (action === "GO") {
        advice = "Proceed through the intersection if it's safe.";
    } else if (action === "STOP (then proceed when safe)") {
        advice = "Treat as a stop sign. Stop, then proceed when safe.";
    } else if (action === "PROCEED WITH CAUTION") {
        advice = "Slow down and be prepared to stop for pedestrians.";
    } else {
        advice = "Invalid traffic light signal.";
    }
    
    return { action, advice };
}

// Test the traffic light simulator
let lights = ["Red", "Yellow", "Green", "Flashing Red", "Flashing Yellow", "Blue"];
lights.forEach(light => {
    let result = getTrafficLightAdvice(light);
    console.log(`${light}: ${result.action}`);
    console.log(`  Advice: ${result.advice}`);
});
```

#### Login System

```javascript
// Exercise 7.5: Login System
console.log("\n=== Login System ===");

function authenticateUser(username, password, role) {
    // Simulated user database
    const users = {
        "admin": { password: "admin123", role: "admin" },
        "user": { password: "user123", role: "user" },
        "guest": { password: "guest123", role: "guest" }
    };
    
    let user = users[username];
    
    if (!user) {
        return { success: false, message: "User not found" };
    }
    
    if (user.password !== password) {
        return { success: false, message: "Incorrect password" };
    }
    
    if (user.role !== role) {
        return { success: false, message: "Role mismatch" };
    }
    
    return { success: true, message: "Login successful", role: user.role };
}

function getAccessLevel(role) {
    switch (role) {
        case "admin":
            return "Full access to all features";
        case "user":
            return "Access to user features";
        case "guest":
            return "Limited access to guest features";
        default:
            return "No access";
    }
}

// Test the login system
let loginAttempts = [
    { username: "admin", password: "admin123", role: "admin" },
    { username: "user", password: "wrongpass", role: "user" },
    { username: "guest", password: "guest123", role: "guest" },
    { username: "unknown", password: "test", role: "user" }
];

loginAttempts.forEach(attempt => {
    let result = authenticateUser(attempt.username, attempt.password, attempt.role);
    console.log(`Login attempt for ${attempt.username}:`);
    console.log(`  ${result.message}`);
    
    if (result.success) {
        console.log(`  Access: ${getAccessLevel(result.role)}`);
    }
});
```

### Exercise 8: Complete Working Example

**Complete script.js:**
```javascript
// Session 4: Conditionals
// This script demonstrates conditional statements and operators

console.log("=== Session 4: Conditionals ===");

// 1. Comparison operators
console.log("\n--- Comparison Operators ---");
console.log("5 == '5':", 5 == "5");
console.log("5 === '5':", 5 === "5");
console.log("5 > 3:", 5 > 3);
console.log("'a' < 'b':", "a" < "b");

// 2. Logical operators
console.log("\n--- Logical Operators ---");
console.log("true && false:", true && false);
console.log("true || false:", true || false);
console.log("!true:", !true);

// 3. if/else statements
console.log("\n--- if/else Statements ---");
let age = 20;
if (age >= 18) {
    console.log("You are an adult");
} else {
    console.log("You are a minor");
}

// 4. Ternary operator
console.log("\n--- Ternary Operator ---");
let score = 85;
let grade = score >= 90 ? "A" : score >= 80 ? "B" : score >= 70 ? "C" : "D";
console.log("Score:", score, "Grade:", grade);

// 5. Nullish coalescing
console.log("\n--- Nullish Coalescing ---");
let name = null;
let displayName = name ?? "Anonymous";
console.log("Display name:", displayName);

// 6. Switch statement
console.log("\n--- Switch Statement ---");
let day = "Monday";
let dayType;
switch (day) {
    case "Monday":
    case "Tuesday":
    case "Wednesday":
    case "Thursday":
    case "Friday":
        dayType = "Weekday";
        break;
    case "Saturday":
    case "Sunday":
        dayType = "Weekend";
        break;
    default:
        dayType = "Invalid";
}
console.log(`${day} is a ${dayType}`);

// 7. Grade calculator
console.log("\n--- Grade Calculator ---");
function calculateGrade(score) {
    if (score < 0 || score > 100) return "Invalid";
    if (score >= 90) return "A";
    if (score >= 80) return "B";
    if (score >= 70) return "C";
    if (score >= 60) return "D";
    return "F";
}

console.log("Score 95:", calculateGrade(95));
console.log("Score 75:", calculateGrade(75));
console.log("Score 55:", calculateGrade(55));

console.log("\n=== Session 4 Complete ===");
```

**Complete index.html:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Session 4 - Conditionals</title>
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
        .calculator label {
            display: inline-block;
            width: 120px;
            margin-right: 10px;
        }
        .calculator input, .calculator select {
            padding: 8px;
            margin: 5px 0;
            border: 1px solid #ddd;
            border-radius: 4px;
        }
        .calculator button {
            padding: 10px 20px;
            background-color: #1890ff;
            color: white;
            border: none;
            border-radius: 4px;
            cursor: pointer;
            margin-top: 10px;
        }
        .calculator button:hover {
            background-color: #0c7cd5;
        }
        .result {
            background: #e6f7ff;
            padding: 15px;
            border-radius: 4px;
            margin-top: 15px;
            border-left: 4px solid #1890ff;
        }
        .result.error {
            background: #fff1f0;
            border-left-color: #ff4d4f;
        }
        .result.success {
            background: #f6ffed;
            border-left-color: #52c41a;
        }
    </style>
</head>
<body>
    <h1>Session 4: Conditionals</h1>
    
    <div class="section">
        <h2>Topics Covered</h2>
        <ul>
            <li>Comparison operators (==, ===, >, <, >=, <=)</li>
            <li>Logical operators (&&, ||, !)</li>
            <li>if/else statements</li>
            <li>Nested if statements</li>
            <li>Ternary operator</li>
            <li>Nullish coalescing operator (??)</li>
            <li>Switch statements</li>
        </ul>
    </div>

    <div class="section">
        <h2>Grade Calculator</h2>
        <div class="calculator">
            <label for="score">Score (0-100):</label>
            <input type="number" id="score" min="0" max="100" value="85">
            <button onclick="calculateGrade()">Calculate Grade</button>
            
            <div id="gradeResult" class="result" style="display: none;">
                Grade result will appear here...
            </div>
        </div>
    </div>

    <div class="section">
        <h2>Temperature Converter</h2>
        <div class="calculator">
            <label for="tempValue">Temperature:</label>
            <input type="number" id="tempValue" value="0">
            
            <label for="fromUnit">From:</label>
            <select id="fromUnit">
                <option value="C">Celsius</option>
                <option value="F">Fahrenheit</option>
                <option value="K">Kelvin</option>
            </select>
            
            <label for="toUnit">To:</label>
            <select id="toUnit">
                <option value="F">Fahrenheit</option>
                <option value="C">Celsius</option>
                <option value="K">Kelvin</option>
            </select>
            
            <button onclick="convertTemperature()">Convert</button>
            
            <div id="tempResult" class="result" style="display: none;">
                Conversion result will appear here...
            </div>
        </div>
    </div>

    <div class="section">
        <h2>Shopping Discount Calculator</h2>
        <div class="calculator">
            <label for="amount">Total Amount ($):</label>
            <input type="number" id="amount" min="0" value="500">
            
            <label for="customerType">Customer Type:</label>
            <select id="customerType">
                <option value="Regular">Regular</option>
                <option value="Premium">Premium</option>
                <option value="VIP">VIP</option>
                <option value="Guest">Guest</option>
            </select>
            
            <button onclick="calculateDiscount()">Calculate Discount</button>
            
            <div id="discountResult" class="result" style="display: none;">
                Discount calculation will appear here...
            </div>
        </div>
    </div>

    <div class="section">
        <h2>Console Output</h2>
        <p>Open the browser console (F12) to see all JavaScript examples.</p>
    </div>

    <script src="script.js" defer></script>
    <script>
        function calculateGrade() {
            const score = parseInt(document.getElementById('score').value);
            const resultDiv = document.getElementById('gradeResult');
            
            let grade, message, className;
            
            if (score < 0 || score > 100) {
                grade = "Invalid";
                message = "Please enter a score between 0 and 100";
                className = "error";
            } else if (score >= 90) {
                grade = "A";
                message = "Outstanding performance!";
                className = "success";
            } else if (score >= 80) {
                grade = "B";
                message = "Very good performance!";
                className = "success";
            } else if (score >= 70) {
                grade = "C";
                message = "Satisfactory performance.";
                className = "success";
            } else if (score >= 60) {
                grade = "D";
                message = "Needs improvement.";
                className = "error";
            } else {
                grade = "F";
                message = "Fail - needs significant improvement.";
                className = "error";
            }
            
            resultDiv.innerHTML = `<strong>Grade: ${grade}</strong><br>${message}`;
            resultDiv.className = `result ${className}`;
            resultDiv.style.display = 'block';
        }

        function convertTemperature() {
            const value = parseFloat(document.getElementById('tempValue').value);
            const fromUnit = document.getElementById('fromUnit').value;
            const toUnit = document.getElementById('toUnit').value;
            const resultDiv = document.getElementById('tempResult');
            
            let celsius;
            
            // Convert to Celsius first
            switch (fromUnit) {
                case "C":
                    celsius = value;
                    break;
                case "F":
                    celsius = (value - 32) * 5/9;
                    break;
                case "K":
                    celsius = value - 273.15;
                    break;
            }
            
            // Convert from Celsius to target unit
            let result;
            switch (toUnit) {
                case "C":
                    result = celsius.toFixed(2);
                    break;
                case "F":
                    result = (celsius * 9/5 + 32).toFixed(2);
                    break;
                case "K":
                    result = (celsius + 273.15).toFixed(2);
                    break;
            }
            
            resultDiv.innerHTML = `${value}°${fromUnit} = ${result}°${toUnit}`;
            resultDiv.className = 'result success';
            resultDiv.style.display = 'block';
        }

        function calculateDiscount() {
            const amount = parseFloat(document.getElementById('amount').value);
            const customerType = document.getElementById('customerType').value;
            const resultDiv = document.getElementById('discountResult');
            
            let discountRate;
            
            switch (customerType) {
                case "VIP":
                    discountRate = 0.20;
                    break;
                case "Premium":
                    discountRate = 0.15;
                    break;
                case "Regular":
                    discountRate = 0.10;
                    break;
                case "Guest":
                    discountRate = 0.05;
                    break;
            }
            
            // Additional discount for large purchases
            if (amount > 1000) {
                discountRate += 0.05;
            }
            
            // Cap discount at 25%
            discountRate = Math.min(discountRate, 0.25);
            
            const discountAmount = amount * discountRate;
            const finalAmount = amount - discountAmount;
            
            resultDiv.innerHTML = `
                <strong>Original:</strong> $${amount.toFixed(2)}<br>
                <strong>Discount:</strong> ${(discountRate * 100).toFixed(1)}% (-$${discountAmount.toFixed(2)})<br>
                <strong>Final:</strong> $${finalAmount.toFixed(2)}
            `;
            resultDiv.className = 'result success';
            resultDiv.style.display = 'block';
        }
    </script>
</body>
</html>
```

---

## 📝 Review (0.5h)

### If Condition Challenge

```javascript
// Challenge 1: Write an if statement to check if a number is positive, negative, or zero
let num = -5;
// Your code here

// Challenge 2: Check if a person can vote (age >= 18) and has ID
let age = 20;
let hasID = true;
// Your code here

// Challenge 3: Determine the largest of three numbers
let a = 10, b = 20, c = 15;
// Your code here

// Challenge 4: Check if a year is a leap year
// Leap year rules:
// - Divisible by 4
// - Not divisible by 100, unless also divisible by 400
let year = 2024;
// Your code here

// Challenge 5: Validate user input
// Check if username is not empty, password is at least 8 characters, and email contains "@"
let username = "john";
let password = "password123";
let email = "john@example.com";
// Your code here
```

### Switch/If Challenge

```javascript
// Challenge 1: Convert the following if/else to switch
let day = "Monday";
let dayType;

if (day === "Monday" || day === "Tuesday" || day === "Wednesday" || 
    day === "Thursday" || day === "Friday") {
    dayType = "Weekday";
} else if (day === "Saturday" || day === "Sunday") {
    dayType = "Weekend";
} else {
    dayType = "Invalid";
}
// Convert to switch statement

// Challenge 2: Convert the following switch to if/else
let grade = "B";
let message;

switch (grade) {
    case "A":
        message = "Excellent";
        break;
    case "B":
        message = "Good";
        break;
    case "C":
        message = "Satisfactory";
        break;
    case "D":
        message = "Needs improvement";
        break;
    case "F":
        message = "Fail";
        break;
    default:
        message = "Invalid grade";
}
// Convert to if/else statement

// Challenge 3: Use ternary operator instead of if/else
let age = 20;
let status;
if (age >= 18) {
    status = "Adult";
} else {
    status = "Minor";
}
// Convert to ternary

// Challenge 4: Use nullish coalescing instead of logical OR
let name = "";
let displayName = name || "Anonymous";
// Convert to nullish coalescing

// Challenge 5: Create a function that returns the day type using both switch and if/else
function getDayTypeSwitch(day) {
    // Using switch
}

function getDayTypeIf(day) {
    // Using if/else
}
// Test both functions
```

### Challenge Solutions

**If Condition Challenge Solutions:**
```javascript
// Challenge 1
let num = -5;
if (num > 0) {
    console.log("Positive");
} else if (num < 0) {
    console.log("Negative");
} else {
    console.log("Zero");
}

// Challenge 2
let age = 20;
let hasID = true;
if (age >= 18 && hasID) {
    console.log("Can vote");
} else {
    console.log("Cannot vote");
}

// Challenge 3
let a = 10, b = 20, c = 15;
let largest;
if (a >= b && a >= c) {
    largest = a;
} else if (b >= a && b >= c) {
    largest = b;
} else {
    largest = c;
}
console.log("Largest:", largest);

// Challenge 4
let year = 2024;
let isLeapYear;
if (year % 4 === 0) {
    if (year % 100 === 0) {
        if (year % 400 === 0) {
            isLeapYear = true;
        } else {
            isLeapYear = false;
        }
    } else {
        isLeapYear = true;
    }
} else {
    isLeapYear = false;
}
console.log(`${year} is ${isLeapYear ? "a leap year" : "not a leap year"}`);

// Simplified version
isLeapYear = (year % 4 === 0 && year % 100 !== 0) || (year % 400 === 0);

// Challenge 5
let username = "john";
let password = "password123";
let email = "john@example.com";

let isValid = username !== "" && password.length >= 8 && email.includes("@");
console.log("Valid input:", isValid);
```

**Switch/If Challenge Solutions:**
```javascript
// Challenge 1: if/else to switch
let day = "Monday";
let dayType;

switch (day) {
    case "Monday":
    case "Tuesday":
    case "Wednesday":
    case "Thursday":
    case "Friday":
        dayType = "Weekday";
        break;
    case "Saturday":
    case "Sunday":
        dayType = "Weekend";
        break;
    default:
        dayType = "Invalid";
}

// Challenge 2: switch to if/else
let grade = "B";
let message;

if (grade === "A") {
    message = "Excellent";
} else if (grade === "B") {
    message = "Good";
} else if (grade === "C") {
    message = "Satisfactory";
} else if (grade === "D") {
    message = "Needs improvement";
} else if (grade === "F") {
    message = "Fail";
} else {
    message = "Invalid grade";
}

// Challenge 3: if/else to ternary
let age = 20;
let status = age >= 18 ? "Adult" : "Minor";

// Challenge 4: logical OR to nullish coalescing
let name = "";
let displayName = name ?? "Anonymous";

// Challenge 5: Both implementations
function getDayTypeSwitch(day) {
    switch (day.toLowerCase()) {
        case "monday":
        case "tuesday":
        case "wednesday":
        case "thursday":
        case "friday":
            return "Weekday";
        case "saturday":
        case "sunday":
            return "Weekend";
        default:
            return "Invalid";
    }
}

function getDayTypeIf(day) {
    const weekdays = ["monday", "tuesday", "wednesday", "thursday", "friday"];
    const weekends = ["saturday", "sunday"];
    const lowerDay = day.toLowerCase();
    
    if (weekdays.includes(lowerDay)) {
        return "Weekday";
    } else if (weekends.includes(lowerDay)) {
        return "Weekend";
    } else {
        return "Invalid";
    }
}

// Test
console.log(getDayTypeSwitch("Monday")); // "Weekday"
console.log(getDayTypeIf("Monday"));     // "Weekday"
console.log(getDayTypeSwitch("Saturday")); // "Weekend"
console.log(getDayTypeIf("Saturday"));     // "Weekend"
```

### Review Questions

1. **What is the difference between `==` and `===`?**
   - [ ] No difference
   - [ ] `==` compares values only, `===` compares values and types
   - [ ] `===` compares values only, `==` compares values and types
   - [ ] Both compare values and types

2. **What does `true && false` evaluate to?**
   - [ ] true
   - [ ] false
   - [ ] undefined
   - [ ] null

3. **What is the result of `5 > 3 && 2 < 1`?**
   - [ ] true
   - [ ] false
   - [ ] undefined
   - [ ] null

4. **What does the ternary operator `condition ? expr1 : expr2` do?**
   - [ ] Always returns expr1
   - [ ] Always returns expr2
   - [ ] Returns expr1 if condition is true, otherwise expr2
   - [ ] Returns expr1 if condition is false, otherwise expr2

5. **What is the difference between `||` and `??`?**
   - [ ] No difference
   - [ ] `||` returns first truthy value, `??` returns right side only for null/undefined
   - [ ] `??` returns first truthy value, `||` returns right side only for null/undefined
   - [ ] Both work the same way

6. **What does `null ?? "default"` return?**
   - [ ] null
   - [ ] "default"
   - [ ] undefined
   - [ ] Error

7. **What does `0 ?? "default"` return?**
   - [ ] 0
   - [ ] "default"
   - [ ] undefined
   - [ ] null

8. **What keyword is used to exit a switch statement?**
   - [ ] return
   - [ ] break
   - [ ] continue
   - [ ] exit

9. **What happens if you forget the `break` in a switch case?**
   - [ ] Error
   - [ ] Code continues to the next case (fallthrough)
   - [ ] Switch exits automatically
   - [ ] Only the first case executes

10. **What does `!true` evaluate to?**
    - [ ] true
    - [ ] false
    - [ ] undefined
    - [ ] null

### Correct Answers

1. ✅ `==` compares values only, `===` compares values and types
2. ✅ false
3. ✅ false
4. ✅ Returns expr1 if condition is true, otherwise expr2
5. ✅ `||` returns first truthy value, `??` returns right side only for null/undefined
6. ✅ "default"
7. ✅ 0
8. ✅ break
9. ✅ Code continues to the next case (fallthrough)
10. ✅ false

---

## 🎯 Next Steps

1. ✅ Practice all comparison operators
2. ✅ Master logical operators and short-circuit evaluation
3. ✅ Use ternary operator for simple conditions
4. ✅ Understand when to use `??` vs `||`
5. ✅ Practice switch statements with fallthrough
6. ✅ Build decision-based programs
7. ✅ Complete all challenge exercises

---

## 📚 Additional Resources

- [MDN: Comparison Operators](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Expressions_and_Operators#comparison_operators)
- [MDN: Logical Operators](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Expressions_and_Operators#logical_operators)
- [MDN: if...else](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/if...else)
- [MDN: Conditional (Ternary) Operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Conditional_operator)
- [MDN: Nullish Coalescing Operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Nullish_coalescing_operator)
- [MDN: switch](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/switch)
- [JavaScript.info: Ifelse](https://javascript.info/ifelse)
- [JavaScript.info: Logical Operators](https://javascript.info/logical-operators)

**Remember:** Conditionals are the backbone of program logic. Master them to create dynamic and responsive applications! 💪
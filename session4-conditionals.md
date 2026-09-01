# Session 4: Conditionals — Active Learning Redesign

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

- "What do you expect to see in the console?"
- "What rule made this happen?"
- "Is the value truthy or falsy?"
- "Which branch will run?"
- "How would you write this with fewer lines?"
- "What would break this code?"


### Instructor Note — Hidden Answers & Interactivity

- **Hidden dropdowns:** Every `<details>` block contains answers, review code, or instructor-only notes. Keep them closed while students are working.
- **Reveal only after the attempt:** Open each `Review`, `Issues`, `Fixed Version`, and `Answer Key` after students have shared their answers or completed the challenge.
- **Make every `Guess` interactive:** ask students to raise their hands, type in chat, or write their prediction on paper before running the code.
- **Live coding:** invite a student to type the `Code` block on the projector while the class reads along.
- **Keep score:** award points for correct predictions and completed challenges. Celebrate the 'Bug Hunter' winners.

---

## Part 0: Warm-Up — The Login That Always Fails (5 minutes)

### Problem

A login page accepts a username and password. The developer wrote this:

```javascript
let username = "admin";
let password = "admin123";

if (username = "admin") {
  console.log("Login allowed");
} else {
  console.log("Login denied");
}
```

### Guess

Ask: "Will this allow the user in? What is the difference between `=` and `===`?"

### Explain

- `=` is assignment, not comparison.
- `==` compares values with type coercion.
- `===` compares values and types without coercion.
- Best practice: always use `===` and `!==` for comparisons.

### Code

```javascript
let username = "admin";
let password = "admin123";

console.log(username === "admin");       // true
console.log(username === "Admin");       // false
console.log(password === "admin123");    // true
console.log(5 == "5");                   // true
console.log(5 === "5");                  // false
```

### Challenge

- **Requirement:** Predict and then run the code above.
- **Time limit:** 3 minutes

### Review

Ask: "Why is `5 == "5"` true but `5 === "5"` false?" Emphasize that the bug in the warm-up is one of the most common JavaScript mistakes.

---

## Part 1: Comparison Operators

### 1.1 Equality: `==` vs `===`

#### Problem

A form sends the age as a string. The code accepts `18` or `"18"`. Is that safe?

```javascript
let age = "18";

if (age == 18) {
  console.log("Allowed");
}
```

#### Guess

Ask: "Will this print `Allowed`? What if the user typed `"18abc"`?"

#### Explain

- `==` compares values after type coercion.
- `===` compares values and types.
- Use `===` unless you have a strong reason to allow coercion.

#### Code

```javascript
// Loose equality
console.log(5 == "5");              // true
console.log(5 == 5);                // true
console.log(null == undefined);     // true
console.log(0 == false);            // true
console.log("" == false);           // true

// Strict equality
console.log(5 === "5");             // false
console.log(5 === 5);               // true
console.log(null === undefined);    // false
console.log(0 === false);           // false
console.log("" === false);          // false

// Inequality
console.log(5 != "5");              // false
console.log(5 !== "5");             // true
```

#### Challenge

- **Requirement:** Predict and then run the following. Explain which results changed because of strict equality.
- **Time limit:** 3 minutes

```javascript
console.log(10 == "10");
console.log(10 === "10");
console.log(null == undefined);
console.log(null === undefined);
console.log(0 == "");
console.log(0 === "");
```

<details>
<summary>Review — reveal after the challenge</summary>

#### Review

Expected:

```
true
false
true
false
true
false
```

</details>

### 1.2 Relational and String Comparison

#### Problem

A sorting app must decide if `"10"` is greater than `2` or `"2"`.

#### Guess

Ask: "Is `"10" > 2` true? Is `"10" > "2"` true?"

#### Explain

- When both operands are strings, `>` and `<` compare Unicode character by character.
- When one side is a number, JavaScript converts the string to a number.

#### Code

```javascript
console.log(5 > 3);              // true
console.log(5 < 3);              // false
console.log(5 >= 5);             // true
console.log(5 <= 5);             // true

console.log("10" > 5);           // true (number comparison)
console.log("10" > "5");         // false (string comparison)
console.log("10" < "2");         // true (string comparison)
console.log("a" < "b");          // true
console.log("A" < "a");          // true (uppercase comes first)
console.log("apple" < "banana"); // true
```

#### Challenge

- **Requirement:** Without running, sort these strings from smallest to largest: `"Zebra"`, `"apple"`, `"Banana"`, `"2"`, `"10"`.
- **Time limit:** 4 minutes
- **Hint:** Compare first characters using Unicode order (digits, then uppercase, then lowercase).

<details>
<summary>Review — reveal after the challenge</summary>

#### Review

Expected order: `"10"`, `"2"`, `"Banana"`, `"Zebra"`, `"apple"`. Discuss why numbers as strings sort oddly.

---

</details>

## Part 2: Logical Operators

### 2.1 Logical AND (`&&`)

#### Problem

A student can only graduate if they passed both math and science.

```javascript
let mathPassed = true;
let sciencePassed = false;

if (mathPassed && sciencePassed) {
  console.log("Graduate");
} else {
  console.log("Not yet");
}
```

#### Guess

Ask: "What will this print?"

#### Explain

- `&&` returns the first falsy value, or the last truthy value if all are truthy.
- `false && anything` does not evaluate the second part (short-circuit).

#### Code

```javascript
console.log(true && true);      // true
console.log(true && false);     // false
console.log(false && true);     // false
console.log(false && false);    // false

console.log(true && "Hello");   // "Hello"
console.log(false && "Hello");  // false
console.log(5 && 10);           // 10
console.log(0 && 10);           // 0

let user = { name: "John", age: 30 };
if (user && user.name) {
  console.log("User name:", user.name);
}
```

#### Challenge

- **Requirement:** Declare `let hasTicket = true;` and `let hasID = false;`. Write an `if` that only lets the user in when both are true.
- **Time limit:** 3 minutes

<details>
<summary>Review — reveal after the challenge</summary>

#### Review

```javascript
let hasTicket = true;
let hasID = false;
if (hasTicket && hasID) {
  console.log("Enter");
} else {
  console.log("Cannot enter");
}
```

</details>

### 2.2 Logical OR (`||`)

#### Problem

A page shows a name, but if the name is missing, it shows `"Anonymous"`.

```javascript
let name = "";
let displayName = name || "Anonymous";
console.log(displayName);
```

#### Guess

Ask: "What will this print? What if `name` is `0`?"

#### Explain

- `||` returns the first truthy value, or the last falsy value.
- `0`, `""`, `null`, `undefined`, `false`, and `NaN` are falsy.
- Use `||` for default values, but be careful with falsy values like `0`.

#### Code

```javascript
console.log(true || true);      // true
console.log(true || false);     // true
console.log(false || true);     // true
console.log(false || false);    // false

console.log(true || "Hello");   // true
console.log(false || "Hello");  // "Hello"
console.log(5 || 10);           // 5
console.log(0 || 10);           // 10

let userName = null;
let displayName = userName || "Anonymous";
console.log(displayName);       // "Anonymous"
```

#### Challenge

- **Requirement:** Write one line that gives `count` a default value of `1` only if it is falsy.
- **Time limit:** 3 minutes
- **Hint:** `let value = count || 1;`

<details>
<summary>Review — reveal after the challenge</summary>

#### Review

`||` returns the first truthy value. Watch out: `0` and `""` are replaced, which may not be what you want. Use `??` for `0` and `""`.

</details>

### 2.3 Logical NOT (`!`) and Double Negation

#### Problem

A form should say "yes" only when the checkbox is not checked.

#### Guess

Ask: "What is `!0`? What is `!!0`? What is `!!"Hello"`?"

#### Explain

- `!` converts a value to a boolean and inverts it.
- `!!` converts a value to a boolean.

#### Code

```javascript
console.log(!true);        // false
console.log(!false);       // true
console.log(!0);           // true
console.log(!"");          // true
console.log(!null);        // true
console.log(!undefined);   // true
console.log(!"Hello");     // false
console.log(!10);          // false

console.log(!!"Hello");    // true
console.log(!!0);          // false
console.log(!!null);       // false
```

#### Challenge

- **Requirement:** Declare `let isOnline = true;`. Use `!` to print the opposite value.
- **Time limit:** 3 minutes

<details>
<summary>Review — reveal after the challenge</summary>

#### Review

`!` inverts the boolean. `!!` is a fast way to convert any value to a boolean. Use `!!` to normalize truthiness.

</details>

### 2.4 Operator Precedence

#### Problem

A quiz checks if a user passed the exam OR had good attendance AND completed the project. Which condition runs first?

#### Guess

Ask: "What is `true || false && false`? Does `||` or `&&` run first?"

#### Explain

Logical operator precedence: `!` > `&&` > `||`. Use parentheses to make the intention clear.

#### Code

```javascript
console.log(true || false && false);   // true (&& first)
console.log((true || false) && false); // false (parentheses first)
console.log(!true && false);           // false (! first)
console.log(!(true && false));         // true
```

#### Challenge

- **Requirement:** Rewrite `age > 18 && hasLicense || hasPermit` with parentheses that make the meaning clear.
- **Time limit:** 4 minutes
- **Hint:** The likely meaning is `(age > 18 && hasLicense) || hasPermit`.

<details>
<summary>Review — reveal after the challenge</summary>

#### Review

Precedence order: `!` > `&&` > `||`. Always use parentheses when mixing `&&` and `||`.

---

</details>

## Bug Hunt 1

### Problem

The following login function has three deliberate bugs. Ask students to find them.

```javascript
function login(username, password) {
  let users = {
    admin: { password: "admin123", role: "admin" },
    user: { password: "user123", role: "user" },
  };

  let user = users[username];

  if (user = undefined) {
    return "User not found";
  }

  if (user.password = password) {
    if (user.role = "admin") {
      return "Welcome, admin";
    } else {
      return "Welcome, user";
    }
  } else {
    return "Wrong password";
  }
}

console.log(login("admin", "admin123"));
```

<details>
<summary>Issues — reveal after the bug hunt</summary>

### Issues

1. `if (user = undefined)` uses `=` instead of `===` or `!user`.
2. `if (user.password = password)` uses `=` instead of `===`.
3. `if (user.role = "admin")` uses `=` instead of `===`.

</details>

<details>
<summary>Fixed Version — for the instructor</summary>

### Fixed Version (for the instructor)

```javascript
function login(username, password) {
  let users = {
    admin: { password: "admin123", role: "admin" },
    user: { password: "user123", role: "user" },
  };

  let user = users[username];

  if (user === undefined) {
    return "User not found";
  }

  if (user.password === password) {
    if (user.role === "admin") {
      return "Welcome, admin";
    } else {
      return "Welcome, user";
    }
  } else {
    return "Wrong password";
  }
}

console.log(login("admin", "admin123"));
```

</details>

### Points

1 point for each found bug.

---

## Part 3: if/else Statements

### 3.1 Basic if/else

#### Problem

A cinema checks age before selling a ticket.

#### Guess

Ask: "Will a 15-year-old see 'You can watch this movie' or 'You are too young'?"

#### Explain

- An `if` checks a condition.
- If the condition is true, the first block runs; otherwise the `else` block runs.
- The condition can be a boolean, a comparison, or any truthy/falsy value.

#### Code

```javascript
let age = 15;

if (age >= 18) {
  console.log("You can watch this movie");
} else {
  console.log("You are too young");
}
```

#### Challenge

- **Requirement:** Declare `let num = -5;`. Print `"Positive"`, `"Negative"`, or `"Zero"`.
- **Time limit:** 4 minutes
- **Hint:** Use `if`, `else if`, and `else`.

<details>
<summary>Review — reveal after the challenge</summary>

#### Review

Check `num > 0`, then `num < 0`, then fall back to `Zero`. Use `else if` for the middle case.

</details>

### 3.2 if/else if/else

#### Problem

A school prints a letter grade from a score.

#### Guess

Ask: "What grade does a score of 85 produce?"

#### Explain

- Use `else if` to test multiple conditions in order.
- Only the first true branch runs.
- Always validate input first when possible.

#### Code

```javascript
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

#### Challenge

- **Requirement:** Write a function `calculateGrade(score)` that returns `"A"`, `"B"`, `"C"`, `"D"`, `"F"`, or `"Invalid"` if the score is outside `0`–`100`.
- **Time limit:** 4 minutes
- **Hint:** Check for invalid input first.

<details>
<summary>Review — reveal after the challenge</summary>

#### Review

```javascript
function calculateGrade(score) {
  if (score < 0 || score > 100) {
    return "Invalid";
  }
  if (score >= 90) return "A";
  if (score >= 80) return "B";
  if (score >= 70) return "C";
  if (score >= 60) return "D";
  return "F";
}
```

</details>

### 3.3 Nested if Statements and Early Returns

#### Problem

A driving eligibility check has too many nested `if`s. How can we flatten it?

#### Guess

Ask: "Which message appears when age is 25, has license, no car?"

#### Explain

- Nested `if`s can be flattened with early `return`s.
- Flat code is easier to read and debug.

#### Code

```javascript
// Nested
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

```javascript
// Flattened with early returns
function checkDriving(age, hasLicense, hasCar) {
  if (age < 18) return "You are too young to drive";
  if (!hasLicense) return "You need to get a license";
  if (!hasCar) return "You have a license but no car";
  return "You can drive your car";
}

console.log(checkDriving(25, true, false));
```

#### Challenge

- **Requirement:** Convert this nested code into a function with early returns.
- **Time limit:** 5 minutes

```javascript
let temp = 30;
let isRaining = false;

if (temp > 30) {
  if (isRaining) {
    console.log("Hot and rainy");
  } else {
    console.log("Hot and sunny");
  }
} else {
  if (isRaining) {
    console.log("Cool and rainy");
  } else {
    console.log("Cool and dry");
  }
}
```

- **Hint:** Check the first condition first, then return a string.

<details>
<summary>Review — reveal after the challenge</summary>

#### Review

Early returns reduce nesting. Each `return` removes one level of `else`.

---

</details>

## Part 4: Ternary Operator

### 4.1 Basic Ternary

#### Problem

A label shows `"Adult"` or `"Minor"` in one line.

#### Guess

Ask: "What does `age >= 18 ? "Adult" : "Minor"` return for age 18?"

#### Explain

- Ternary is a shorthand for `if/else` when you need to assign a value.
- Syntax: `condition ? valueIfTrue : valueIfFalse`.

#### Code

```javascript
let age = 18;
let message = age >= 18 ? "Adult" : "Minor";
console.log(message);
```

#### Challenge

- **Requirement:** Use a ternary to print `"even"` or `"odd"` for `let num = 15;`.
- **Time limit:** 3 minutes
- **Hint:** `num % 2 === 0 ? "even" : "odd"`.

<details>
<summary>Review — reveal after the challenge</summary>

#### Review

Use ternary for simple two-way choices. Avoid deeply nested ternaries.

</details>

### 4.2 Nested Ternary

#### Problem

A grade must be shown as a letter in one expression.

#### Guess

Ask: "What grade does 85 produce with the nested ternary?"

#### Explain

- You can chain ternaries to handle many cases.
- It becomes hard to read quickly, so use it sparingly.

#### Code

```javascript
let score = 85;
let grade = score >= 90 ? "A" :
            score >= 80 ? "B" :
            score >= 70 ? "C" :
            score >= 60 ? "D" : "F";
console.log("Grade:", grade);
```

#### Challenge

- **Requirement:** Write a function `greet(timeOfDay)` that returns `"Good morning!"`, `"Good afternoon!"`, `"Good evening!"`, or `"Hello!"` using a nested ternary.
- **Time limit:** 4 minutes
- **Hint:** Compare `timeOfDay` to `"morning"`, `"afternoon"`, and `"evening"`.

<details>
<summary>Review — reveal after the challenge</summary>

#### Review

```javascript
function greet(timeOfDay) {
  return timeOfDay === "morning" ? "Good morning!" :
         timeOfDay === "afternoon" ? "Good afternoon!" :
         timeOfDay === "evening" ? "Good evening!" :
         "Hello!";
}
```

</details>

### 4.3 When to Use Ternary

#### Problem

When should we prefer a ternary over `if/else`?

#### Guess

Ask: "Would you write a complex grading system as one ternary?"

#### Explain

- Use ternary for simple `if/else` assignments.
- Avoid deeply nested ternaries; use `if/else` or `switch` for complex logic.

#### Code

```javascript
let age = 20;
let status = age >= 18 ? "adult" : "minor";
console.log(status);
```

#### Challenge

- **Requirement:** Convert this `if/else` into a one-line ternary.
- **Time limit:** 2 minutes

```javascript
let price;
let isMember = true;

if (isMember) {
  price = 80;
} else {
  price = 100;
}
```

<details>
<summary>Review — reveal after the challenge</summary>

#### Review

The ternary version: `let price = isMember ? 80 : 100;`. Keep it readable.

---

</details>

## Part 5: Nullish Coalescing Operator (`??`)

### 5.1 The Difference Between `||` and `??`

#### Problem

A user sets their age to `0`. The app should keep `0`, not replace it with a default.

```javascript
let age = 0;
let displayAge = age || 18;
console.log(displayAge); // 18, but should be 0
```

#### Guess

Ask: "Is `0` a real age, or should it be replaced?"

#### Explain

- `||` returns the first truthy value.
- `??` returns the right side only if the left side is `null` or `undefined`.
- `0`, `""`, `false`, and `NaN` are kept by `??`.

#### Code

```javascript
let count1 = 0;
console.log(count1 || 10);     // 10
console.log(count1 ?? 10);     // 0

let value1 = "";
console.log(value1 || "default");  // "default"
console.log(value1 ?? "default");  // ""

let value2 = false;
console.log(value2 || "default");  // "default"
console.log(value2 ?? "default");  // false

let value3 = null;
console.log(value3 ?? "default");  // "default"
```

#### Challenge

- **Requirement:** A user might not have a name (`null` or `undefined`). Use `??` to set a default `"Anonymous"`, but keep `""` if they explicitly set it.
- **Time limit:** 4 minutes
- **Hint:** `let displayName = name ?? "Anonymous";`

<details>
<summary>Review — reveal after the challenge</summary>

#### Review

`??` only falls back for `null` or `undefined`. Use it when `0`, `''`, `false`, and `NaN` are valid values.

</details>

### 5.2 Optional Chaining with Nullish Coalescing

#### Problem

A user object might not have a `profile` property. How do we safely get a name?

#### Guess

Ask: "What is the result if `settings` does not exist?"

#### Explain

- `?.` stops the chain when it hits `null` or `undefined` and returns `undefined`.
- Then `??` can supply a default.

#### Code

```javascript
let user1 = {
  profile: {
    name: "John",
    email: null
  }
};

let userName = user1?.profile?.name ?? "Anonymous";
let userEmail = user1?.profile?.email ?? "No email";
let userPhone = user1?.profile?.phone ?? "No phone";

console.log(userName);   // "John"
console.log(userEmail);  // "No email"
console.log(userPhone);  // "No phone"
```

#### Challenge

- **Requirement:** Given an object that may or may not have `settings.theme.color`, write one line that prints the color or `"blue"` as a default.
- **Time limit:** 4 minutes
- **Hint:** `console.log(obj?.settings?.theme?.color ?? "blue");`

<details>
<summary>Review — reveal after the challenge</summary>

#### Review

Combine `?.` and `??` for safe deep access and defaults. `?.` does not help with missing variables, only with `null` and `undefined`.

---

</details>

## Part 6: Switch Statements

### 6.1 Basic Switch

#### Problem

A calendar app tells the user whether a day is a weekday or weekend.

#### Guess

Ask: "What season is February in the Northern Hemisphere?"

#### Explain

- A `switch` compares one value against many `case`s.
- Use `break` to stop fallthrough.
- Fallthrough can be useful to group cases.

#### Code

```javascript
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
```

#### Challenge

- **Requirement:** Use a `switch` with fallthrough to print the season for a given month number (`1` to `12`).
- **Time limit:** 5 minutes
- **Hint:** Group `case 12`, `case 1`, `case 2` for Winter.

<details>
<summary>Review — reveal after the challenge</summary>

#### Review

```javascript
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
console.log(season);
```

</details>

### 6.2 Switch with Functions

#### Problem

A grade calculator returns a message based on a letter grade.

#### Guess

Ask: "What happens if the light color is 'RED' (uppercase)?"

#### Explain

- Make a switch case-insensitive by normalizing the input.
- `return` inside a `case` exits the function, replacing `break`.

#### Code

```javascript
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

console.log(message);
```

#### Challenge

- **Requirement:** Write a function `trafficLightAction(color)` that returns `"STOP"`, `"SLOW DOWN"`, `"GO"`, or `"INVALID"` using a switch. Make the input case-insensitive.
- **Time limit:** 5 minutes
- **Hint:** Convert `color` to lowercase before the switch.

<details>
<summary>Review — reveal after the challenge</summary>

#### Review

```javascript
function trafficLightAction(color) {
  switch (color.toLowerCase()) {
    case "red":
      return "STOP";
    case "yellow":
      return "SLOW DOWN";
    case "green":
      return "GO";
    default:
      return "INVALID";
  }
}
```

---

</details>

## Part 7: Practical Decision Programs

### 7.1 Grade Calculator

#### Problem

We need a function that returns both a grade and a message for any score.

#### Guess

Ask: "What does the function return for a score of 95?"

#### Explain

- Combine `if/else` to compute the grade.
- Use `switch` to choose the message.

#### Code

```javascript
function getGradeDetails(score) {
  let grade;

  if (score < 0 || score > 100) {
    return { grade: "Invalid", message: "Score out of range" };
  } else if (score >= 90) {
    grade = "A";
  } else if (score >= 80) {
    grade = "B";
  } else if (score >= 70) {
    grade = "C";
  } else if (score >= 60) {
    grade = "D";
  } else {
    grade = "F";
  }

  let message;
  switch (grade) {
    case "A": message = "Outstanding performance!"; break;
    case "B": message = "Very good performance!"; break;
    case "C": message = "Satisfactory performance."; break;
    case "D": message = "Needs improvement."; break;
    case "F": message = "Fail - needs significant improvement."; break;
  }

  return { grade, message };
}

console.log(getGradeDetails(95));
console.log(getGradeDetails(85));
```

#### Challenge

- **Requirement:** Use `getGradeDetails` with the array `[95, 85, 75, 65, 55, 105, -5]` and log each result.
- **Time limit:** 5 minutes

<details>
<summary>Review — reveal after the challenge</summary>

#### Review

Separating grade calculation from message lookup makes the function easier to maintain.

</details>

### 7.2 Temperature Converter

#### Problem

Convert a temperature between Celsius, Fahrenheit, and Kelvin.

#### Guess

Ask: "What is 0°C in Fahrenheit?"

#### Explain

- Use `switch` to convert the input to Celsius, then another `switch` to convert to the target unit.

#### Code

```javascript
function convertTemperature(value, fromUnit, toUnit) {
  if (fromUnit === toUnit) {
    return value.toFixed(2);
  }

  let celsius;

  switch (fromUnit) {
    case "C": celsius = value; break;
    case "F": celsius = (value - 32) * 5 / 9; break;
    case "K": celsius = value - 273.15; break;
    default: return "Invalid fromUnit";
  }

  switch (toUnit) {
    case "C": return celsius.toFixed(2);
    case "F": return (celsius * 9 / 5 + 32).toFixed(2);
    case "K": return (celsius + 273.15).toFixed(2);
    default: return "Invalid toUnit";
  }
}

console.log("0C to F:", convertTemperature(0, "C", "F"));
console.log("32F to C:", convertTemperature(32, "F", "C"));
```

#### Challenge

- **Requirement:** Convert `100°C` to Kelvin and `273.15K` to Celsius.
- **Time limit:** 4 minutes

<details>
<summary>Review — reveal after the challenge</summary>

#### Review

Each `switch` has one responsibility: convert to Celsius, or convert from Celsius.

</details>

### 7.3 Shopping Discount Calculator

#### Problem

Calculate a final price based on customer type and total amount.

#### Guess

Ask: "What is the final price for a VIP with $1500 purchase?"

#### Explain

- Use `switch` to select the base discount rate.
- Use `if` to add an extra discount for large orders.
- Cap the total discount to avoid over-discounting.

#### Code

```javascript
function calculateDiscount(totalAmount, customerType) {
  let discountRate;

  switch (customerType) {
    case "VIP": discountRate = 0.20; break;
    case "Premium": discountRate = 0.15; break;
    case "Regular": discountRate = 0.10; break;
    case "Guest": discountRate = 0.05; break;
    default: discountRate = 0;
  }

  if (totalAmount > 1000) {
    discountRate += 0.05;
  }

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

console.log(calculateDiscount(1500, "VIP"));
```

#### Challenge

- **Requirement:** Use the function for these purchases and log the final amounts.
- **Time limit:** 5 minutes

```javascript
let purchases = [
  { amount: 500, type: "Regular" },
  { amount: 1500, type: "VIP" },
  { amount: 800, type: "Premium" },
  { amount: 200, type: "Guest" }
];
```

<details>
<summary>Review — reveal after the challenge</summary>

#### Review

`Math.min(discountRate, 0.25)` caps the discount. Always guard against impossible edge cases.

</details>

### 7.4 Login System

#### Problem

Authenticate a user and return an access level.

#### Guess

Ask: "What happens if the username does not exist?"

#### Explain

- Use `if` checks for the user, password, and role.
- Use `switch` to map the role to an access level.

#### Code

```javascript
function authenticateUser(username, password, role) {
  const users = {
    admin: { password: "admin123", role: "admin" },
    user: { password: "user123", role: "user" },
    guest: { password: "guest123", role: "guest" }
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
    case "admin": return "Full access to all features";
    case "user": return "Access to user features";
    case "guest": return "Limited access to guest features";
    default: return "No access";
  }
}

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

#### Challenge

- **Requirement:** Add a new role `"manager"` with a custom access level.
- **Time limit:** 5 minutes

<details>
<summary>Review — reveal after the challenge</summary>

#### Review

Early `return`s inside `if` blocks keep the function flat. The `switch` is only responsible for mapping a valid role.

---

</details>

## Bug Hunt 2

### Problem

The discount calculator has bugs. Ask students to find them.

```javascript
function discount(total, customerType) {
  let rate = 0;

  if (customerType = "VIP") {
    rate = 0.2;
  } else if (customerType = "Premium") {
    rate = 0.15;
  } else customerType = "Guest" {
    rate = 0.05;
  }

  if (total > 1000) {
    rate = rate + 0.05;
  }

  let final = total - (total * rate);
  return final.toFixed(2);
}

console.log(discount(500, "Premium"));
```

<details>
<summary>Issues — reveal after the bug hunt</summary>

### Issues

1. `if (customerType = "VIP")` and `if (customerType = "Premium")` use `=` instead of `===`.
2. `else customerType = "Guest"` is missing `if` and uses `=`.
3. The code does not validate `total`.
4. Missing braces make `else` attach to the wrong `if`.

</details>

<details>
<summary>Fixed Version — for the instructor</summary>

### Fixed Version (for the instructor)

```javascript
function discount(total, customerType) {
  if (total < 0) return "Invalid total";

  let rate = 0;

  if (customerType === "VIP") {
    rate = 0.2;
  } else if (customerType === "Premium") {
    rate = 0.15;
  } else if (customerType === "Guest") {
    rate = 0.05;
  }

  if (total > 1000) {
    rate += 0.05;
  }

  let final = total - (total * rate);
  return final.toFixed(2);
}

console.log(discount(500, "Premium"));
```

</details>

### Points

1 point for each found bug.

---

## Group Challenge: Condition Builders

- **Time:** 8 minutes
- **Teams:** 2 or 3 students per team
- **Task:** Each team must write the smallest correct condition for each situation.
- **Scoring:** 1 point per correct condition. The team with the most points wins.

### Situations

1. A number `x` is between `10` and `20` (inclusive).
2. A user can vote if age is at least `18` and they have an ID.
3. A password is valid if it is not empty and has at least `8` characters.
4. A year is a leap year.
5. A light is safe to pass: it is `"green"` or `"flashing yellow"`.

<details>
<summary>Answer Key — for the instructor</summary>

### Instructor Answer Key

```javascript
1. x >= 10 && x <= 20
2. age >= 18 && hasID
3. password !== "" && password.length >= 8
4. (year % 4 === 0 && year % 100 !== 0) || (year % 400 === 0)
5. light === "green" || light === "flashing yellow"
```

---

</details>

## Individual Challenges — Progressive Difficulty

### Level 1: Number Sign (3 minutes)

- **Requirement:** Print `"Positive"`, `"Negative"`, or `"Zero"` for a number.

### Level 2: Leap Year (5 minutes)

- **Requirement:** Write a function `isLeapYear(year)` that returns `true` for leap years.
- **Rules:** divisible by 4, not by 100 unless also by 400.

### Level 3: Access Control (5 minutes)

- **Requirement:** Write a function `canAccess(role, isPaid)` that returns `true` for `"admin"`, or for `"user"` when `isPaid` is true, otherwise `false`.

### Level 4: Grade to Message (4 minutes)

- **Requirement:** Use a `switch` to convert a grade letter to a message.

### Level 5: Nullish Default (4 minutes)

- **Requirement:** Write one line that uses `??` to give `0` for `count` if it is `null` or `undefined`, but keeps `0` as a valid value.

---

## Mini Project: Smart Access Dashboard

### Time

20 minutes

### Goal

Combine comparisons, logical operators, `if/else`, `switch`, ternary, and `??` into one small HTML page.

### Requirements for the Students

1. Create a small HTML page with inputs for:
   - Username
   - Password
   - Role (`admin`, `user`, `guest`)
   - Account status (`active`, `suspended`)

2. When the user clicks a button:
   - Validate the username is not empty.
   - Validate the password is at least 6 characters.
   - Check if the account is active.
   - Use a `switch` to determine the access level based on role.
   - Show a result message.

3. Example messages:
   - `"Access denied: account suspended"`
   - `"Welcome admin. Full access"`
   - `"Welcome user. Limited access"`
   - `"Welcome guest. Read-only access"`

### Time Limit

20 minutes

### Hints (optional)

- Use `if` to validate input and status.
- Use `||` or `??` for default messages.
- Use `switch` for role-based access.

### Starter `index.html`

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Smart Access Dashboard</title>
  <style>
    body { font-family: Arial, sans-serif; max-width: 500px; margin: 20px auto; }
    label { display: block; margin-top: 10px; }
    input, select { width: 100%; padding: 5px; }
    button { margin-top: 15px; padding: 10px 20px; }
    #result { background: #f5f5f5; padding: 15px; margin-top: 20px; border-radius: 4px; }
  </style>
</head>
<body>
  <h1>Smart Access Dashboard</h1>
  <label>Username: <input type="text" id="username" value="john"></label>
  <label>Password: <input type="password" id="password" value="pass123"></label>
  <label>Role:
    <select id="role">
      <option value="admin">admin</option>
      <option value="user">user</option>
      <option value="guest">guest</option>
    </select>
  </label>
  <label>Status:
    <select id="status">
      <option value="active">active</option>
      <option value="suspended">suspended</option>
    </select>
  </label>
  <button id="check">Check Access</button>
  <div id="result"></div>

  <script>
    document.getElementById("check").addEventListener("click", function () {
      let username = document.getElementById("username").value.trim();
      let password = document.getElementById("password").value;
      let role = document.getElementById("role").value;
      let status = document.getElementById("status").value;
      let result = document.getElementById("result");

      if (username === "") {
        result.textContent = "Username is required";
        return;
      }

      if (password.length < 6) {
        result.textContent = "Password must be at least 6 characters";
        return;
      }

      if (status === "suspended") {
        result.textContent = "Access denied: account suspended";
        return;
      }

      let access;
      switch (role) {
        case "admin":
          access = "Full access to all features";
          break;
        case "user":
          access = "Limited access to user features";
          break;
        case "guest":
          access = "Read-only access";
          break;
        default:
          access = "No access";
      }

      result.textContent = `Welcome ${username}. ${access}`;
    });
  </script>
</body>
</html>
```

### Review Questions for the Mini Project

- "What happens if the role is not one of the three cases?"
- "Why did we use `return` inside the event listener?"
- "What is the difference between `===` and `==` in this project?"

---

<details>
<summary>Trainer Solutions — Do Not Show Until Students Try</summary>

## Trainer Solutions — Do Not Show Until Students Try

### Challenge 1.1

```javascript
console.log(10 == "10");            // true
console.log(10 === "10");           // false
console.log(null == undefined);     // true
console.log(null === undefined);    // false
console.log(0 == "");               // true
console.log(0 === "");              // false
```

### Challenge 1.2

Order: `"10"`, `"2"`, `"Banana"`, `"Zebra"`, `"apple"`.

### Challenge 2.1

```javascript
let hasTicket = true;
let hasID = false;
if (hasTicket && hasID) {
  console.log("Enter");
} else {
  console.log("Cannot enter");
}
```

### Challenge 2.2

```javascript
let count = 0;
let value = count || 1;
```

### Challenge 2.3

```javascript
let isOnline = true;
console.log(!isOnline);
```

### Challenge 2.4

```javascript
(age > 18 && hasLicense) || hasPermit
```

### Challenge 3.1

```javascript
let num = -5;
if (num > 0) {
  console.log("Positive");
} else if (num < 0) {
  console.log("Negative");
} else {
  console.log("Zero");
}
```

### Challenge 3.2

```javascript
function calculateGrade(score) {
  if (score < 0 || score > 100) return "Invalid";
  if (score >= 90) return "A";
  if (score >= 80) return "B";
  if (score >= 70) return "C";
  if (score >= 60) return "D";
  return "F";
}
```

### Challenge 3.3

```javascript
function weather(temp, isRaining) {
  if (temp > 30 && isRaining) return "Hot and rainy";
  if (temp > 30) return "Hot and sunny";
  if (isRaining) return "Cool and rainy";
  return "Cool and dry";
}
```

### Challenge 4.1

```javascript
let num = 15;
console.log(num % 2 === 0 ? "even" : "odd");
```

### Challenge 4.2

```javascript
function greet(timeOfDay) {
  return timeOfDay === "morning" ? "Good morning!" :
         timeOfDay === "afternoon" ? "Good afternoon!" :
         timeOfDay === "evening" ? "Good evening!" :
         "Hello!";
}
```

### Challenge 5.1

```javascript
let displayName = name ?? "Anonymous";
```

### Challenge 5.2

```javascript
console.log(obj?.settings?.theme?.color ?? "blue");
```

### Challenge 6.1

See the season switch in Part 6.1.

### Challenge 6.2

```javascript
function trafficLightAction(color) {
  switch (color.toLowerCase()) {
    case "red":
      return "STOP";
    case "yellow":
      return "SLOW DOWN";
    case "green":
      return "GO";
    default:
      return "INVALID";
  }
}
```

### Challenge 7.1

```javascript
let scores = [95, 85, 75, 65, 55, 105, -5];
scores.forEach(score => console.log(getGradeDetails(score)));
```

### Individual Challenges Solutions

```javascript
// Level 1
let num = 5;
if (num > 0) console.log("Positive");
else if (num < 0) console.log("Negative");
else console.log("Zero");

// Level 2
function isLeapYear(year) {
  return (year % 4 === 0 && year % 100 !== 0) || (year % 400 === 0);
}

// Level 3
function canAccess(role, isPaid) {
  if (role === "admin") return true;
  return role === "user" && isPaid;
}

// Level 4
function gradeMessage(grade) {
  switch (grade) {
    case "A": return "Excellent";
    case "B": return "Good";
    case "C": return "Satisfactory";
    case "D": return "Needs improvement";
    case "F": return "Fail";
    default: return "Invalid";
  }
}

// Level 5
let count = undefined;
let value = count ?? 0;
```

</details>

---

<details>
<summary>Review Questions — reveal at the end of the session</summary>

## Review Questions

1. What is the difference between `==` and `===`?
   - [ ] No difference
   - [x] `==` compares values only, `===` compares values and types
   - [ ] `===` compares values only, `==` compares values and types
   - [ ] Both compare values and types

2. What does `true && false` evaluate to?
   - [ ] true
   - [x] false
   - [ ] undefined
   - [ ] null

3. What is the result of `5 > 3 && 2 < 1`?
   - [ ] true
   - [x] false
   - [ ] undefined
   - [ ] null

4. What does the ternary operator `condition ? expr1 : expr2` do?
   - [ ] Always returns expr1
   - [ ] Always returns expr2
   - [x] Returns expr1 if condition is true, otherwise expr2
   - [ ] Returns expr1 if condition is false, otherwise expr2

5. What is the difference between `||` and `??`?
   - [ ] No difference
   - [x] `||` returns first truthy value, `??` returns right side only for null/undefined
   - [ ] `??` returns first truthy value, `||` returns right side only for null/undefined
   - [ ] Both work the same way

6. What does `null ?? "default"` return?
   - [ ] null
   - [x] "default"
   - [ ] undefined
   - [ ] Error

7. What does `0 ?? "default"` return?
   - [x] 0
   - [ ] "default"
   - [ ] undefined
   - [ ] null

8. What keyword is used to exit a switch statement?
   - [ ] return
   - [x] break
   - [ ] continue
   - [ ] exit

9. What happens if you forget the `break` in a switch case?
   - [ ] Error
   - [x] Code continues to the next case (fallthrough)
   - [ ] Switch exits automatically
   - [ ] Only the first case executes

10. What does `!true` evaluate to?
    - [ ] true
    - [x] false
    - [ ] undefined
    - [ ] null

---

</details>

## Additional Resources

- [MDN: Comparison Operators](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Expressions_and_Operators#comparison_operators)
- [MDN: Logical Operators](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Expressions_and_Operators#logical_operators)
- [MDN: if...else](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/if...else)
- [MDN: Conditional (Ternary) Operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Conditional_operator)
- [MDN: Nullish Coalescing Operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Nullish_coalescing_operator)
- [MDN: switch](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/switch)
- [JavaScript.info: Ifelse](https://javascript.info/ifelse)
- [JavaScript.info: Logical Operators](https://javascript.info/logical-operators)

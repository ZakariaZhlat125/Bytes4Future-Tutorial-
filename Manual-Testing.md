# Session 14: Manual Testing

## Duration Breakdown
- **1 Hour**: Theoretical Explanation + Live Examples
- **1.5 Hours**: Practical Application (Student writes test cases and bug reports)
- **0.5 Hours**: Review, Questions, Problem Solving

---

## Learning Objectives
By the end of this session, you will be able to:
- Understand what software testing is and why it matters
- Distinguish between testing types, levels, and techniques
- Write professional test cases and test scenarios
- Report bugs clearly so developers can reproduce and fix them
- Understand severity vs priority and the bug life cycle
- Perform structured exploratory testing
- Read requirements and turn them into a test plan

---

## Part 1: Theoretical Explanation + Live Examples (1 Hour)

### Section 1: What is Software Testing?

**Definition:**
Software testing is the process of evaluating a software application to find defects and verify that it meets requirements and user expectations.

**Goals of Testing:**
- Find bugs before users do
- Verify the software works as specified
- Verify it handles invalid input gracefully
- Build confidence in the release
- Reduce the cost and risk of failures in production

**Why Testing Matters:**
- Bugs found in production cost far more than bugs found in development
- A small typo in requirements can become a crashed rocket (Ariane 5, 1996) or a lost Mars orbiter (NASA, 1999 — metric vs imperial units)
- Users leave apps that crash, lose data, or behave unexpectedly

**Cost of a Bug Over Time:**
```
Requirements → cheapest to fix
Design
Development
Testing
Production → most expensive to fix (fixes + support + reputation)
```

**Important truth:**
Testing can show the presence of bugs, never their absence. "Tested" means "we looked and found no problems under these conditions" — not "bug-free".

---

### Section 2: Manual vs Automated Testing

**Manual Testing:**
A human executes test cases without automation tools.

**Best for:**
- Exploratory testing (human curiosity finds what scripts miss)
- Usability and "look and feel"
- One-time or rarely repeated tests
- Rapidly changing features
- Ad-hoc verification of a bug fix

**Automated Testing:**
Scripts execute tests (e.g., Selenium, Cypress, Playwright, unit tests).

**Best for:**
- Regression suites run every release
- Repetitive, stable test cases
- Large data combinations
- Performance/load tests
- CI/CD pipelines

**Key Difference:**
Manual testing uses human judgment; automation gives speed and repeatability. Professional teams use **both** — manual testing usually comes first.

---

### Section 3: SDLC — Where Does Testing Fit?

**Software Development Life Cycle:**
```
Requirements
    ↓
Design
    ↓
Development
    ↓
Testing
    ↓
Deployment
    ↓
Maintenance
```

**Modern reality:**
Testing is not a phase at the end — QA is involved from requirements ("shift-left testing"). Testers review requirements early because a requirement bug is the cheapest bug.

**QA Roles:**
- **QA Engineer / Tester**: designs and executes tests, reports bugs
- **Test Lead**: plans strategy, assigns work
- **Automation Engineer**: writes test scripts
- **Developers**: write unit tests, fix bugs

---

### Section 4: STLC — Software Testing Life Cycle

```
Requirement Analysis
        ↓
Test Planning
        ↓
Test Case Design
        ↓
Environment Setup
        ↓
Test Execution
        ↓
Test Closure / Reporting
```

**Phases:**
1. **Requirement Analysis** — understand what must be tested; clarify ambiguous requirements
2. **Test Planning** — scope, strategy, tools, schedule, who tests what
3. **Test Case Design** — write scenarios and detailed test cases
4. **Environment Setup** — test servers, test data, devices/browsers
5. **Test Execution** — run tests, log results, report bugs
6. **Test Closure** — summary report: what passed, what failed, what remains risky

---

### Section 5: Levels of Testing

```
Unit Testing        → smallest pieces (functions, components)
    ↓
Integration Testing → modules working together
    ↓
System Testing      → the complete application end-to-end
    ↓
Acceptance Testing  → does it satisfy the business/user? (UAT)
```

- **Unit testing**: usually written by developers
- **Integration testing**: does the login form actually call the auth API correctly?
- **System testing**: the whole product in a production-like environment
- **UAT (User Acceptance Testing)**: real users/stakeholders confirm it solves their problem

---

### Section 6: Types of Testing

**Functional Testing** — does it do what it should?
- **Smoke testing**: quick check that the build isn't "on fire" (app opens, login works)
- **Sanity testing**: narrow check that a specific fix/change works
- **Regression testing**: re-run existing tests to catch what a change broke
- **Retesting**: verify a specific reported bug is fixed

**Non-Functional Testing** — how well does it do it?
- **Performance/Load**: fast enough under many users?
- **Security**: can data leak? Can users escalate privileges?
- **Usability**: is it understandable and easy to use?
- **Compatibility**: works on Chrome/Firefox/Safari, mobile, different screen sizes?
- **Accessibility**: usable with screen readers, keyboard only, color-blind safe?

**Smoke vs Sanity:**
- Smoke = wide and shallow ("does the build basically work?")
- Sanity = narrow and deep ("does this specific fix work?")

**Black-box vs White-box:**
- **Black-box**: test inputs/outputs without seeing the code (typical manual QA)
- **White-box**: test with knowledge of the internal code (developers)

---

### Section 7: Test Scenarios and Test Cases

**Test Scenario:** a high-level idea of what to test.
Example: *"Verify user can log in with valid credentials"*

**Test Case:** detailed step-by-step instructions with expected results.

**Test Case Anatomy:**
| Field | Description |
|---|---|
| Test Case ID | Unique identifier (TC-LOGIN-001) |
| Title | Short description |
| Preconditions | State required before starting |
| Test Steps | Numbered actions |
| Test Data | Inputs used (email, password) |
| Expected Result | What should happen |
| Actual Result | What happened |
| Status | Pass / Fail / Blocked |

**Example:**
```
TC-LOGIN-001
Title: Login with valid credentials
Preconditions: User is registered; on the login page
Steps:
  1. Enter valid email "user@test.com"
  2. Enter valid password "Pass123!"
  3. Click "Login"
Expected Result: User is redirected to the dashboard
Actual Result: (filled during execution)
Status: Pass
```

**Positive vs Negative Testing:**
- **Positive**: valid input → expected success ("valid email + password logs in")
- **Negative**: invalid input → graceful error ("wrong password shows error, no crash")

Always test both. Most bugs hide in negative and edge cases.

---

### Section 8: Test Design Techniques

Smart testers don't test everything — they pick inputs that cover the most risk.

**1. Equivalence Partitioning (EP)**
Divide inputs into groups where every value should behave the same. Test one value per group.

Example — age field accepting 18–60:
- Invalid: 17 (below range)
- Valid: 35 (inside range)
- Invalid: 75 (above range)

**2. Boundary Value Analysis (BVA)**
Bugs cluster at edges. For 18–60, test: 17, 18, 19 and 59, 60, 61.

**3. Decision Table**
For combinations of conditions. Example — discount:
- Member + coupon → 20%
- Member, no coupon → 10%
- Not member + coupon → 10%
- Neither → 0%

**4. Error Guessing**
Experience-based: try empty fields, special characters (`' OR 1=1`), emoji, very long strings, pasting into number fields, double-clicking submit, hitting Back after logout.

---

### Section 9: Bug Reports

**A bug report exists so a developer can reproduce and fix the problem without talking to you.**

**Bug Report Anatomy:**
| Field | Description |
|---|---|
| Bug ID | Unique identifier (BUG-1234) |
| Title | One-line summary: what + where + when |
| Environment | Browser/OS/device/version, test vs prod |
| Steps to Reproduce | Numbered, exact steps |
| Expected Result | What should happen |
| Actual Result | What happened |
| Severity | Impact on the system |
| Priority | How urgently it must be fixed |
| Attachments | Screenshots, video, console/network logs |

**Bad bug report:**
> "Login doesn't work. Please fix."

**Good bug report:**
```
BUG-1234
Title: Login button unresponsive on Safari mobile (iOS 17)
Environment: iPhone 14, iOS 17.2, Safari, staging build v2.4.1
Steps:
  1. Open https://staging.example.com on Safari mobile
  2. Enter valid email and password
  3. Tap "Login"
Expected: Redirected to dashboard
Actual: Nothing happens; no error shown. Console shows
        "TypeError: undefined is not an object (auth.js:42)"
Severity: Critical | Priority: High
Attachment: screen-recording.mp4, console-log.txt
```

**Severity vs Priority** (not the same!):
- **Severity** = how badly it breaks the system (Critical / Major / Minor / Trivial)
- **Priority** = how soon it must be fixed (High / Medium / Low)

Classic example: a typo on the home page logo = **low severity, high priority** (cosmetic but embarrassing). A crash on a feature nobody uses = **high severity, low priority**.

**Bug Life Cycle:**
```
New → Assigned → Open → Fixed → Retest → Closed
                  ↘ Rejected/Duplicate/Deferred
        (Retest fails) → Reopened → back to Open
```

---

### Section 10: Test Plan & RTM

**Test Plan** — the document describing the whole testing effort:
- Scope: what will and won't be tested
- Strategy: types and levels of testing
- Environment: browsers, devices, test data
- Schedule and responsibilities
- Entry criteria (when testing can start) / Exit criteria (when it's done)
- Risks

**RTM (Requirement Traceability Matrix):**
A table mapping each requirement to the test cases covering it — answers "did we test everything we promised?"

```
| Requirement | Test Cases       | Status |
| REQ-01 Login | TC-001..TC-008  | Pass   |
| REQ-02 Cart  | TC-009..TC-015  | 1 Fail |
```

---

### Section 11: Exploratory Testing

**Definition:**
Simultaneously learning the app, designing tests, and executing them — without pre-written scripts. A skilled tester's creativity is the tool.

**How to run a session:**
1. Write a **charter**: "Explore the checkout flow focusing on payment errors"
2. Time-box it (30–90 min)
3. Take notes: what you tried, what you found, questions raised
4. Convert important findings into real test cases and bug reports

**Useful heuristics:**
- CRUD operations (create/read/update/delete everything)
- Boundaries (0, -1, max, max+1)
- Interruptions (lose network mid-checkout, receive a call)
- Undo paths (Back button, cancel, refresh mid-action)

---

### Section 12: Testing Tools

| Tool | Purpose |
|---|---|
| Jira / GitHub Issues | Bug tracking and workflow |
| TestRail / Zephyr / Xray | Test case management |
| Postman | Manual API testing (requests/responses) |
| BrowserStack / real devices | Cross-browser & device testing |
| Browser DevTools | Console errors, network inspection (Session 13!) |

**Connection to Session 13:**
DevTools is a tester's best friend — check the **Console** for JS errors and the **Network** tab for failed API calls, and attach them to bug reports.

---

## Part 2: Practical Application (1.5 Hours)

### Exercise 1: Requirements → Test Scenarios (20 minutes)

**Task:**
Given this requirement, write at least 8 test scenarios.

> **REQ-LOGIN-01:** Users must log in with email and password. Password minimum 8 characters. After 5 failed attempts, the account locks for 15 minutes.

**Example answers:**
```
TS-01: Login with valid email + valid password → success
TS-02: Login with valid email + wrong password → error message
TS-03: Login with unregistered email → error message
TS-04: Login with empty fields → validation messages
TS-05: Password with 7 characters → rejected
TS-06: Password with exactly 8 characters → accepted
TS-07: 5 failed attempts → account locks
TS-08: Login during lock period → blocked with lock message
TS-09: Login after lock expires → works again
TS-10: Password field masks input (shows •••)
```

---

### Exercise 2: Write Detailed Test Cases (25 minutes)

**Task:**
Turn 5 of your scenarios into full test cases using the template below.

```
| TC-ID | Title | Preconditions | Steps | Test Data | Expected | Actual | Status |
```

**One example to start:**
```
TC-LOGIN-003 | Empty email field shows validation error
Preconditions: On login page
Steps:
  1. Leave email empty
  2. Enter valid password "Pass123!"
  3. Click Login
Test Data: password = "Pass123!"
Expected: "Email is required" appears; no login request sent
Actual:
Status:
```

---

### Exercise 3: EP + BVA Practice (20 minutes)

**Task:**
A discount field accepts ages **18–60 inclusive**. Using Equivalence Partitioning and Boundary Value Analysis, list the exact test values you would enter.

**Expected answer:**
```
EP groups:  <18 (invalid) | 18–60 (valid) | >60 (invalid)
EP picks:   10            | 35            | 75
BVA edges:  17, 18, 19    | 59, 60, 61
Combined:   10, 17, 18, 19, 35, 59, 60, 61, 75
```

Bonus: What happens with `18.5`? `abc`? empty? `-5`? `999999999999`? Add them.

---

### Exercise 4: Write a Bug Report (15 minutes)

**Task:**
Scenario: On the registration page, clicking "Sign Up" twice quickly creates two identical accounts and sends two welcome emails.

Write a complete bug report: title, environment, steps, expected, actual, severity, priority.

**Discuss:** What severity/priority would you assign, and why? (Typical: Major severity — duplicate data; Medium priority — requires fast double-click.)

---

### Exercise 5: Exploratory Testing Session (20 minutes)

**Task:**
Pick a demo site (e.g., `https://www.saucedemo.com` — a site built for practice, credentials shown on the page).

1. Write a charter: *"Explore login and cart for 15 minutes"*
2. Take notes while testing — try edge cases: locked user (`locked_out_user`), removing items, refreshing mid-checkout, Back button after logout
3. Log at least 2 observations or potential bugs
4. Convert one finding into a formal bug report

---

## Part 3: Review, Questions, Problem Solving (0.5 Hours)

### Common Testing Mistakes (10 minutes)

❌ Vague bug reports ("it doesn't work")

❌ Only positive testing — never trying invalid input

❌ No edge cases — testing `25` but never `17`, `18`, `60`, `61`

❌ Assuming developers already tested everything

❌ Testing without documenting — no test cases, no evidence

❌ Testing directly on production

❌ Reporting a bug with no environment info ("works on my machine" in reverse)

❌ Confusing severity with priority

❌ Skipping regression — "we only changed one small thing"

❌ Not retesting after a fix

**Better habits:**
```
✔ Every bug report has steps + expected vs actual + environment
✔ Test valid AND invalid input AND boundaries
✔ Write scenarios before testing, cases before release
✔ Always retest fixes and run regression before release
```

---

### Review Questions (10 minutes)

**What is software testing?**
Evaluating an application to find defects and verify it meets requirements.

**Difference between verification and validation?**
- Verification: "Did we build the product right?" (matches spec)
- Validation: "Did we build the right product?" (solves the user's need)

**Difference between smoke and sanity testing?**
Smoke is wide and shallow (is the build usable?); sanity is narrow and deep (does this fix work?).

**Difference between severity and priority?**
Severity = impact on the system. Priority = urgency of the fix. They are independent.

**What is regression testing?**
Re-running existing tests to make sure new changes didn't break old functionality.

**Difference between a test scenario and a test case?**
A scenario is a high-level "what to test"; a test case is the detailed "how to test" with steps, data, and expected results.

**What is a test plan?**
A document describing scope, strategy, environment, schedule, and entry/exit criteria for testing.

**What should every bug report contain?**
Title, environment, steps to reproduce, expected result, actual result, severity, priority, attachments.

**What is equivalence partitioning?**
Dividing inputs into groups that should behave identically, then testing one representative per group.

**What is boundary value analysis?**
Testing the edges of input ranges where defects cluster (e.g., for 18–60: 17, 18, 59, 60, 61).

**What is exploratory testing?**
Simultaneous learning, test design, and execution without pre-scripted cases — guided by a charter and time-boxed.

**What is the difference between retesting and regression?**
Retesting = re-checking a specific fixed bug. Regression = checking nothing else broke.

**Can testing prove software is bug-free?**
No — testing shows the presence of defects, never their absence.

---

### Practice Challenges (5 minutes)

**Challenge 1**
Write 6 test scenarios for a "Forgot Password" feature.

**Challenge 2**
A text field accepts usernames of 4–16 characters. List your BVA test values.

**Challenge 3**
Write a bug report for: "On the checkout page, the total shows $NaN when the cart is emptied via the Back button."

---

### Homework Assignment

**Task:**
Pick a real website or a demo app (e.g., saucedemo.com, a small personal project).

**Requirements:**
- Write a mini test plan (scope, types, environment — half a page)
- Write at least **10 test scenarios**
- Write at least **15 detailed test cases** covering positive, negative, and boundary cases
- Execute them and record actual results
- Write at least **3 bug reports** (real or simulated)
- Write a short test summary report: what passed, what failed, risks

**Deliverable:** One folder with `test-plan.md`, `test-cases.md`, `bug-reports.md`, `summary.md`

**Due Date:** Next session

---

## Templates

### Test Case Template
```
TC-ID:
Title:
Module:
Preconditions:
Steps:
  1.
  2.
Test Data:
Expected Result:
Actual Result:
Status: Pass / Fail / Blocked
Severity (if failed):
```

### Bug Report Template
```
Bug ID:
Title: [what] + [where] + [when]
Environment: (browser/OS/device/build/URL)
Steps to Reproduce:
  1.
  2.
Expected Result:
Actual Result:
Severity: Critical / Major / Minor / Trivial
Priority: High / Medium / Low
Attachments: (screenshots, video, logs)
```

### Test Plan Outline
```
1. Scope (in scope / out of scope)
2. Testing types and levels
3. Test environment (browsers, devices, data)
4. Entry criteria / Exit criteria
5. Schedule and responsibilities
6. Risks
```

---

## End of Session 14

## 🎉 Complete Course Completion!

**You have successfully completed the entire course!**

**Course Summary:**
- Sessions 1-8: CSS Fundamentals to Advanced
- Sessions 9-10: Tailwind CSS Fundamentals & Advanced
- Session 11: Git Fundamentals
- Session 12: Terminal & npm
- Session 13: DevTools and Debugging
- Session 14: Manual Testing

**Final Achievement:**
You now have comprehensive knowledge of:
- CSS (Traditional + Tailwind)
- Version Control (Git)
- Development Tools (Terminal, npm, DevTools)
- Software Testing (test cases, bug reports, testing types, QA workflow)

**Next Steps:**
- Build real-world projects and test them
- Learn API testing in depth (Postman)
- Explore test automation (Playwright, Cypress, Selenium)
- Learn JavaScript frameworks (React, Vue, etc.)
- Contribute to open source
- Build your portfolio

**Happy Testing! 🚀**

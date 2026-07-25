---

# 📘 SQA INTERVIEW REVISION SHEET (Topics 1–8)

---

## TOPIC 1: REST API & HTTP FUNDAMENTALS

### What is an API / REST?
An API (Application Programming Interface) lets two systems talk to each other. REST (Representational State Transfer) is an architectural style where each resource (a user, an order, a course) is accessed via a URL, and you act on it using standard HTTP methods. REST APIs are stateless — the server doesn't remember you between requests; every request must carry all the info needed (like an auth token).

### HTTP Methods
| Method | Purpose | Idempotent? | Body? |
|---|---|---|---|
| GET | Retrieve data | Yes | No |
| POST | Create a new resource | No | Yes |
| PUT | Replace a resource entirely | Yes | Yes |
| PATCH | Update part of a resource | No (usually) | Yes |
| DELETE | Remove a resource | Yes | Usually no |

**Idempotent** = calling it once or 100 times gives the same end state. GET, PUT, DELETE are idempotent. POST is not (calling it twice creates two resources).

### HTTP Status Codes (memorize these cold)
- **2xx – Success**
  - `200 OK` – standard success (GET/PUT/PATCH)
  - `201 Created` – successful POST that created something
  - `204 No Content` – success, but nothing to return (e.g., successful DELETE)
- **4xx – Client's fault**
  - `400 Bad Request` – malformed request (missing field, wrong data type)
  - `401 Unauthorized` – you haven't proven who you are (no/invalid token)
  - `403 Forbidden` – you're known, but not allowed to do this
  - `404 Not Found` – wrong URL / resource doesn't exist
  - `422 Unprocessable Entity` – syntactically correct but fails validation rules (e.g., valid JSON, but email format is invalid)
- **5xx – Server's fault**
  - `500 Internal Server Error` – server code crashed
  - `503 Service Unavailable` – server is overloaded/down for maintenance

**401 vs 403 — the classic gotcha:** 401 = "I don't know who you are." 403 = "I know who you are, you're just not allowed here."
**400 vs 422 — a common follow-up:** 400 usually means the request itself is broken (bad JSON, wrong type). 422 means the request is well-formed but violates a business rule (e.g., password too short).

### Authentication Basics
- **API Key** – a static string identifying the calling app, sent in a header (`x-api-key`). Simple, but doesn't identify individual users well.
- **Bearer Token / JWT** – sent as `Authorization: Bearer <token>`. A JWT (JSON Web Token) has 3 parts separated by dots: `header.payload.signature` — the header says the algorithm, the payload holds claims (user id, role, expiry), the signature proves it wasn't tampered with. You should be able to say: "JWTs are not encrypted, only signed — anyone can decode the payload, so never put secrets in it."
- **Session-based auth** – server stores session state and gives you a cookie; stateful, less common in modern APIs.

### Request/Response Structure
- **Request:** Method + URL + Headers (Content-Type, Authorization) + Body (JSON, for POST/PUT/PATCH)
- **Response:** Status code + Headers + Body (JSON payload)

---

## TOPIC 2: API SECURITY TESTING (OWASP API Security Top 10 — 2023)

Study this as "attack pattern + one-line fix," not textbook definitions.

1. **API1 – Broken Object Level Authorization (BOLA)**
   Change an ID in the request (`/orders/1234` → `/orders/1235`) and get someone else's data. The #1 API vulnerability — most common, easiest to test, most damaging. Fix: check ownership/permissions server-side on every single request, never trust the client.
2. **API2 – Broken Authentication**
   Weak login flow — no token expiry, brute-forceable passwords, missing MFA, tokens reused after logout. Lets an attacker impersonate a real user.
3. **API3 – Broken Object Property Level Authorization**
   A user can view or edit *fields* they shouldn't — e.g., a Student PATCHes their own profile and slips `"role": "admin"` into the JSON body, and the backend blindly accepts it (this is "mass assignment"). Or an API response leaks internal fields like `passwordHash` that the frontend never needed.
4. **API4 – Unrestricted Resource Consumption**
   No rate limiting / no size limits — you can hammer an endpoint thousands of times per second, or upload a 10GB file with no cap, and the server never says no.
5. **API5 – Broken Function Level Authorization (BFLA)**
   Different from BOLA — here it's not about *which object*, it's about *which action/endpoint*. A regular Student calling `DELETE /admin/users/5` directly and it actually works, because only the UI hides the button — the backend never checks the role.
6. **API6 – Unrestricted Access to Sensitive Business Flows**
   No protection against a script abusing a legitimate flow at scale — e.g., a bot scripting account creation to snipe every limited discount code before real users can.
7. **API7 – Server-Side Request Forgery (SSRF)**
   The API takes a URL from the user (e.g., "fetch this image from this link for my avatar") and the server fetches it blindly — an attacker can point that at internal-only infrastructure the server can reach but the public can't.
8. **API8 – Security Misconfiguration**
   Catch-all: verbose stack traces in error responses, default admin passwords left on, unnecessary HTTP methods enabled (e.g., `TRACE` on production), missing security headers.
9. **API9 – Improper Inventory Management**
   Old, forgotten API versions (`/api/v1/login`) still live and unpatched next to the current one, expanding the attack surface nobody's watching.
10. **API10 – Unsafe Consumption of APIs**
    Blindly trusting data coming back from a third-party API (e.g., a payment gateway) without validating it — if that third party is compromised, you inherit the problem.

**Memory grouping:** Access Control (1, 3, 5) → Identity (2) → Abuse/Scale (4, 6) → Infrastructure (7, 8, 9) → Trust boundaries (10).

**Practical test techniques to have ready:**
- SQL injection probe string: `' OR 1=1 --` in login/search fields — a safe system shows a generic error, an unsafe one logs you in or leaks a DB error.
- Swap a user ID/token between two accounts in Postman to test BOLA.
- Try calling an admin endpoint with a Student's token to test BFLA.
- Send extra unexpected fields in a PATCH body (`role`, `isAdmin`) to test mass assignment.
- Send 100 rapid requests to a login endpoint to check for rate limiting.

---

## TOPIC 3: API PERFORMANCE & STRESS TESTING

### The Types (don't mix these up)
| Type | What it does | Goal |
|---|---|---|
| **Performance Testing** | Measures speed/responsiveness under normal, expected conditions | Baseline: "is it fast enough?" |
| **Load Testing** | Simulates expected real-world concurrent users (e.g., 1,000) | "Can it handle normal peak traffic?" |
| **Stress Testing** | Pushes far beyond expected load (e.g., 20,000 users) until it breaks | "Where's the breaking point, and how does it fail?" |
| **Spike Testing** | Sudden, sharp burst of traffic (0 to 10,000 in seconds) | "Can it survive a viral moment / flash sale?" |
| **Soak Testing** (Endurance) | Moderate load sustained for a long time (hours/days) | "Does performance degrade over time?" (memory leaks) |

### Key Metrics to Know
- **Response Time / Latency** – time for one request to complete
- **Throughput / TPS (Transactions Per Second)** – how many requests the system handles per second
- **Error Rate** – % of requests failing under load
- **P95 / P99 latency** – "95% (or 99%) of requests finished faster than this" — more meaningful than average, because averages hide the worst-case slow requests
- **Concurrent Users** – how many users are active simultaneously
- **Ramp-up Period** – how gradually load is increased (sudden vs. gradual reveals different bugs)

### Tools
- **JMeter** – classic, GUI-based, widely used for load/stress testing
- **k6** – modern, script-based (JavaScript), popular for CI/CD-integrated performance tests
- **Postman Runner / Newman** – can simulate repeated requests for lightweight checks
- **Locust** – Python-based load testing tool

### What a good answer sounds like
"I'd use k6 or JMeter to simulate 1,000 concurrent users hitting the checkout endpoint, watch the P95 response time and error rate, then gradually increase load to find the breaking point — that tells the team our actual capacity ceiling before a real traffic spike does it for us."

---

## TOPIC 4: FEATURE TESTING & EDGE-CASE DISCOVERY

### STLC vs SDLC (quick recap)
SDLC = the development lifecycle (requirements → design → code → test → deploy). STLC = the testing lifecycle, running in parallel: Requirement Analysis → Test Planning → Test Case Development → Environment Setup → Test Execution → Test Cycle Closure. Testing isn't a phase after coding — it happens alongside it ("Shift-Left").

### Equivalence Partitioning (EP)
Divide inputs into groups ("partitions") where the system should behave the same way, and test just **one representative value per group** instead of every possible value.
Example — age field accepting 18–60: partitions are *Invalid-low* (<18), *Valid* (18–60), *Invalid-high* (>60). Pick one value from each: 10, 30, 70.

### Boundary Value Analysis (BVA)
Most bugs live at the **edges** of a range (off-by-one errors: `>` vs `>=`). Test the exact boundary, and one value just inside and just outside it.
Example — same 18–60 field: test **17, 18, 19, 59, 60, 61**. You skip the "safe middle" (like 30) because BVA + EP together already covers it via the partition test.

### Functional vs Negative Testing
- **Positive/Functional testing** – valid inputs, happy path, "does it work as intended?"
- **Negative testing** – invalid inputs, verifying graceful error handling ("does it fail safely instead of crashing?"). Spend at least 50% of your test design time here — this is where real bugs hide.

### Test Scenario vs Test Case vs Test Script
- **Scenario** – one-line goal: "Verify user can log in."
- **Test Case** – detailed steps + expected result for one specific path of that scenario.
- **Test Script** – the same case, automated into code (Playwright/Selenium).
One scenario typically spawns 5–10 test cases (valid login, invalid password, blank fields, SQL injection attempt, etc.)

### Decision Table & State Transition (quick recap)
- **Decision Table** – used when behavior depends on multiple combined conditions ("IF Premium AND Cart>$100 AND ValidCode THEN 20% off") — map every True/False combination.
- **State Transition** – used for workflows with defined states (Order: Placed → Confirmed → Shipped → Delivered) — test valid transitions AND invalid ones (can't go Placed → Shipped directly, can't cancel after Shipped).

---

## TOPIC 5: SPOTTING WHETHER DEVELOPERS FOLLOW BEST PRACTICES

### Root Cause Analysis (RCA)
RCA means tracing a **Failure** back through the **Defect** to the original human **Error**, instead of just patching the symptom. Process:
1. **Reproduce** the failure reliably (exact steps).
2. **Isolate variables** — does it happen on every browser? Every user? Every environment? Change one variable at a time.
3. **Trace backward** — check logs, use `git blame` to see which commit introduced the change, check the database state.
4. **Ask "why" repeatedly** (the "5 Whys" technique) until you hit the actual root cause, not just the surface symptom.
   Example: *Checkout crashes* → why? → *null pointer on tax calculation* → why? → *tax field wasn't set for guest users* → why? → *guest checkout wasn't included in the original requirements*. Root cause: a requirements gap, not just a code bug.
5. Report the root cause to the developer, not just "it's broken" — this is what separates a strong QA from someone who just clicks buttons.

### Severity vs Priority (recap — high-yield question)
- **Severity** = technical impact, decided by QA. (Does it crash? Corrupt data? Or is it cosmetic?)
- **Priority** = business urgency, decided by the Product Owner. (Does it need fixing today, or can it wait?)
- Classic example to have ready: a typo "Sing Up" instead of "Sign Up" during a big ad campaign = **Low Severity, High Priority**.

### Bug Reporting Best Practices
A good bug report needs zero follow-up questions from the developer. Include:
- **Steps to reproduce** (numbered, exact)
- **Expected result** (tied to the requirement/AC, not your opinion)
- **Actual result** (what really happened)
- **Environment** (browser, OS, staging vs prod)
- **Severity** (your technical judgment)
- **Evidence** (screenshot, screen recording, console log, HAR file, API response)

### Browser Cache & Frontend State Management (this one isn't in your README, so pay attention)
This is about testing bugs that only appear because of **stale data on the client side**, not the server:
- **HTTP Caching** – `Cache-Control` and `ETag` headers tell the browser how long to reuse a cached response instead of asking the server again. A tester should know: if a developer changes an API response but forgets to update caching headers, users might see stale data until they hard-refresh.
- **Browser storage types you must be able to distinguish:**
  - `Cookies` – small, sent automatically with every request, used for sessions.
  - `localStorage` – persists even after closing the browser, no expiry unless cleared manually.
  - `sessionStorage` – cleared when the tab closes.
- **Common bugs this causes:** a user updates their profile picture, but an old cached version still shows until hard refresh; a logged-out user still sees dashboard data because a token wasn't properly cleared from localStorage; stale cart data persists across sessions incorrectly.
- **How you'd test it:** test in Incognito/Private mode (no cache/cookies) vs. a normal session; do a hard refresh (`Ctrl+Shift+R`) vs. soft refresh; inspect Application tab in DevTools to check what's actually stored in Cookies/localStorage/sessionStorage after an action.
- **What a good answer sounds like:** "After I make a change, I check if it's really persisted server-side (grey-box, via API/DB) or if it just looks fixed because of a cached response. I also check the Application tab in DevTools to see what's sitting in localStorage after logout — leftover tokens there are a security bug."

---

## TOPIC 6: AUTOMATED TESTING (PLAYWRIGHT / SELENIUM)

### CSS Selectors (syntax you must recognize instantly)
| Selector | Meaning |
|---|---|
| `#id` | Element with that ID |
| `.class` | Element with that class |
| `div p` | Any `p` inside a `div` (descendant, any depth) |
| `div > p` | `p` that is a *direct* child of `div` |
| `div + p` | `p` immediately after a `div` (sibling) |
| `[type="submit"]` | Attribute selector |
| `button:nth-child(2)` | Pseudo-class (position-based) |

### XPath (this comes up a LOT — practice writing these by hand)
- **Absolute XPath:** `/html/body/div[1]/form/button` — starts from root, breaks the moment HTML structure changes. Avoid in real automation.
- **Relative XPath:** `//button[@id='loginBtn']` — starts with `//`, searches anywhere in the DOM, much more resilient.
- **Axes** (navigate relationships): `parent::`, `child::`, `following-sibling::`, `ancestor::`, `descendant::`
  Example: `//input[@id='email']/following-sibling::span` — find the error message span right after the email input.
- **Functions:**
  - `contains(@class, 'btn')` – partial match, useful when class names have multiple values
  - `text()='Submit'` – exact visible text match
  - `starts-with(@id, 'user-')` – partial match at the start of an attribute

### HTML DOM Structure
The DOM is the browser's live, in-memory tree representation of the HTML — it's what your locators actually query, not the static HTML source. When you inspect an element in DevTools, you're looking at the DOM, which can differ from "View Source" if JavaScript modified the page after load.

### Dynamic Element Handling
Modern apps have elements that appear/disappear/change after JS runs (loading spinners, modals, infinite scroll). Best practices:
- Prefer `data-testid="submit-btn"` — a stable attribute developers add specifically for testing, immune to CSS/styling changes.
- Or `aria-label="Close"` — an accessibility attribute that's also stable and DOM-change-resistant.
- Avoid selectors based on visual class names (`.btn-primary-v2`) — these change whenever a designer touches the CSS.
- For elements that load asynchronously, rely on Playwright's **auto-waiting** (it retries until the element is visible/stable/enabled) rather than hardcoded `sleep()`/`waitForTimeout()`.

### Playwright Quick Recap
- `getByRole`, `getByText`, `getByTestId` > raw CSS/XPath — resilient to DOM restructuring.
- Auto-waiting checks: attached, visible, stable, enabled, receiving events — before every action.
- Always `await` every action — forgetting it causes race conditions (test checks the next page before the click/navigation finished).
- Page Object Model (POM): one class per page holding locators + methods, so a UI change means updating one file, not fifty tests.

### Selenium Quick Recap (if asked to compare)
- **Implicit Wait** – one global timeout applied to all `findElement` calls.
- **Explicit Wait** – `WebDriverWait` targeting a specific condition on a specific element.
- Selenium can throw `StaleElementReferenceException` when the DOM re-renders after you've stored an element reference — Playwright avoids this because it re-queries the DOM live via locators instead of holding element references.

---

## TOPIC 7: SUPPORTING TECHNICAL SKILLS (JS/TS + ALGORITHMS)

### JavaScript/TypeScript Basics
**Variables:**
```javascript
let count = 5;        // can be reassigned
const name = "John";  // cannot be reassigned
```
**Functions:**
```javascript
function add(a, b) { return a + b; }
const add = (a, b) => a + b;  // arrow function
```
**Loops:**
```javascript
for (let i = 0; i < 5; i++) { console.log(i); }
for (const item of array) { console.log(item); }
while (condition) { /* ... */ }
```
**Conditions:**
```javascript
if (age >= 18) { /* ... */ } else { /* ... */ }
const status = isActive ? "Active" : "Inactive"; // ternary
```
**Useful array methods (common in test scripts):**
```javascript
array.map(x => x * 2);          // transform each item
array.filter(x => x > 10);      // keep matching items
array.find(x => x.id === 5);    // first match
```
**Async/Await (critical for Playwright):**
```javascript
async function login(page) {
  await page.fill('#email', 'test@test.com');
  await page.click('#loginBtn');
}
```

### Basic Algorithms (classic coding-round questions)
**Palindrome check:**
```javascript
function isPalindrome(str) {
  const clean = str.toLowerCase().replace(/[^a-z0-9]/g, '');
  return clean === clean.split('').reverse().join('');
}
```
**Reverse a string:**
```javascript
function reverseString(str) {
  return str.split('').reverse().join('');
}
```
**Factorial:**
```javascript
function factorial(n) {
  if (n <= 1) return 1;
  return n * factorial(n - 1);
}
```
**Prime number check:**
```javascript
function isPrime(n) {
  if (n < 2) return false;
  for (let i = 2; i <= Math.sqrt(n); i++) {
    if (n % i === 0) return false;
  }
  return true;
}
```
Practice writing these **from memory, on paper or in a plain editor with no autocomplete** — that's how they're usually tested live.

---

## TOPIC 8: TEST DOCUMENTATION SKILL

### Professional Test Case Format
A complete test case should include:
1. **Test Case ID** (e.g., `TC_LOGIN_001`)
2. **Title** – what it verifies
3. **Pre-conditions** – what must be true before starting
4. **Test Steps** – exact, numbered
5. **Test Data** – specific inputs used
6. **Expected Result** – tied to the requirement
7. **Actual Result** – filled in after execution
8. **Status** – Pass / Fail / Blocked

**Example:**
- ID: `TC_CART_001`
- Title: Verify adding one item to cart with default quantity
- Pre-conditions: User logged in; product in stock
- Steps: (1) Go to product page (2) Confirm qty = 1 (3) Click "Add to Cart" (4) Check cart badge
- Expected: Success message appears; cart badge updates 0 → 1
- Status: Pass/Fail

### Bug Report Format
- **Summary** – one clear line
- **Steps to Reproduce** – numbered, exact
- **Expected vs Actual Result**
- **Environment** – browser/OS/staging or prod
- **Severity** (yours) / **Priority** (PO's)
- **Attachments** – screenshot, video, logs

### Test Plan Components (recap)
Scope (in/out), Test Strategy (tools, manual vs automation), Entry Criteria (conditions to start), Exit Criteria (conditions to stop — e.g., "zero Critical bugs open, 95%+ pass rate"), Resources, Schedule, Risks.

### Requirement Traceability Matrix (RTM)
A matrix linking each requirement to the test case(s) that validate it — used to prove there are no coverage gaps. If a requirement has zero mapped test cases, that's a red flag.

---
This guide is your comprehensive preparation resource for the SQA engineering test. It covers the five core areas you'll be evaluated on: **API Security Testing (OWASP Top 10), API Performance & Stress Testing, Feature Testing & Edge Cases, Developer Best Practices, and Automated Testing with Playwright.** Each section is structured to provide the deep, practical knowledge expected of an engineer with a strong testing mindset.

---

### 1. API Security Testing (OWASP API Security Top 10)

APIs expose application logic and sensitive data, making them prime targets for attackers. The **OWASP API Security Top 10** is the definitive guide to the most critical API security risks. Your task is to not just find functional bugs, but to proactively identify these vulnerabilities before they can be exploited.

Here is the **OWASP API Security Top 10 – 2023** and how to test for each:

1.  **API1:2023 - Broken Object Level Authorization (BOLA)**
    - **The Risk:** APIs expose endpoints that handle object identifiers (like `user_id` or `order_id`), creating a wide attack surface. If the API fails to verify that the logged-in user has permission to access that specific object, an attacker can simply change the ID in the request to access another user's data.
    - **Testing Strategy:**
        - Log in as User A. Obtain a valid object ID (e.g., `GET /orders` to get `order_id=123`).
        - Log in as User B (or use an unauthenticated request). Attempt to access `GET /orders/123`.
        - **Pass Criteria:** The API returns a `403 Forbidden` or `404 Not Found`. It **must not** return User A's order data.

2.  **API2:2023 - Broken Authentication**
    - **The Risk:** Authentication mechanisms are often implemented incorrectly. This can allow attackers to compromise tokens or exploit flaws to assume other users' identities.
    - **Testing Strategy:**
        - **Token Strength:** Test if JWTs are properly signed and not using `none` algorithm.
        - **Token Expiry:** Attempt to use an expired token.
        - **Weak Credentials:** Test for default passwords, lack of rate limiting on login, and missing account lockout policies.
        - **Pass Criteria:** The API strictly enforces authentication and rejects all requests with missing, invalid, or expired credentials with a `401 Unauthorized`.

3.  **API3:2023 - Broken Object Property Level Authorization**
    - **The Risk:** This combines Excessive Data Exposure and Mass Assignment. The API might return more data than necessary (e.g., a user's password hash), or it might allow a client to modify properties they shouldn't have access to (e.g., changing `"role":"user"` to `"role":"admin"` in a `PUT /users/123` request).
    - **Testing Strategy:**
        - **Excessive Data Exposure:** Call an endpoint to get a user's profile. Check the response for any sensitive fields that shouldn't be exposed (e.g., `password_hash`, `credit_card_number`, `internal_id`).
        - **Mass Assignment:** Send a `PUT` or `PATCH` request to update a user, but include a property you shouldn't be able to change, like `"role": "admin"`.
        - **Pass Criteria:** Sensitive fields are not returned. The API ignores or rejects unauthorized property changes.

4.  **API4:2023 - Unrestricted Resource Consumption**
    - **The Risk:** APIs require resources like network, CPU, and memory. Without limits, attackers can cause a Denial of Service (DoS) or drive up operational costs.
    - **Testing Strategy:**
        - **Rate Limiting:** Send a high volume of requests to an endpoint in a short period. Does the API start returning `429 Too Many Requests` after a certain threshold?
        - **Large Payloads:** Send a POST request with a massive JSON payload (e.g., 10MB). Does the API handle it gracefully or crash?
        - **Pass Criteria:** The API enforces rate limits and rejects oversized payloads with appropriate error messages.

5.  **API5:2023 - Broken Function Level Authorization**
    - **The Risk:** Complex access control policies can lead to flaws. Attackers can gain access to administrative functions by guessing or exploiting endpoints that weren't properly secured.
    - **Testing Strategy:**
        - Log in as a regular user.
        - Attempt to access admin-level endpoints directly, e.g., `DELETE /admin/users`, `GET /admin/audit-logs`.
        - **Pass Criteria:** The API returns a `403 Forbidden` for all admin functions when accessed by non-admin users.

6.  **API6:2023 - Unrestricted Access to Sensitive Business Flows**
    - **The Risk:** This is a business logic flaw. The API exposes a sensitive business flow (e.g., "buy a ticket") without compensating for how it could be abused in an automated fashion.
    - **Testing Strategy:**
        - Automate a critical flow (e.g., applying a discount coupon) and run it hundreds of times. Does the system have checks to prevent this abuse?
        - **Pass Criteria:** The application has mechanisms (like rate limiting, CAPTCHA, or business rules) to prevent automated abuse of critical flows.

7.  **API7:2023 - Server Side Request Forgery (SSRF)**
    - **The Risk:** The API fetches a remote resource without validating the user-supplied URI. An attacker can coerce the application to send a crafted request to an unexpected internal or external destination.
    - **Testing Strategy:**
        - If an endpoint accepts a URL (e.g., `POST /fetch-website` with `{"url": "https://example.com"}`), try to supply internal IP addresses (e.g., `http://169.254.169.254/` for AWS metadata or `http://localhost:8080/admin`).
        - **Pass Criteria:** The API validates and sanitizes the URL, preventing requests to internal networks.

8.  **API8:2023 - Security Misconfiguration**
    - **The Risk:** APIs and their supporting systems have complex configurations that can be missed. This includes default configurations, open cloud storage, verbose error messages, and missing security headers.
    - **Testing Strategy:**
        - **Check Headers:** Verify security headers like `Content-Security-Policy`, `Strict-Transport-Security`, and `X-Content-Type-Options` are present.
        - **Error Messages:** Send a malformed request. Does the API return a stack trace or database error (bad) or a generic "Bad Request" (good)?
        - **CORS Policy:** Check if the `Access-Control-Allow-Origin` header is too permissive (e.g., `*`).
        - **Pass Criteria:** Security headers are set, error messages are generic, and CORS policies are tightly controlled.

9.  **API9:2023 - Improper Inventory Management**
    - *(While not explicitly detailed in the provided text, it's a standard OWASP API Top 10 risk.)* It refers to exposing older, unpatched API versions or endpoints that are not properly documented or monitored.
    - **Testing Strategy:** Scan for exposed API endpoints that are not documented or are from older versions (e.g., `/api/v1/users` vs `/api/v2/users`). Test if the old version is still active and vulnerable.

10. **API10:2023 - Unsafe Consumption of APIs**
    - **The Risk:** Developers tend to trust data received from third-party APIs more than user input, adopting weaker security standards. Attackers can compromise the third-party service to attack the target API.
    - **Testing Strategy:** This is harder to test directly but involves reviewing the code for how third-party responses are handled. The tester should check if the application properly validates and sanitizes data received from external APIs.

**The Testing Mindset:** An engineer with a security mindset doesn't just test the happy path. They think like an attacker, trying to subvert the intended logic of the API to gain unauthorized access or cause damage.

---

### 2. API Performance and Stress Testing

An API can be functionally perfect and still fail in production. Performance testing ensures it is fast and reliable under real-world traffic. Your goal is to identify bottlenecks and breaking points *before* users do.

#### Key Types of Performance Tests
| Test Type | Objective |
| :--- | :--- |
| **Load Testing** | Simulates expected normal and peak traffic to confirm the API handles it within acceptable response times. |
| **Stress Testing** | Pushes the system beyond expected limits until it fails. The goal is to find the breaking point and see how it fails—does it slow gracefully or crash catastrophically? |
| **Spike Testing** | Applies a sudden, sharp increase in traffic (e.g., from a viral moment) to see if the system can absorb it or collapses. |
| **Soak/Endurance Testing** | Holds a moderate load for a long period to expose memory leaks, connection pool exhaustion, or log file issues that only appear over time. |
| **Smoke Testing** | A lightweight pre-check with a small load to confirm the test setup is working before a major test run. |

#### Key Metrics to Monitor
- **Response Time (Percentiles):** Don't look at the average; look at the **95th and 99th percentiles**. The slowest 5% or 1% of requests are what real users feel.
- **Throughput:** The number of requests the API completes per second (RPS). This tells you the real capacity of the system.
- **Error Rate:** The percentage of failed requests under load.
- **Concurrent Users / Virtual Users:** How many simultaneous users the test simulates.

#### Best Practices for Performance Testing
1.  **Design Realistic User Flows:** Don't just hit one endpoint. Simulate real user behavior (e.g., `Login → Search → View Product → Add to Cart → Checkout`).
2.  **Use Dynamic Data:** Every virtual user should send unique data (e.g., different user IDs, search terms) to avoid caching that would mask real-world issues.
3.  **Start Early and Test Often:** Catch performance regressions in development, not production. Integrate performance tests into your CI/CD pipeline.
4.  **Define Clear SLOs (Service Level Objectives):** Set clear targets (e.g., "95% of responses under 300ms"). Fail the test in CI if these are breached.
5.  **Monitor Everything:** Look at more than just response time. Monitor CPU, RAM, database latency, cache hit ratios, and error logs to pinpoint the root cause of bottlenecks.
6.  **Ramp Up Gradually:** Don't flood your API with all users at once. Use a gradual ramp-up to see how the system handles increasing pressure.

#### Performance Testing Tools
- **k6:** Modern, open-source, scriptable in JavaScript. Great for CI/CD.
- **JMeter:** The classic, Java-based tool. Powerful but can be complex.
- **Gatling:** Code-driven, high-performance tool for Scala/Java developers.
- **Locust:** Python-based, good for complex scenarios.

---

### 3. Feature Testing and Finding Edge Cases

This tests your core engineering mindset: the ability to think beyond the obvious "happy path" and systematically destroy a feature to find hidden bugs.

**The Engineer's Approach:**
You don't just test a feature; you analyze it, break it down into input variables, and apply systematic test design techniques.

#### Key Test Design Techniques
- **Equivalence Partitioning (EP):** Divide input data into groups (partitions) where the system should behave the same way. Test only one representative value from each group.
    - *Example:* For an age field (18-60), valid partition: `30`; invalid partitions: `10` (too young), `70` (too old).
- **Boundary Value Analysis (BVA):** Test the exact edges of those partitions. 60-70% of bugs are found here.
    - *Example:* For the age field (18-60), test the boundaries: `17`, `18`, `59`, `60`, `61`.
- **State Transition Testing:** Test how the system behaves as it moves through different states.
    - *Example:* Testing the "Order" lifecycle: `Created → Paid → Shipped → Delivered`.

#### The "Zero-One-Many" Rule
A powerful way to construct test cases is the Zero-One-Many rule. For any feature, test the scenarios where a specific element occurs **zero** times, **one** time, and **many** times.
- *Example (Shopping Cart):* Test adding **zero** items (empty cart), **one** item, and **many** items to the cart.

#### Spotting Edge Cases (The "What If?" Mindset)
An SQA engineer relentlessly asks "What if?" Consider these categories:
- **Extremal Values:** The maximum, minimum, and just outside the limits.
- **Special Values:** `0`, `null`, `undefined`, `""`, `" "` (space), `true`/`false`.
- **Invalid Combinations:** Sending a request where two fields are mutually exclusive or contradictory.
- **Concurrency:** What if two users try to book the last seat on a flight at the exact same time?
- **External Failures:** What if the payment gateway is down? What if the database times out?
- **Data Corruption:** What if you send an image where a string is expected, or special characters like `!@#$%^&*()`?

**The Ultimate Goal:** Find the hidden, corner-case bugs that a junior tester would miss, ensuring the software is robust under any condition, not just the perfect one.

---

### 4. Spotting Whether Developers Are Following Best Practices

As an SQA engineer with an "engineering mindset," you must go beyond testing *what* the code does and evaluate *how* it was written. This is a form of **code review**. The goal is to find defects before they reach production, share knowledge, and maintain standards.

#### A Code Review Checklist for QA
You should be able to look at a developer's Pull Request (PR) and assess it against these criteria:

1.  **Functionality (Does it work?)**
    - Is the logic correct?
    - Are edge cases and boundary conditions handled properly?
    - Are `null` or `undefined` values handled gracefully?
    - Are errors handled appropriately with helpful messages and proper logging?

2.  **Testing (Is it tested?)**
    - Is the new code covered by tests?
    - Are edge cases, error paths, and boundary conditions included in the tests?
    - Are the tests clear, focused, independent, and repeatable?

3.  **Readability (Can I understand it?)**
    - Do variable, function, and class names reveal their intent?
    - Are functions small (ideally < 20 lines) and do they do only one thing?
    - Is the code logically organized with appropriate abstraction levels?

4.  **Maintainability (Can we change it?)**
    - Is there any duplicate code (DRY principle)?
    - Are there any "god" classes or functions that do too much?
    - Are there no "magic numbers" or unexplained constants?

5.  **Security**
    - Are inputs validated and sanitized to prevent injection attacks (SQLi, XSS)?
    - Are sensitive data (passwords, tokens) handled securely and never logged?
    - Are proper authentication and authorization checks in place?

#### How to Provide Feedback
Focus on the code, not the author. Be constructive and specific. Instead of saying "This is bad code," say "This function is doing too much; consider breaking it down into smaller functions for better readability and testability."

---

### 5. Automated Testing with Playwright and Similar Tools

Playwright is the modern standard for end-to-end (E2E) web testing. It's fast, reliable, and packed with features that prevent flaky tests. Your task is to write robust, maintainable, and efficient test automation.

#### Core Principles of Robust Playwright Tests
1.  **Use Web-First Assertions and Auto-Waiting:** Playwright automatically waits for elements to be ready before performing actions. Never use hard-coded `sleep()` waits. Use assertions like `await expect(page.locator('button')).toBeVisible();`.

2.  **Prioritize Stable Locators:** The key to a non-flaky test is a stable selector. Follow this priority order:
    - **`getByRole`:** Best for buttons, links, and other interactive elements (e.g., `page.getByRole('button', { name: 'Submit' })`).
    - **`getByLabel`:** Perfect for form fields (e.g., `page.getByLabel('Email')`).
    - **`getByText`:** Good for finding elements by their content.
    - **`data-testid`:** Use only when semantic selectors are not feasible (e.g., `page.locator('[data-testid="checkout-button"]')`).
    - **Avoid brittle CSS/XPath** like deeply nested divs or `nth-child` selectors.

3.  **Ensure Test Isolation:** Tests must be independent and can run in any order. Use Playwright's **Browser Contexts** to create isolated, incognito-like sessions for each test, preventing state from leaking between tests.

4.  **Handle Network and Dependencies:** Mock third-party APIs using Playwright's network interception to make your tests fast and reliable, and to avoid depending on external services.

5.  **Control Flakiness:**
    - **Configure Retries:** Playwright can automatically retry failed tests. Treat any test that passes on a retry as a flake that needs investigation.
    - **Capture Trace/Screenshot/Video:** Always capture a trace and screenshot on failure. Use the Playwright Trace Viewer to debug exactly what went wrong.

6.  **Parallelization and Sharding:** Ensure your suite is safe to run with multiple workers (no shared state). For large suites, use sharding to split tests across multiple machines in CI.

#### A QA Engineer's Playwright Checklist
| Area | Best Practice |
| :--- | :--- |
| **Scope** | Tests protect a **critical user journey** (e.g., checkout, login). Not "nice-to-have" features. |
| **Selectors** | Use `getByRole`, `getByLabel`, `getByText` as default. Use `data-testid` only as a last resort. |
| **Assertions** | Use web-first assertions with auto-waiting. No `sleep()` or time-based waits. |
| **Isolation** | Tests are independent, can run alone or in parallel. Data setup is explicit and cleanup is deterministic. |
| **Flakes** | Trace/screenshot/video captured on failure. Retries are configured for flaky tests. |
| **Parallelization** | The suite is safe to run with multiple workers. A sharding plan exists for large suites. |

---

**Final Preparation Tip:** The interview will likely involve hands-on tasks. Practice writing a Playwright test from scratch, identifying a security vulnerability in a sample API, and writing a performance test script. Review the OWASP API Security Top 10 list multiple times to internalize it. Good luck—you are well-prepared for this challenge!
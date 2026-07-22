# SQA Intern Interview — Cheat Sheet (Appifylab / EzyCourse)

Use this to *review*, not memorize word-for-word. Say these ideas in your own words — that's what will sound natural in the room.

---

## Tier 1 — Core Terminology
- **QA vs QC:** QA = process, proactive, prevents defects. QC = product, reactive, detects defects.
- **Verification vs Validation:** Verification = static, no execution ("right product built right"). Validation = dynamic, executes code ("is it the right product").
- **Error → Defect → Failure:** Human mistake → flawed code (dormant until run) → observed wrong behavior when triggered.
- **SDLC vs STLC:** STLC runs in parallel with SDLC, phase-for-phase (shift-left).
- **STLC 6 phases:** Requirement Analysis → Test Planning → Test Case Development → Environment Setup → Execution → Test Cycle Closure.
- **Bug life cycle:** New → Assigned → Open → Fixed → Retest → Closed (or Reopened if it fails retest).
- **Reopened vs Open:** Reopened = fix failed retest, not a fresh bug — always attach new evidence.
- **Deferred vs Duplicate:** Deferred = valid, fixed later. Duplicate = already logged, link to original.
- **Severity vs Priority:** Severity = technical impact (you decide). Priority = business urgency (PO decides). Know all 4 combos (e.g., typo during a big ad campaign = low severity, high priority).

## Tier 2 — Types & Levels of Testing
- **4 levels:** Unit (dev) → Integration → System → UAT.
- **Top-down** uses **Stubs** (dummy called functions); **Bottom-up** uses **Drivers** (dummy calling functions).
- **Functional** (what it does) vs **Non-functional** (how well: performance, load, stress, security, usability, compatibility).
- **Smoke vs Sanity:** Smoke = broad, shallow, "is the build even alive" (run first on a new build). Sanity = narrow, deep check after a specific fix.
- **Retest vs Regression:** Retest = re-run the *exact* failed case. Regression = check that the fix didn't break *other* things.
- **Positive vs Negative testing:** valid inputs vs invalid inputs (spend ~50% effort on negative — that's where bugs hide).
- **Black/White/Grey-box:** no code knowledge / full code knowledge / partial (you'll live ~95% in black-box + some grey-box via SQL checks).
- **Static vs Dynamic:** static = reviewing without running (code review); dynamic = executing.
- **Exploratory (structured, charter-driven) vs Ad-hoc (unstructured, random).**
- **Alpha (internal, pre-release) vs Beta (external users, real-world).**

## Tier 3 — Test Design & Documentation
- **Scenario → Case → Script:** one-line goal → detailed manual steps → automated code.
- **Boundary Value Analysis (BVA):** test the edges (17/18/19...59/60/61 for an 18–60 range) — most bugs are off-by-one errors.
- **Equivalence Partitioning:** one representative value per valid/invalid group, then apply BVA at the edges.
- **Decision Table:** use when behavior depends on *combinations* of conditions (AND/OR logic), not a single range.
- **State Transition testing:** for features with defined states (e.g., order: Placed → Shipped → Delivered).
- **Good bug report:** clear title, steps to reproduce, expected vs actual, environment/build, severity, screenshots/logs.
- **Test Plan components:** Scope, Strategy, Entry/Exit Criteria, Resources, Schedule, Risks.
- **Entry Criteria** = conditions to *start* testing (build deployed, smoke passed). **Exit Criteria** = conditions to *stop* (0 open critical bugs, pass rate >95%).
- **RTM (Requirement Traceability Matrix):** maps requirements → test cases, exposes coverage gaps.

## Tier 4 — Automation (Playwright/JS)
- **User-first locators** (`getByRole`, `getByText`, `getByTestId`) > CSS/XPath — resilient to DOM changes.
- **Auto-waiting:** Playwright retries actionability checks (attached, visible, stable, enabled) automatically — no `sleep()` needed.
- **Web-first assertions:** `await expect(...)` retries until the condition is met or times out (handles async UI updates).
- **`await` everywhere:** forgetting it causes race conditions — code moves on before the action/navigation finishes.
- **No StaleElementReferenceException in Playwright:** it re-queries the DOM at action time instead of storing a dead reference (a common Selenium pain point).
- **Page Object Model (POM):** one class per page holding locators + actions — change the UI once, update one file.
- **Data-Driven** (same steps, different data from CSV/JSON) vs **Keyword-Driven** (non-coders write steps as keywords) vs **Hybrid** (combination — most common in industry).

## Tier 5 — Web & API Basics
- **GET** = retrieve, idempotent, params in URL. **POST** = create, not idempotent, params in body. **PUT** = full update (idempotent). **PATCH** = partial update.
- **401** = not authenticated ("who are you?"). **403** = authenticated but not permitted ("I know you, but no").
- **404** = client's fault (bad URL/resource). **500** = server's fault (backend crash).
- **REST** (HTTP + JSON, lightweight, industry standard) vs **SOAP** (XML, rigid, used in banking).
- **Postman:** send request → check status code → write `pm.test()` assertions on response body/status.
- **DevTools:** Elements tab for locators/DOM inspection; Network tab to check API calls/responses when "nothing happens" on click.
- **SQL:** `SELECT * FROM Users WHERE email = 'x';` — know this cold. **INNER JOIN** = only matching rows in both tables. **LEFT JOIN** = all rows from left table, NULLs where no match.

## Tier 6 — Process (Agile, CI/CD, Git)
- Testing runs **parallel** to the sprint, not just at the end — start with the API/backend before the UI is even ready.
- **Product Owner** = the "what" (business value, backlog). **Scrum Master** = the "how" (removes blockers, process).
- **Standup 3 questions:** yesterday / today / blockers.
- **Acceptance Criteria** = your source of truth — each AC becomes at least one test case.
- **Definition of Done** = the whole team's checklist for "complete" (tested, reviewed, merged, etc.), broader than one story's AC.
- **CI/CD:** automated tests (Playwright) run automatically on push/deploy as a quality gate before merge/release.
- **Git:** why QA needs it — automation scripts live in a repo too (branch → commit → push → PR → merge like any other code).

## Tier 7 — Applied Scenarios (quick logic, not scripts)
- **Vague requirements →** don't assume, get clarifications in writing from the PO before writing test cases.
- **"Works like the competitor" →** explore competitor, but flag the risk of inheriting their bugs; get our own explicit AC.
- **Not enough time to test everything →** risk-based triage: Smoke → new features → high-business-impact regression. Communicate what's *not* being tested.
- **Deadline vs quality tension →** never silently ship or silently block. Present the risk with data, let the PO/manager decide, document the decision.
- **Standard "how would you test X" flows** (Login, Search, Video Upload, Checkout) — structure your answer as: functional happy path → negative/edge cases → non-functional (performance, security) → cross-device.

## Tier 8 — Appifylab / EzyCourse specifics
- **EzyCourse** = no-code SaaS LMS for creators to build/sell courses, communities, memberships. Related products: EzyCommunity, EzyStudio, EzyLive.
- **If asked "most critical feature to test":** payment & subscription system — real money, direct creator revenue, churn risk if broken.
- **RBAC (Admin/Instructor/Student):** test both that each role *can* do what it should, and *cannot* do what it shouldn't — check via UI **and** directly via API (don't trust hidden buttons).
- **Subscriptions:** renewal, failed payment/dunning, proration on upgrade/downgrade, cancellation (access until period end vs immediate).
- Mention specific things you noticed exploring the actual product (curriculum builder, role dashboards, live streaming) — genuine, specific observations beat generic praise.

---

## HR/Behavioral — key beats, not scripts
- **Tell me about yourself:** background → why QA (the "detective work" framing) → why Appifylab/EzyCourse specifically. 60–90 sec, practice out loud.
- **Why QA over dev:** QA = specialization, not a downgrade. Your dev background = you read code, talk to devs in their language, write better automation.
- **Strengths/weaknesses:** pick a real weakness with a concrete improvement action, not a fake humble-brag.
- **Bug isn't a bug (dev disagrees):** stay evidence-based — reference the requirement/AC, don't get emotional, escalate to PO only if truly unresolved.
- **Critical bug, manager wants to ship anyway:** document, present risk with evidence, let them decide, document the decision. Never silently comply or silently block.
- **Explaining bugs to non-technical people:** translate to business/user impact, not jargon.
- **Salary:** flexible, learning-focused, give a modest range if pushed — but verify current Sylhet intern norms yourself rather than trusting a fixed number.
- **Always have 2–3 questions ready for them** — e.g. manual/automation ratio, test data/staging setup, onboarding process.

---

*Full detail with analogies and worked examples is in your original prep.md — use this sheet for fast recall, and that one when you want to go deeper on a specific topic.*

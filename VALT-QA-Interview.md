## VALT QA Interview — Prioritized Question List

### Priority 1: Behavioral / Non-Technical (Topic 7) — prep ALL of these
1. Tell me about yourself.
Ans:Thank you for this opportunity. My name is Robiul Islam, from Sylhet, and I recently completed my Bachelor's in Computer Science and Engineering at Leading University.

From an early age I was drawn to computers and the internet, which led me into web development. Over time I built a solid, practical foundation in HTML, CSS, JavaScript, React, Node.js, Express, and MongoDB — working across several projects that taught me not just how to build features, but how they tend to break.

While working on one of my personal projects, I ended up doing a lot of the manual testing myself — going through each page and user flow carefully. I caught and helped fix things like buttons that looked fine but didn't actually trigger the right action, missing validation messages, and even a case where an admin-only page wasn't properly restricted for regular users. Going through that process, I realized I actually enjoyed that side of the work more than writing the feature itself — deliberately trying to break something to make it more reliable. That's what pulled me toward QA, and I've since been learning Playwright, Postman, and core testing methodologies to build on that instinct properly.

I'm especially excited about Appifylab because of EzyCourse a platform with real subscriptions, role-based access, and live content, which is exactly the kind of complexity I want to test rigorously. I see this internship as the right place to turn my development background into a career as a detail-oriented QA engineer.

**2. Walk me through your background — why QA after full-stack projects?**
> "I have a CSE degree and built LU Pulse, a full-stack multi-role web app with student, admin, and super admin access levels. While building it, the part I spent the most time on — and enjoyed most — was breaking it: testing that each role could only access what it should, checking API responses for every auth state, and catching a real security gap where roles were trusted from the JWT without a database check. That experience showed me I have both the technical background and the mindset for QA, and given how competitive junior dev hiring is right now, I decided to pursue that strength directly instead of waiting months for a dev role to open up."

**3. Why QA specifically?**
> "Because it's where my strengths already point. I like systematically finding what's broken, documenting it clearly, and verifying fixes — that's what I was doing informally as a developer testing my own app. QA lets me do that as my primary job instead of a side task, and long-term I want to specialize in it, particularly automation."

**4. Why not keep pursuing an SWE role?**
> "I'm not ruling out development long-term, but the SWE market right now is very competitive, especially for junior roles — it could realistically take another six months to land one. I don't want to sit idle waiting for the 'ideal' role when I already have a clear path into QA today, using skills I've already demonstrated. I'd rather start building a career now than wait."

**5. Developer or tester going forward?**
> "Tester, with a technical edge. My near-term and honestly my primary path forward is QA — and specifically I want to grow toward QA automation / SDET, which uses coding skills but is a testing career, not a development one. I'm not treating this as a stepping stone back into pure development."

**6. What do you know about VALT / Yo Tech?**
> "VALT is a UK fintech company — a cloud-based EPOS and restaurant management platform for hospitality businesses: POS, kitchen display systems, self-service kiosks, and integrations with delivery platforms like Just Eat, Deliveroo, and Uber Eats, plus built-in payments. Yo Tech is the Sylhet-based team that's actually built and run VALT's technology for about a decade. VALT only launched formal Bangladesh operations this past July, so Yo Tech isn't new to the product at all — it's the engineering team behind it, now also serving the local market."

**7. Why VALT/Yo Tech specifically?**
> "A few reasons: it's real payment/transaction software, so QA here actually carries weight — correctness and security matter, not just UI polish. It's also local, so I can build a serious QA career without relocating. And the JD specifically calls out API testing as a plus, which lines up with the API testing work I already did as a developer."

**8. Comfortable with 1 PM–10 PM, on-site?**
> "Yes, completely comfortable with both." — Say this plainly and immediately, no hedging. Any pause here reads as a flight-risk signal given their concern.

**9–10–11–12. The retention cluster (treat as one theme, answer each with a slightly different angle so you're not repeating yourself word-for-word):**

*9. How do we know you won't leave for a dev job?*
> "Fair question given my background. My honest answer: I'm not treating QA as a waiting room. My actual long-term goal is a senior QA automation or SDET role — that's a testing career I want to build, not a dev job I'm settling for. If I wanted to keep chasing dev roles, I'd keep doing that instead of starting a QA career at all. I'm here to build something, not park somewhere."

*10. Where in 2–3 years?*
> "Growing from manual testing into API and automation testing, ideally within this team — building toward a QA lead or SDET-level role where I'm also writing test scripts and contributing to test frameworks."

*11. Long-term career goals?*
> "To become a senior QA automation engineer / SDET — someone who bridges testing and development, designs test strategy, and builds automated test suites. That's a multi-year path, and I want to build it step by step starting with strong manual testing fundamentals, which is exactly what this role offers."

*12. What guarantee do you offer if we invest in training you?*
> "I can't give you a contract-level guarantee, no one honestly can — but I can tell you my incentive structure is aligned with staying: the QA/SDET path I want takes years to build properly, and I get more value out of building it here, with real product experience, than by restarting somewhere else. I'd also point to LU Pulse as evidence — I stuck with a demanding multi-role project through to completion rather than abandoning it partway."

**13. Handling switching work / shifting priorities?**
> "In LU Pulse I regularly moved between building features, debugging integration issues via DevTools, and manually testing permission boundaries in the same day. I stay organized by keeping a running list of what's in progress versus what's blocked, and I re-prioritize based on what's blocking others' work first."

**14. A challenge you faced and how you handled it?**
> "The JWT role-trust issue was the clearest one. The system worked fine on the surface — different roles saw different UIs — but I noticed the actual authorization was being read from the token itself rather than re-checked against the database. That's the kind of bug that looks fine until someone crafts or reuses a stale token. I traced it by testing edge cases across roles with Postman and DevTools, confirmed the gap, then fixed the backend to verify role from the database on each request."

**15. A bug others missed?**
> Same JWT story — reuse it, framed for "found something others missed": "During testing, the feature worked correctly for every normal user flow — which is likely why it wasn't caught. I found it specifically because I tested with modified/stale tokens instead of just normal login flows, which is a testing mindset, not just a 'does it work' check."

**16. Disagreement with a dev who says "it's not a bug"?**
> "I'd bring concrete evidence — exact steps to reproduce, expected behavior versus actual behavior, and reference the requirement or spec if there is one. If we still disagree after that, I'd loop in the QA lead or PM to make the call rather than argue it out unresolved. It's not personal — it's about aligning on what the requirement actually says."

**17. Prioritizing multiple test items under a tight deadline?**
> "I'd prioritize by risk and blast radius first — critical paths and anything blocking other people's work (like a broken API endpoint others depend on) before cosmetic issues. I'd also flag early to the team lead if something can't realistically be covered in time, rather than silently skipping it."

**18. Strengths and weaknesses?**
> Strengths: "Attention to detail from a developer's perspective — I notice things like incorrect status codes or permission leaks that someone without dev background might not think to check. I also document clearly, which matters a lot in bug reports."
> Weakness (pick something real and QA-relevant, with a fix in motion): "I'm still building formal QA process knowledge — like structured test case design and test management tools — since my testing so far has been project-driven rather than process-driven. I'm actively working on that, which is part of why this role appeals to me." (Don't say "I work too hard" — sounds fake.)

**19. Why hire you over someone with actual QA experience?**
> "I won't out-experience someone who's already done QA for years — but I bring something they might not: a developer's instinct for *why* something breaks, not just *that* it broke. I already understand REST APIs, auth flows, and permission systems from having built them, so I can write more targeted test cases faster than someone learning those systems for the first time. I'm also clearly motivated to build a long-term specialization here, not just fill a seat."

20. Do you have any questions for us?

### Priority 2: Light Technical (baseline conversational competence)

**Testing Fundamentals**
Let's get you ready for this. I'll cover the narrative questions first (these matter most since the interview is mostly non-technical), then go through the technical list.

## The "why" questions — these decide the interview

**Why QA and not dev?**
Keep it simple and honest, don't over-explain: "During my final year project, LU Pulse, I ended up doing a lot of manual testing myself — checking permission boundaries between Student/Admin/Super Admin roles, testing API responses across auth states, using DevTools to trace bugs. I actually found a real security bug where roles were trusted from the JWT without a database check. I enjoyed that investigative process more than I expected — the mindset of 'how can I break this' rather than just 'does this work.' That's when I started looking at QA as a real path, not a fallback."

**Why leave dev / are you just settling for QA because SWE is hard to get?**
This is the trap question. Don't lie and don't over-justify either — just be direct and confident:
"I won't pretend the SWE market isn't competitive right now — it's taking people 6+ months to land roles, and I want to start my career now rather than wait it out. But this isn't just a placeholder for me. My dev background is actually an asset in QA — I understand how APIs are built, how auth works, how databases are structured, which means I can test more intelligently than someone without that context. I want to grow into API testing, automation, and eventually SDET — where my dev skills and QA skills combine. So this is a deliberate path, not a detour."

This is honest, shows self-awareness, and reframes your dev background as a strength instead of a red flag. Say this confidently — don't apologize for it.

**Will you job-hop back to dev in 3 months?**
"No — realistically, building real QA experience, learning the tools properly, and becoming genuinely good at this takes time, not weeks. I'm looking at this as at least a 1.5–2 year commitment before I'd even think about SDET, and only internally if that path exists at VALT. I'm not interested in job-hopping — I've been an Organizing Secretary and held leadership roles in my university clubs, so working with a team long-term and being reliable is something I value."

**What do you know about VALT?**
Say this in your own words based on what's on their JD: they build POS (point-of-sale) machines and software for businesses — hardware + software combo, which means testing likely spans both web/software functionality and possibly hardware-integration edge cases (transaction speed, security, reliability under real store conditions). Mention it's based in Sylhet at YOTECH, full-time, and that transactions need to be fast and secure — which tells you correctness and reliability testing really matters here (a POS bug means someone can't complete a sale or a transaction is wrong — real money, real stakes). This shows you actually read the JD rather than giving a generic answer.

**Why YOTECH/VALT specifically?**
Tie it to something concrete: it's local (Sylhet), it's a real product company (not just an agency), and QA on a financial/transactional product (POS) means the testing is meaningful — bugs have real consequences, which is exactly the kind of ownership you want early in a QA career.

---

## Technical Q&A

**21. Verification vs Validation**
Verification asks "are we building the product right?" — checking that the software meets the specified requirements/design, usually through reviews, walkthroughs, static analysis (no code execution needed). Validation asks "are we building the right product?" — checking the actual software behaves as the user expects, done by executing the software (testing). Example: verification = reviewing a login page's design doc against requirements; validation = actually logging in and confirming it works as the user needs.

**22. Types of testing**
- **Functional testing** – does each feature work as specified (e.g., does login authenticate correctly)
- **Regression testing** – after a code change, confirm existing features still work (didn't break anything)
- **Smoke testing** – quick check that the build is stable enough to test further ("is it worth testing today")
- **Sanity testing** – narrow, focused check that a specific bug fix or feature works, without a full regression
- **Integration testing** – do different modules/components work together correctly (e.g., frontend correctly consumes backend API)
- **System testing** – testing the whole, fully integrated system end-to-end against requirements
- **UAT (User Acceptance Testing)** – client/end-user verifies the system meets their real-world needs before go-live

You can mention you've done informal versions of several of these on LU Pulse — e.g., you tested role-based access (functional), retested after fixing the JWT bug (regression/sanity), and tested how frontend and backend/API worked together (integration).

**23. Bug/Defect Life Cycle**
The stages a bug goes through from discovery to closure:
New → Assigned (to a developer) → Open/In Progress → Fixed → Retest (QA verifies the fix) → Verified/Closed (if fix works) or Reopened (if it doesn't). Some flows also include "Rejected" (not a real bug), "Duplicate," or "Deferred" (valid but not fixed now).

**24. Severity vs Priority**
- **Severity** = how badly the bug impacts the system's functionality (technical impact)
- **Priority** = how urgently it needs to be fixed (business impact/order of fixing)

They don't always match. Example: a typo in the company name on the homepage is **low severity** (doesn't break functionality) but can be **high priority** (looks unprofessional, needs fixing before launch). Conversely, a crash that only happens in a rarely-used admin feature might be **high severity** (crashes the app) but **low priority** (few users hit it, can wait).

**25. Writing a good bug report**
Should include: a clear, specific title; steps to reproduce (numbered, precise); expected result vs actual result; environment details (browser, OS, device, app version); severity/priority; screenshots or screen recordings/logs when possible; and whether it's reproducible consistently or intermittent. The goal is that any developer can read it and reproduce the bug without asking you follow-up questions. You can mention you did this naturally while documenting the JWT auth bug you found in LU Pulse.

**26. What is an API (in your own words)**
An API (Application Programming Interface) is a set of rules that lets one piece of software talk to another — for example, your frontend (React) sending a request to your backend (Express) to fetch or update data, without needing to know how the backend is implemented internally. It defines what requests you can make, what data to send, and what response you'll get back.

**27. HTTP methods**
- **GET** – retrieve data (e.g., fetch a list of notices)
- **POST** – create new data (e.g., submit a new notice)
- **PUT** – update/replace an existing resource entirely
- **PATCH** – partially update an existing resource
- **DELETE** – remove a resource

**28. HTTP status codes**
- **2xx (Success)** – request worked (200 OK, 201 Created)
- **4xx (Client Error)** – something wrong with the request itself (400 Bad Request, 401 Unauthorized, 403 Forbidden, 404 Not Found)
- **5xx (Server Error)** – something went wrong on the server side even though the request was valid (500 Internal Server Error)

You actually tested this directly on LU Pulse, so mention that confidently.

**29. 401 vs 403**
- **401 Unauthorized** – you're not authenticated at all (no valid login/token — the server doesn't know who you are)
- **403 Forbidden** – you are authenticated, but you don't have permission to access this specific resource (server knows who you are, but says "no")

Great example from your own project: a Student role hitting an Admin-only route while logged in would return 403, while an unauthenticated user hitting any protected route would get 401. This is a strong, genuine example — use it.

**30. Testing an API manually with Postman**
Set up a request (choose method, enter the endpoint URL), add headers (like Authorization token), add a request body if needed (JSON for POST/PUT/PATCH), send the request, then check: the status code, the response body/structure, response time, and whether the data returned matches what's expected. You'd also test edge cases — invalid tokens, missing fields, wrong data types — to see if the API handles errors gracefully rather than crashing.

**31. SDLC (in your own words)**
Software Development Life Cycle — the overall process of building software, from planning and requirement gathering, through design, development, testing, deployment, and maintenance. It's the big-picture framework that the whole project follows.

**32. Agile vs Waterfall**
- **Waterfall** – linear, sequential phases (requirements → design → development → testing → deployment), each phase finished before the next starts. Rigid, hard to change direction mid-project.
- **Agile** – iterative, works in short cycles (sprints), with continuous feedback and flexibility to adapt requirements as you go. Testing happens throughout, not just at the end.

**33. Scrum and its ceremonies**
Scrum is a framework for implementing Agile, organized into fixed-length sprints (usually 1-2 weeks). Key ceremonies:
- **Sprint Planning** – deciding what work will be done in the upcoming sprint
- **Daily Standup** – short daily sync on what was done, what's next, and blockers
- **Sprint Review** – demoing completed work to stakeholders at the end of the sprint
- **Retrospective** – team reflects on what went well/poorly and how to improve the process

**34. STLC (in your own words)**
Software Testing Life Cycle — the specific process QA follows within the larger SDLC: requirement analysis (from a testing perspective), test planning, test case design, test environment setup, test execution, and test closure/reporting.

**35. STLC vs SDLC**
SDLC covers the entire software lifecycle (planning through maintenance) across the whole team. STLC is the testing-specific subset that runs alongside/within SDLC — it starts once requirements are available and focuses purely on how the QA process is planned and executed, not on development itself.

**36. MongoDB (in your own words)**
MongoDB is a NoSQL, document-oriented database. Instead of storing data in rigid tables and rows like a relational database, it stores data as flexible JSON-like documents (BSON) grouped into collections. You used it directly in LU Pulse to store notices, events, and user data.

**37. SQL vs NoSQL**
- **SQL (relational)** – structured tables with fixed schemas, rows/columns, relationships enforced via foreign keys, strong consistency, uses SQL query language (e.g., MySQL, PostgreSQL). Good when data structure is stable and relationships matter a lot.
- **NoSQL** – flexible/dynamic schema, data stored as documents (MongoDB), key-value pairs, or graphs. Easier to scale horizontally, better for rapidly evolving or unstructured data.

**38. Why MongoDB fit LU Pulse**
Notices, events, and user data don't always have a completely uniform structure — different notice types might have different fields (e.g., an event notice might need a date/location, a general notice might not). MongoDB's flexible schema handles that variability without needing rigid table redesigns. It also made rapid development easier as a student project with changing requirements, and integrates naturally with Node/Express (JSON in, JSON out) — no complex ORM/JOIN logic needed for a relatively simple, document-shaped dataset like this.


---


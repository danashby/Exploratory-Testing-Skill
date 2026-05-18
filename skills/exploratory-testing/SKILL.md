---
name: Exploratory Testing Skill
description: Exploring your products to discover information, related to a range of testing heuristics and oracles, guided by your test charter inputs with notes on problems and discoveries being produced as an output.
---

# Exploratory Testing Heuristics, Mnemonics & Oracles
### A Reference for AI-Assisted Exploratory Testing

## How to Use This Document

This file is designed to be included in a prompt to Claude Code to enable structured, heuristic-driven exploratory testing. Use the **test charter format** to focus a session:

> **"Explore [target], with [resources/data/tools], to discover [information about risks]"**

**Examples:**
- *"Explore the user registration flow, with valid and boundary-edge email inputs, to discover data validation and error handling risks."*
- *"Explore the payment checkout, with multi-currency and network interruption scenarios, to discover reliability and state-management risks."*
- *"Explore the admin user permissions, with role-switching and direct URL access, to discover authorisation bypass risks."*

When executing a charter, apply the heuristics in this document as a menu of lenses, not a mandatory checklist. Select what is relevant. Combine them freely.

---

## Output

At the end of every exploratory testing session, produce a full MCOASTER report structured as follows:

1. **MCOASTER sections** — Mission, Coverage, Obstacles, Audience, Status, Techniques, Environment, Risks
2. **Bug findings** — For each issue found, document:
   - A sequential ID (BUG-01, BUG-02, etc.)
   - The relevant criterion or standard (e.g. WCAG 2.2 AA criterion, FEW HICCUPPS oracle)
   - Severity: CRITICAL / HIGH / MEDIUM / LOW
   - Screen or area affected
   - Description of the problem and the evidence observed
   - Suggested fix
3. **Summary table** — All bugs in a single table with ID, criterion, severity, screen, and status
4. **Recommendations** — Prioritised fix list grouped by priority band (Priority 1 / 2 / 3)

Do not wait to be asked for the report — produce it automatically when the session is complete.

---

## Part 1: Test Oracles — How Do You Know There's a Problem?

An oracle is any means by which you recognise that something is wrong. All oracles are heuristic — fallible and context-dependent. Use them to argue *why* something is a bug, not just that it is.

---

### FEW HICCUPPS
**By Michael Bolton & James Bach**

The most widely used oracle heuristic in exploratory testing. Each letter represents a consistency criterion. A product may have a problem if it is inconsistent with any of these:

| Letter | Oracle | Description |
|--------|--------|-------------|
| **F** | **Familiar Problems** | Watch for patterns that match bugs you've seen before. If it looks like a familiar failure mode, investigate. This is the *opposite* of consistency — you want the product to be *inconsistent* with known failure patterns. |
| **E** | **Explainability** | The system's behaviour should be explainable to yourself and others. If you cannot articulate *why* the system did what it did, that's a signal worth chasing. |
| **W** | **World** | The product should be consistent with things that exist and are true in the real world. Physical laws, geography, date logic, domain facts. |
| **H** | **History** | The current version should behave consistently with previous versions unless a change was intentional and communicated. Unexplained regressions are bugs. |
| **I** | **Image** | The product should be consistent with the organisation's desired brand, professional standards, and reputation. Broken UI, spelling errors, or embarrassing output are image problems. |
| **C** | **Claims** | The product should do what its documentation, marketing, help text, labels, tooltips, and UI copy say it does. Any claim made about behaviour is a testable oracle. |
| **C** | **Comparable Products** | Compare behaviour against competitors, prior art, or analogous systems. If they all do X and your product does Y, investigate why. |
| **U** | **User Expectations** | The product should meet the reasonable expectations of its intended users, even when not explicitly specified. |
| **P** | **Product** | The product should be internally consistent with itself. The same operation should produce the same result across contexts, screens, and workflows. |
| **P** | **Purpose** | The product should serve the goals it was designed to serve — not just technically comply with specs. |
| **S** | **Standards** | The product should comply with relevant industry standards, accessibility guidelines, API contracts, and coding conventions. |
| **S** | **Statutes** | The product should comply with applicable laws, regulations, and legal requirements (GDPR, accessibility law, financial regulation, etc.). |

**How to apply:** When you observe a behaviour, ask: "Is this inconsistent with History? With Claims? With User Expectations?" Each oracle gives you language to justify a bug report.

---

### HICCUPPS (Original)
**By James Bach (earlier version)**

The predecessor to FEW HICCUPPS. Covers History, Image, Comparable Products, Claims, User Expectations, Product, Purpose, Standards/Statutes. Use FEW HICCUPPS as the current canonical version.

---

## Part 2: Coverage & Strategy — What Should You Explore?

---

### SFDIPOT (San Francisco Depot)
**By James Bach**

A product coverage model. Use this to ensure your exploratory session doesn't accidentally ignore entire dimensions of the system. Each letter is a domain to explore:

| Letter | Area | What to Explore |
|--------|------|-----------------|
| **S** | **Structure** | The physical and logical components: files, databases, config files, APIs, code modules, UI elements, third-party libraries. What does the product *consist of*? |
| **F** | **Function** | What the product *does*: features, behaviours, operations, commands, transactions. Explore both the happy path and the edges of each function. |
| **D** | **Data** | Inputs the product accepts, outputs it produces, data it stores and retrieves. Focus on types, ranges, formats, encoding, validation, transformation, and corruption. |
| **I** | **Interfaces** | How the product connects to things outside itself: APIs, databases, file systems, external services, hardware, browsers, OS. Integration points are high-risk. |
| **P** | **Platform** | The environment the product runs in: OS, browser, hardware, network, screen resolution, locale, container, cloud infrastructure. Vary the platform. |
| **O** | **Operations** | How users *use* the product in real-world contexts: workflows, sequences, habits, interruptions, concurrent users, admin operations, scheduled jobs. |
| **T** | **Time** | Time-sensitive behaviours: timeouts, session expiry, scheduled tasks, leap years, daylight saving changes, date/time formatting, race conditions, concurrency, sequencing. |

**Extended version:** Sometimes written as **SFDPOT** (without the I for Interfaces). Karen Johnson added T for Time for mobile testing contexts.

---

### CRUSSPIC STMPL
**By James Bach (from the Heuristic Test Strategy Model)**

A quality attribute model for non-functional testing. Split into two parts:

**CRUSSPIC — Operational Quality Criteria:**

| Letter | Criterion | Description |
|--------|-----------|-------------|
| **C** | **Capability** | Does it do what it's supposed to do? All intended features present and functional. |
| **R** | **Reliability** | Does it keep working under expected conditions over time? Stable, consistent, no random failures. |
| **U** | **Usability** | Can users figure out how to use it without pain? Learnability, efficiency, error recovery. |
| **S** | **Security** | Is it protected against misuse, unauthorised access, data exposure, and attack vectors? |
| **S** | **Scalability** | Does it cope with increasing load, data volume, or number of users? |
| **P** | **Performance** | Does it respond in acceptable time under normal and peak conditions? |
| **I** | **Installability** | Can it be installed, configured, updated, and uninstalled cleanly across target environments? |
| **C** | **Compatibility** | Does it work correctly alongside other systems, browsers, versions, and platforms? |

**STMPL — Development Quality Criteria:**

| Letter | Criterion | Description |
|--------|-----------|-------------|
| **S** | **Supportability** | Can it be monitored, diagnosed, and supported in production? Logging, alerting, diagnostics. |
| **T** | **Testability** | Can it be tested? Are there hooks, observability, and controllability to enable verification? |
| **M** | **Maintainability** | Can it be changed without high risk? Code quality, modularity, documentation. |
| **P** | **Portability** | Can it be moved to different environments, platforms, or configurations? |
| **L** | **Localisability** | Can it be adapted for different languages, regions, currencies, date formats, and cultural conventions? |

---

### FIBLOTS — Model Workloads for Performance Testing
**By Scott Barber**

When choosing what to performance test, prioritise workloads that are:

| Letter | Criterion | Description |
|--------|-----------|-------------|
| **F** | **Frequent** | Transactions that happen most often will have the highest impact on perceived performance. |
| **I** | **Intensive** | Operations that consume the most resources (CPU, memory, I/O, network). |
| **B** | **Business Critical** | Paths that directly drive revenue, compliance, or mission-critical outcomes. |
| **L** | **Legal** | Performance requirements mandated by contract, SLA, or regulation. |
| **O** | **Obvious** | Things that users will notice immediately if slow: page loads, search, login. |
| **T** | **Technically Risky** | Complex operations, integrations with external services, known bottlenecks. |
| **S** | **Stakeholder Mandated** | Explicit performance goals set by product owners, customers, or leadership. |

---

### IVECTRAS — Performance Test Classification
**By Scott Barber**

A framework for naming and scoping performance tests:

> **Investigation or Validation of End-to-End or Component response Times and/or Resource consumption under Anticipated or Stressful conditions**

Use this to define what kind of performance test you're running: investigation (exploratory) vs validation (against a benchmark), end-to-end vs component, and anticipated load vs stress/spike.

---

## Part 3: Touring Heuristics — How Do You Move Through the Application?

Tours are structured approaches to exploring an application. Think of each tour as a different *lens* to apply during a session.

---

### FCC CUTS VIDS
**By Michael D. Kelly**

Eleven distinct touring strategies. Run one per session or combine as needed:

| Tour | What You Do |
|------|-------------|
| **Feature Tour** | Move through every visible feature and control. The goal is familiarity — build a map of what exists. |
| **Complexity Tour** | Find the five most complex things in the application. Complexity = risk. Explore deeply. |
| **Claims Tour** | Find every place the product makes a claim about what it does (labels, tooltips, help text, docs, marketing). Test each claim. |
| **Configuration Tour** | Find every way to change settings that the application persists. Toggle, combine, and reset settings. Look for unexpected interactions. |
| **User Tour** | Identify the key user roles or personas. Test the product as each user would use it, following their realistic goals. |
| **Testability Tour** | Explore the application's built-in mechanisms for testing: logging, debug modes, diagnostics, test data generation, feature flags. |
| **Scenario Tour** | Execute realistic end-to-end user scenarios — stories, not just features. Cover the full journey from entry to completion. |
| **Variability Tour** | Find things that can vary and vary them: input values, screen sizes, speeds, concurrency, data sets. Look for where variation breaks things. |
| **Interoperability Tour** | Test how the product interacts with other systems: APIs, plugins, file formats, external services, browsers, OS features. |
| **Data Tour** | Follow the data. Trace inputs to outputs, storage, and transmission. Look for where data is transformed, truncated, lost, or corrupted. |
| **Structure Tour** | Explore the underlying structure: file system, database schema, API endpoints, configuration files, logs. |

---

### Cem Kaner's Software Testing Attacks
**By Cem Kaner**

A set of aggressive, targeted testing strategies to probe specific risk areas. Key attacks include:

- **Long Name Attack**: Submit inputs far exceeding expected length limits (255+, 1000+, 10000+ chars).
- **Special Characters Attack**: Submit inputs containing characters with special meaning: `< > & " ' / \ ; ( ) { } [ ] | * ? % # @ !`
- **Boundary Value Attack**: Test exactly at, just below, and just above defined boundary values.
- **Invalid Type Attack**: Submit data of the wrong type where a specific type is expected (text in a number field, number in a date field).
- **Null/Empty Attack**: Submit null, empty string, whitespace-only, zero, or absent values for every input.
- **Overload Attack**: Submit the maximum number of items, records, or parallel operations.
- **Interruption Attack**: Interrupt operations mid-flight: close the browser, kill the network, pull power, switch tabs.
- **Sequence Attack**: Perform operations out of order, skip steps, repeat steps, or reverse a workflow.

---

## Part 4: Data & Input Heuristics — What Values Do You Test?

These heuristics from the **Test Heuristics Cheat Sheet** (Elisabeth Hendrickson, James Lyndsay, Dale Emery) guide what data and inputs to choose.

---

### Goldilocks
Too Big, Too Small, Just Right.

For any input or value with a range: test a value that is too large, too small, and exactly right. Also test just over and just under boundary values. Ask: what happens at the extremes?

---

### Zero, One, Many (ZOM)
**Sometimes written as "0, 1, Many"**

For any collection, count, or quantity: test with zero instances, exactly one instance, and many instances. Also test with the maximum number. Catches off-by-one errors, plural/singular handling, and count boundary failures.

*Examples: 0 items in a cart, 1 item, 100 items. 0 search results, 1 result, thousands of results.*

---

### CRUD
Create, Read, Update, Delete.

The four fundamental data operations. For any data entity in the system, test all four operations and verify data integrity at each step. Combine with other heuristics:

- *CRUD + Zero-One-Many*: Create 0, 1, many records; Delete a record that has 0, 1, many children.
- *CRUD + Position*: Update the first item, a middle item, the last item.
- *CRUD + Goldilocks*: Create with too-large, too-small, and valid values.

---

### Beginning, Middle, End (BME)
**Also called Position**

Vary where in a sequence an action occurs. Test operations at the start, middle, and end of a list, string, workflow, or data set. Truncation bugs, off-by-one errors, and ordering bugs often appear at boundaries.

*Examples: Insert text at the start/middle/end of a field. Delete the first, a middle, and the last item. Navigate to the first, a middle, and the last page.*

---

### Boundaries
Test the exact boundary value, one below, and one above. For numeric ranges, string lengths, date ranges, file sizes, and any defined limit. Boundary bugs are extremely common.

*Examples: A field that accepts 1–100 characters — test 0, 1, 50, 99, 100, 101. A date range that starts on 1 Jan — test 31 Dec, 1 Jan, 2 Jan.*

---

### Selection: Some, None, All
For any multi-select, permission set, checkbox group, or collection of options: test with none selected, some selected, and all selected. Also test "select all then deselect one" and vice versa.

---

### Sequences
Test operations in different orders. What happens if you do Step 3 before Step 1? What if you do an operation twice? What if you go back and redo a previous step? State machines and workflow engines are especially vulnerable.

---

### Count
Test the count of elements: zero, one, many, the maximum, and one over the maximum. Watch for pluralisation bugs ("1 records found"), pagination edge cases, and overflow.

---

### CRUD + Dependencies
Identify "has-a" relationships (Customer has Invoices; Invoice has Line Items). Apply CRUD and count heuristics across parent-child relationships. Delete a parent with no children, one child, and many children. Create a child before creating the parent.

---

### Multi-User / Concurrency
Test the same operation performed simultaneously by multiple users or processes. Look for race conditions, data corruption, deadlocks, and stale data. Especially relevant for shared resources, counters, and state.

---

### Interruptions
Interrupt operations in progress: lose network connectivity, close the browser tab, log out mid-transaction, kill the process, run out of disk space. What is the state of the system after recovery?

---

### Starvation
Deprive the system of resources: low memory, low disk, high CPU load, saturated network, unavailable external service. Does the system degrade gracefully? Do error messages make sense?

---

### Flood
Send a large volume of requests, data, or events in a short time. Does the system throttle, queue, drop, or crash? Look for memory leaks, buffer overflows, and cascading failures.

---

### Sorting
Test how the system sorts data: alphabetically, numerically, by date, by relevance. Test with mixed case, accented characters, numbers as strings, nulls, and duplicates. Try reversing the sort order.

---

### Input Method Variations
Change *how* you input data, not just *what* you input: paste vs type, keyboard vs mouse, mobile keyboard vs desktop, copy from a different encoding, use autocomplete, drag-and-drop, scan/upload.

---

### Configurations
Test different combinations of settings, preferences, feature flags, and environment variables. Some bugs only appear in specific configurations. Especially important for multi-tenant or heavily configurable products.

---

### State Analysis
Model the states the system can be in and the transitions between them. Test each state transition, including invalid transitions (trying to go from State A directly to State C, skipping B). Test the same operation in different states.

---

### Map Making
Draw or model the system before testing it. Create a state diagram, a flow chart, a mind map, or a data model. Translate between representations (state diagram to table, outline to mind map). The act of modelling surfaces assumptions and gaps.

---

## Part 5: Data Type Attack Cheat Sheet

Specific input values to probe for common vulnerabilities. Apply these to any input field.

### String / Text Inputs
- Empty string
- Single space / multiple spaces / only whitespace
- Very long string (255 chars, 1000 chars, 65535 chars)
- SQL injection: `' OR '1'='1`, `'; DROP TABLE users;--`
- XSS: `<script>alert(1)</script>`, `<img src=x onerror=alert(1)>`
- Special characters: `! @ # $ % ^ & * ( ) - _ + = [ ] { } | \ : ; ' " < > , . ? /`
- Unicode and emoji: `™`, `©`, `€`, `🎉`, `中文`, `العربية`
- Null character: `\0`
- Newline / carriage return: `\n`, `\r\n`
- Format strings: `%s %d %n %x`
- Path traversal: `../../etc/passwd`, `..\windows\system32`
- HTML entities: `&lt;`, `&amp;`, `&nbsp;`
- Extremely long word with no spaces (tests wrapping/truncation)

### Numeric Inputs
- Zero
- Negative numbers
- Very large number (beyond int32, int64)
- Very small decimal (0.0001, 0.000001)
- Maximum integer: `2147483647`, `2147483648`, `-2147483648`
- Floating point precision edge: `0.1 + 0.2`
- Scientific notation: `1e10`, `1e-10`
- Infinity, NaN
- Number as a string: `"123"`

### Date / Time Inputs
- Invalid dates: Feb 30, Sept 31, Feb 29 in a non-leap year
- Leap day: Feb 29 in a leap year
- Year boundaries: Jan 1 / Dec 31
- Millennium / century boundaries: Y2K, Y2K38 (Unix epoch)
- Daylight saving transition times
- Time zone edge cases: UTC±14, half-hour offsets
- Date in the past vs future vs today
- Different formats: `June 5 2025`, `06/05/2025`, `2025-06-05`, `06-05-25`
- Clock reset: moving system clock backwards or forwards
- Time difference between client and server

### File / Path Inputs
- Non-existent file path
- Already exists (overwrite scenario)
- No disk space available
- Write-protected or read-only
- File locked by another process
- File on a remote or unmapped drive
- Corrupted file
- Empty file (0 bytes)
- Maximum file size
- Wrong file type / extension mismatch
- File with special characters in the name

---

## Part 6: Regression Testing Heuristics

---

### RCRCRC
**By Karen Johnson**

Use this to identify *what* to regression test when a change is made:

| Letter | Focus | What to Ask |
|--------|-------|-------------|
| **R** | **Recent** | What new features or code were added? New code is more likely to introduce bugs. |
| **C** | **Core** | What essential functionality must always work? What would be catastrophic to break? |
| **R** | **Risky** | What areas of the codebase or product are inherently risky or complex? |
| **C** | **Configuration-Sensitive** | What code depends on environment settings, feature flags, or infrastructure config? |
| **R** | **Repaired** | What bugs were fixed in this release? Bug fixes can introduce new failures nearby. |
| **C** | **Chronic** | What areas perpetually break or have a history of instability? These need watching every time. |

---

## Part 7: Error Handling Heuristics

---

### FAILURE
**By Ben Simo**

Use this to evaluate the quality of error handling whenever an error state is encountered:

| Letter | Check | Description |
|--------|-------|-------------|
| **F** | **Functional** | Does the system continue to function correctly after the error? Or is it stuck? |
| **A** | **Appropriate** | Is the error message appropriate for the audience? Developers, end users, and admins need different messages. |
| **I** | **Impact** | What is the actual impact of the error on the user, the system, and the data? Has anything been corrupted or lost? |
| **L** | **Log** | Is the error logged with enough detail to diagnose it? Is sensitive information excluded from logs? |
| **U** | **UI** | Does the UI communicate the error clearly? Is it visible, readable, and non-misleading? |
| **R** | **Recovery** | Can the user or system recover from the error? Is recovery automatic, guided, or manual? |
| **E** | **Emotions** | How does the error make the user feel? Confused? Alarmed? Reassured? Does the tone fit the context? |

---

## Part 8: Bug Advocacy & Reporting Heuristics

---

### RIMGEA
**By Cem Kaner — Bug Advocacy Mnemonic**

Use this when writing a bug report or arguing that a defect should be fixed:

| Letter | Element | Description |
|--------|---------|-------------|
| **R** | **Replicate** | Can the bug be reliably reproduced? Document the exact steps. |
| **I** | **Isolate** | Have you isolated the minimum conditions needed to trigger the bug? |
| **M** | **Maximise** | What is the worst-case impact? Maximise the apparent severity when relevant. |
| **G** | **Generalise** | Is this a specific bug, or a pattern? Does it affect a class of inputs or situations? |
| **E** | **Externally Describe** | Describe the bug from the user's or business perspective, not just technically. |
| **A** | **And Report** | Document and report the bug with clear, reproducible steps and a compelling case for fixing it. |

---

### MCOASTER
**By Michael D. Kelly — Test Reporting Heuristic**

Use this to structure exploratory testing session notes and reports:

| Letter | Component | Description |
|--------|-----------|-------------|
| **M** | **Mission** | What was the goal of this testing session? What charter did you use? |
| **C** | **Coverage** | What did you actually cover? What areas, features, or risks did you explore? |
| **O** | **Obstacles** | What got in the way of your testing? Broken environments, missing test data, unclear requirements? |
| **A** | **Audience** | Who is this report for? Tailor the language and detail to their needs. |
| **S** | **Status** | What is the overall status? Is the tested area passing, failing, or unknown? |
| **T** | **Techniques** | What testing techniques and heuristics did you apply? |
| **E** | **Environment** | What environment did you test in? OS, browser, device, version, config. |
| **R** | **Risks** | What risks did you find or confirm? What risks remain uninvestigated? |

---

## Part 9: Project & Session Context Heuristics

---

### MIDTESTD (Kid Tested)
**By James Bach — Project Environment Heuristic**

Factors to consider when assessing the environment you're testing in:

| Letter | Factor | Description |
|--------|--------|-------------|
| **M** | **Mission / Customer** | Who is the customer? What do they value? What is the testing mission? |
| **I** | **Information** | What do you know about the product? What documentation, specs, or context exists? |
| **D** | **Developer Relations** | How does the testing team relate to development? Collaborative, adversarial, distant? |
| **T** | **Team** | Who is on the team? What are their skills, bandwidth, and motivations? |
| **E** | **Equipment & Tools** | What tools, environments, and infrastructure are available for testing? |
| **S** | **Schedule** | How much time is available? What are the deadlines and milestones? |
| **T** | **Test Items** | What exactly is being tested? What versions, builds, or components? |
| **D** | **Deliverables** | What outputs are expected? Reports, test cases, sign-off, metrics? |

---

### W5HE — Requirements Analysis
**By Darren McMillan**

Use this when analysing requirements or stories before testing:

| Element | Question |
|---------|----------|
| **Who** | Who are the users? Who is affected by this behaviour? |
| **What** | What is the feature supposed to do? What are the acceptance criteria? |
| **Where** | Where does this feature appear? What screens, APIs, or data stores are involved? |
| **When** | When does this behaviour occur? What triggers it? What are the pre-conditions? |
| **Why** | Why does this feature exist? What user need or business goal does it serve? |
| **How** | How does it work technically? What is the implementation approach? |
| **Exceptions** | What are the edge cases, error paths, and exceptions to the happy path? |

---

### I SLICED UP FUN — Mobile Application Testing
**By Jonathan Kohl**

Use this to guide exploratory testing of mobile apps:

| Letter | Area | Description |
|--------|------|-------------|
| **I** | **Inputs** | All the ways to interact with the device: touch, gestures, keyboard, voice, stylus, hardware buttons. |
| **S** | **Store** | App store submission requirements: ratings, permissions, screenshots, privacy policy, compliance. |
| **L** | **Location** | Geolocation: GPS accuracy, movement, stopping, entering/leaving zones, location denied. |
| **I** | **Interactions / Interruptions** | Incoming calls, notifications, other apps, background/foreground switching, low battery alerts. |
| **C** | **Communication** | SMS, phone calls, push notifications, email, Bluetooth, NFC, Wi-Fi, cellular data. |
| **E** | **Ergonomics** | Physical usability: portrait/landscape, one-handed use, screen size, contrast, font scaling. |
| **D** | **Data** | Data storage, offline mode, sync, data privacy, cache clearing, backup/restore. |
| **U** | **Usability** | Navigation patterns, discoverability, onboarding, accessibility, cultural conventions. |
| **P** | **Platform** | iOS vs Android specifics, OS version differences, device manufacturer variations. |
| **F** | **Function** | Core feature functionality: what the app is supposed to do. |
| **U** | **User Scenarios** | Real-world usage flows: typical journeys, power users, first-time users. |
| **N** | **Network** | Behaviour on different network types (Wi-Fi, 4G, 3G, offline), network switching, high latency, packet loss. |

---

## Part 10: Test Techniques Reference

---

### DUFFSSCRA — Test Technique Coverage
**By James Bach**

A set of test technique categories to ensure variety in your approach:

| Letter | Technique | Description |
|--------|-----------|-------------|
| **D** | **Domain** | Partition inputs and outputs into meaningful domains. Test values within each domain and at boundaries. |
| **U** | **User** | Test from the user's perspective: personas, goals, workflows, mental models. |
| **F** | **Function** | Test each function and feature systematically: inputs, outputs, state changes, side effects. |
| **F** | **Flow** | Test sequences, workflows, and end-to-end journeys. Test the path between features. |
| **S** | **Stress** | Push the system beyond normal operating conditions: high load, large data, sustained pressure. |
| **S** | **Scenario** | Test realistic usage scenarios that combine multiple features and user goals. |
| **C** | **Claims** | Test every testable claim made about the product. |
| **R** | **Risk** | Prioritise testing based on where the highest risk of failure exists. |
| **A** | **Automatic** | Consider what automated checks can be applied to accelerate coverage or catch regressions. |

---

### SLIME — Ordering Test Tasks
**By Adam Goucher**

A prioritisation heuristic for where to focus testing first:

| Letter | Focus | Rationale |
|--------|-------|-----------|
| **S** | **Security** | Security bugs have the highest potential for harm and embarrassment. Test it first. |
| **L** | **Languages** | Localisation and internationalisation bugs are often found late and are expensive to fix. |
| **I** | **Requirements** | Verify requirements are met before exploring edge cases. |
| **M** | **Measurement** | Test analytics, metrics, logging, and monitoring — these are often forgotten. |
| **E** | **Existing** | Regression: ensure existing functionality still works before testing new functionality. |

---

## Part 11: Wisdom Heuristics — Rules of Thumb

These are informal but powerful mental models experienced testers use.

---

### "Bugs Cluster"
Defects are not randomly distributed. They cluster around areas of complexity, recent change, and poor design. When you find one bug, look harder in the same area — more are usually nearby. *"Find one bug, find ten more."*

---

### "It's All About the Variables"
Every test is defined by the variables it controls and varies. To find more bugs, find more variables and vary them more deliberately. What has never been varied? What combination has never been tried?

---

### "The Narrower the View, the Wider the Ignorance"
Focusing on only one dimension of a system creates blind spots. Rotate your lens regularly: look at the same feature from a user perspective, a data perspective, a platform perspective, and a security perspective.

---

### "Vary Sequences, Configurations, and Data"
The probability of finding a bug increases with the diversity of what you try. Don't repeat the same test in slightly different ways — introduce genuine variety in the sequence of actions, the configuration of the environment, and the values of data.

---

### "Big Bugs Are Often Found by Coincidence"
Not all bugs are found by methodical analysis. Serendipitous discovery during unplanned exploration catches things structured testing misses. Leave room for following hunches.

---

### "Fresh Eyes Find Failure"
A new tester with no context will find bugs that the experienced team has normalised. The curse of knowledge causes testers to stop noticing things they've seen hundreds of times. Bring in new perspectives deliberately.

---

### "Never and Always"
Every system has behaviours that should *never* happen and behaviours that should *always* happen. Identify them explicitly. Then try to violate the "never" and break the "always." These are the most important invariants to test.

---

### "The Goldilocks Problem"
Not just for data: applies to any dimension of a test. The system should handle not too much, not too little, but just the right amount. Applied to: input length, file size, number of users, time allowed, permissions granted.

---

### "Observe the System Under Test as a Whole"
Bugs in complex systems often manifest at the intersection of components, not within them. Watch for unexpected effects on other parts of the system when testing one area: performance impact, shared state, log pollution, side effects.

---

### "Stopping Heuristics"
Know when to stop a line of investigation. Stop when:
- You've run out of new ideas.
- The risk of continuing is lower than the cost.
- You've reached the time budget.
- You've confirmed or disconfirmed the hypothesis you started with.
- You've found enough information to make a decision.

---

## Part 12: Session-Based Test Management (SBTM)

**By James Bach & Jon Bach**

A lightweight structure for making exploratory testing accountable and reportable.

### Charter Format
> **"Explore [target], with [resources], to discover [information]"**

Keep charters focused enough to complete in 60–120 minutes. Longer charters lose focus.

### Session Notes
Use MCOASTER (see above) to structure your notes. Record:
- What you did
- What you found (bugs, questions, observations)
- What you didn't have time to cover
- What you'd explore next

### Metrics
Track: charter completion rate, bug discovery rate, session coverage, and debrief findings. These make exploratory testing visible and defensible.

---

## Part 13: Rapid Application-Level Heuristics

Quick questions to ask about any area of the system:

### Security
- Can a user access data or functionality they shouldn't?
- What happens with direct URL manipulation?
- What happens with crafted API requests (bypassing UI validation)?
- Are session tokens properly invalidated on logout?
- Is sensitive data exposed in URLs, logs, error messages, or browser history?
- Are file uploads validated on the server side?

### Performance
- How does response time change as data volume increases?
- What happens at 10x, 100x the expected load?
- Are there memory leaks visible over time?
- Does the UI remain responsive while background operations run?

### Accessibility
- Is the app usable with keyboard only (no mouse)?
- Does it work with a screen reader?
- Are colour contrast ratios sufficient?
- Do form fields have associated labels?
- Are error messages announced to assistive technology?

### Internationalisation
- Does the UI break with right-to-left languages?
- Do date, time, number, and currency formats respect locale?
- Is translated text correctly accommodated in UI layouts (text expansion)?
- Are non-ASCII characters stored and retrieved correctly?

### Concurrency & State
- What happens when two users perform the same operation simultaneously?
- What happens when the same user opens two tabs?
- Is state correctly isolated between sessions?
- Does the system handle stale reads gracefully?

---

## Quick Reference: Mnemonic Index

| Mnemonic | Purpose | Author |
|----------|---------|--------|
| FEW HICCUPPS | Test oracles — recognising problems | Michael Bolton & James Bach |
| SFDIPOT | Product coverage — what to explore | James Bach |
| CRUSSPIC STMPL | Quality attributes — non-functional testing | James Bach |
| FCC CUTS VIDS | Application touring — how to move through the app | Michael D. Kelly |
| RCRCRC | Regression testing priorities | Karen Johnson |
| FAILURE | Error handling evaluation | Ben Simo |
| RIMGEA | Bug advocacy and reporting | Cem Kaner |
| MCOASTER | Session notes and test reporting | Michael D. Kelly |
| FIBLOTS | Performance test workload selection | Scott Barber |
| IVECTRAS | Performance test classification | Scott Barber |
| I SLICED UP FUN | Mobile application testing | Jonathan Kohl |
| DUFFSSCRA | Test technique coverage | James Bach |
| SLIME | Test task prioritisation | Adam Goucher |
| MIDTESTD | Project environment assessment | James Bach |
| W5HE | Requirements analysis | Darren McMillan |
| CRUD | Data operation coverage | General |
| ZOM (0,1,Many) | Count-based boundary testing | General |
| Goldilocks | Size/range boundary testing | General |
| BME | Position-based testing | General |

---

*Sources: James Bach (satisfice.com), Michael Bolton (developsense.com), Elisabeth Hendrickson (qualitytree.com / Explore It!), Michael D. Kelly (michaeldkelly.com), Karen Johnson (karennicolejohnson.com), Cem Kaner, Ben Simo, Scott Barber, Jonathan Kohl, Adam Goucher, Ministry of Testing.*

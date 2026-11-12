# m05-5: Conditionals, Expressions and Functions

**Builds on:** `06-m05-loops-meta-arguments` · **Feeds into:** `08-m06-modules-and-dr`

## Objective

By the end of this stage, the trainee has made `main/` compute common and environment-specific tags with locals and `merge`, conditionally create an optional archive bucket, and expose a map of the repeated support buckets through a for-expression output.

## Scope

| # | Subject | Time | Key concepts and terms | Research | Lab chunk |
| --- | --- | --- | --- | --- | --- |
| 5.5.1 | Locals and tag composition | 2.0 h | `locals`, `merge`, `length`, `element`, provider default tags versus resource tags, `terraform console` | When is a local value preferable to repeating a long expression, and how does `merge` handle duplicate map keys? | Steps 1–4: Locals and tag composition |
| 5.5.2 | Conditionals, for expressions and lookup | 3.0 h | conditional expression, `count` toggle, for expression, `lookup` with default, `length` check | What problem does a conditional expression solve, what does a for expression return, and why should `lookup` use a default for optional keys? | Steps 5–10: Conditional resource and output transformation |
| | **Total** | **5.0 h** | | | |

## Stack constraints

| Constraint | Detail |
| --- | --- |
| Runtime | Same as m04. |
| Environment | Continue in `main/` with the remote state and the support resources from m05. |
| Readability | Expressions make intent clearer. A nested expression that takes several minutes to decode is replaced by a local value or split into simpler expressions. |
| Lab conventions | Same as m02 (authentication, region, default tags, operating checklist, plan before apply, state handling, cost control, public access). |
| Cleanup | The optional resource is returned to the instructor-approved setting and destroyed if asked. The remote backend and the persistent resources needed for m06 are kept. |

## Lab

This stage has one lab, built up in chunks. The Lab chunk column in the Scope table shows which steps practise each subject. The Acceptance row is what the trainer checks.

### Lab m05-5: Locals, conditional resource and output transformation

| Step | What the trainee does |
| --- | --- |
| 1 | Creates a `locals` block for common tags: Project, Environment, ManagedBy and Owner. |
| 2 | Creates another local map for environment-specific tags or settings and uses `merge` to combine maps where needed. |
| 3 | References the resulting local map from resources that support tags and confirms provider default tags and resource tags do not conflict unexpectedly. |
| 4 | Runs `terraform console`, experiments with `length`, `merge`, `lookup` and `element` on small sample values, then exits. |
| 5 | Adds an `enable_archive` input variable with a boolean type and a default of false. |
| 6 | Uses a conditional expression with `count` to create the optional archive bucket only when `enable_archive` is true. |
| 7 | Uses a for expression to create an output map listing each support bucket key and its bucket name. |
| 8 | Uses `lookup` to retrieve an environment setting with a default value, and `length` to check whether a supplied collection is empty before relying on it. |
| 9 | Runs `plan` with `enable_archive` false, then true, and compares the plans, applying only what the instructor requires. |
| 10 | Restores the approved final variable value and verifies `terraform plan` shows the expected result. |
| **Acceptance** | Resources that support tags receive the merged local tag map. Provider default tags and resource tags do not conflict in the plan. `terraform console` results for `length`, `merge`, `lookup` and `element` on sample values are shown. With `enable_archive` false the plan creates no archive bucket, and with it true the plan creates one. The `enable_archive` default is false. The output map lists each support bucket key with its bucket name. The `lookup` call has a default and the emptiness check uses `length`. After the approved final value is restored, the plan shows the expected result. |

## Deliverable

The deliverable contains:

- [ ] Updated HCL in `main/`
- [ ] Before/after plan showing the conditional resource toggling
- [ ] Output map, in the README or as terminal evidence
- [ ] `write-up.md`, filled in from `write-up-template.md`

## Still open / ask the trainer

| Question | Why it matters |
| --- | --- |
| Which `enable_archive` value is the approved final setting, and does the instructor want the archive bucket applied? | Lab chunk 5.5.2 and cleanup |
| Which environment-specific tags or settings does the second local map carry? | Lab chunks 5.5.1 and 5.5.2 |
| How deep should the written explanations be? | Avoids over- or under-investing in the write-up |

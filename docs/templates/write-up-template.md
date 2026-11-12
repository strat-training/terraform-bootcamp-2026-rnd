<!--
INSTRUCTOR SETUP: Copy this file to `modules/<stage-folder>/write-up-template.md`.
Replace the bracketed placeholders below with the specific details from the stage's brief.
Delete this comment block before publishing.

VARIABLES TO REPLACE:
- [STAGE_ID] and [STAGE_TITLE]
- [DELIVERABLE_DESCRIPTION] (One line naming the deliverable and its companion pieces)
- [DESIGN_QUESTION_1/2/3] (Up to 3 specific prompts tied to Objective claims)
- [GUIDE_SCOPE] (One line saying what the guide takes a learner from and to)
- [SCOPE_TOPIC_1/2/3] (3+ candidates for concepts worth explaining)
- [DOD_ID_1/2/3] and [DOD_EVIDENCE_REQ_1/2/3] (Specific output requests hardest to fake)

DoD ID CONVENTION (the same in every stage and every course):
- A stage with labs uses `DoD-<lab>.<n>`, for example `DoD-1.4.1`. It is the n-th clause of that lab's
  Acceptance row in the brief. IDs are derived from the brief, never typed into it.
- A stage without labs uses a flat list in the brief's Definition of done: `DoD-01`, `DoD-02`, and so on.
- The verify task `V-<lab>.<n>` in `tasks.md` checks `DoD-<lab>.<n>`. The same number runs through the
  brief, the tasks, and this write-up.
- [DOD_ID_n] below takes either form. The Self-Assessment in section 5 lists every DoD ID in the stage.

RULES FOR FILLING THEM (generate from the stage's brief, and keep it short):
- Key decisions: up to three of the brief's research prompts, each cut down to one or two short
  questions. Pick the prompts that tie to a claim in the brief's Objective (something the trainee
  is meant to be able to explain), and check that every Objective claim has a question or a
  "Concepts Applied" candidate. Merge only prompts that are close; drop the rest. Do not add a
  "where did you spend the most time" question: the Friction Log already asks it.
  Example of a good fill-in: "Where did the trainee draw the line between a function, a class, and a
  module, and why?"
- Guide scope: one line saying what the guide takes a learner from and to.
  Example: "from an empty folder to a package that runs from the container terminal".
- Concepts: 3 or more candidates, inline, taken from the scope topic titles. Use only scope topics
  that no key-decision question already covers, so the two sections never ask about the same thing.
- Evidence: up to three items, each one piece of real output (or a note, where the brief asks for
  one) that shows a central claim of the brief's Objective was actually done, naming the DoD id it
  backs. Pick the outputs that are hardest to fake; do not restate every definition-of-done bullet.
  Example of a good request: "the error message, the cause, and the fix for a deliberately created
  circular import".
Everything else is the same in every stage.
-->

# [STAGE_ID] Write-Up: [STAGE_TITLE]

> **Instructions for the trainee:**
> Copy this file to `write-up.md` in this folder and fill it in as the work goes, not at the end. A write-up finished after the build already works tends to be thin, and it is both the checkpoint evidence and the draft teaching content for the next cohort. Write it so a trainee starting this stage next cohort could follow the same path. Delete all blockquotes (like this one) before submitting. **Never paste credentials, tokens, or key material.**

## 1. Deliverable & Evidence
> Link the code and provide the exact output proving it works, based on the Definition of Done (DoD) IDs from the brief. Paste real output, trimmed to the relevant lines and redacted, or link a screenshot.

**Repository:** [Link here]
**Overview:** [One or two sentences, in the trainee's own words, on what was built in this stage]
**Deliverable:** [DELIVERABLE_DESCRIPTION]

*   **DoD-[DOD_ID_1]:** [DOD_EVIDENCE_REQ_1]
    *   *Output/Evidence:*
*   **DoD-[DOD_ID_2]:** [DOD_EVIDENCE_REQ_2]
    *   *Output/Evidence:*
*   **DoD-[DOD_ID_3]:** [DOD_EVIDENCE_REQ_3]
    *   *Output/Evidence:*

## 2. How to build it (teach it to the next trainee)
> Write a guide covering the scope below, in the order the work was tackled and with the reason for that order. Use the trainee's own output and logs to show reasoning, not just a list of commands. Assume the reader is a peer who has not started this stage yet.

**Guide scope:** [GUIDE_SCOPE]

## 3. Decisions & Core Concepts
> This section covers the *why* behind the build and the theory applied.

**Key Decisions**
> Design choices and trade-offs, including any alternative that was considered and rejected.

*   **[DESIGN_QUESTION_1]:**
*   **[DESIGN_QUESTION_2]:**
*   **[DESIGN_QUESTION_3]:**

**Concepts Applied**
> Pick 2 to 3 of the ideas below. Explain each in the trainee's own words, based on how it was actually used in this stage, as if teaching someone who has never seen it.

**Ideas this stage covers:** [SCOPE_TOPIC_1], [SCOPE_TOPIC_2], or [SCOPE_TOPIC_3].

## 4. AI Collaboration Log
> Be specific, and answer each prompt below in as many lines as it needs. Only if no AI was used at all, say so in one line and skip the rest.

*   **Tools & Scope:** Which AI tool(s) were used, and for roughly what portion of the work?
*   **Prompt Examples:** Give 2 to 3 concrete examples of what the trainee asked for.
*   **Course Correction:** Describe at least one moment the tool's first suggestion was wrong, inefficient, or did not fit the design. What was wrong, how was it noticed, and what was done instead?
*   **Human Intervention:** What was accepted largely as-is, and what did the trainee rewrite or redesign personally?

## 5. Retrospective & Self-Assessment
> Reflect on the friction faced, how the work grades against the DoD, and what would change.

*   **Friction Log (What tripped the trainee up):** Where was the most time spent, and why? What did not behave as expected the first time? Keep the exact error texts, the cause, and how each was resolved, including dead ends and how the trainee got unstuck.
*   **What I'd do differently:** If the trainee started this stage over today, what is the single biggest change to the approach?
*   **DoD Self-Assessment:** Rate the work honestly against every DoD ID in the stage (Met, Partly Met, Not Met) and against the stage rubric.
*   **Blind Spots:** Name any part of the work the trainee would struggle to explain cold right now, without re-reading it first.

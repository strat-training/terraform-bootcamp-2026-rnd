# m02 Write-Up: First AWS Resource

> **Instructions for the trainee:**
> Copy this file to `write-up.md` in this folder and fill it in as the work goes, not at the end. A write-up finished after the build already works tends to be thin, and it is both the checkpoint evidence and the draft teaching content for the next cohort. Write it so a trainee starting this stage next cohort could follow the same path. Delete all blockquotes (like this one) before submitting. **Never paste credentials, tokens, or key material.**

## 1. Deliverable & Evidence
> Link the code and provide the exact output proving it works, based on the Definition of Done (DoD) IDs from the brief. Paste real output, trimmed to the relevant lines and redacted, or link a screenshot.

**Repository:** [Link here]
**Overview:** [One or two sentences, in the trainee's own words, on what was built in this stage]
**Deliverable:** the `main/` AWS root module (provider, default tags, private versioned S3 bucket) with `.terraform.lock.hcl`, the reviewed-apply screenshot and the bucket evidence

*   **DoD-m02.2:** The identity-check output, redacted as needed, showing the sandbox account
    *   *Output/Evidence:*
*   **DoD-m02.8:** The CLI or console output showing the public access block and versioning on the bucket
    *   *Output/Evidence:*
*   **DoD-m02.9:** The plan output for the tag change, showing an update only
    *   *Output/Evidence:*

## 2. How to build it (teach it to the next trainee)
> Write a guide covering the scope below, in the order the work was tackled and with the reason for that order. Use the trainee's own output and logs to show reasoning, not just a list of commands. Assume the reader is a peer who has not started this stage yet.

**Guide scope:** from a machine with no AWS setup to a verified private, versioned bucket created by a reviewed apply

## 3. Decisions & Core Concepts
> This section covers the *why* behind the build and the theory applied.

**Key Decisions**
> Design choices and trade-offs, including any alternative that was considered and rejected.

*   **Why do credentials stay outside the `.tf` files, and how did the trainee authenticate instead?**
*   **How did the trainee split the configuration across files, and what does each file own?**
*   **Why is versioning enabled on a bucket that will later hold state?**

**Concepts Applied**
> Pick 2 to 3 of the ideas below. Explain each in the trainee's own words, based on how it was actually used in this stage, as if teaching someone who has never seen it.

**Ideas this stage covers:** the AWS provider, `default_tags`, S3 Block Public Access, version pinning and the lock file, or reading a plan before apply.

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

<!--
=====================================================================
MODULE BRIEF TEMPLATE  (delete this whole comment block before publishing)
=====================================================================
HOW TO REUSE
1. Copy this file to `moduleN_<slug>_brief.md`.
2. Replace every {{PLACEHOLDER}}. Search for "{{" at the end; none may remain.
3. Delete every "AUTHOR NOTE" comment once its section is filled.
4. Work through the CHECKLIST at the bottom before handing it to a trainee.

WHAT IS FIXED (same in every module, do not restyle)
- Section order: Objective, Scope, Stack constraints, Labs, Deliverable, Still open.
- One scope row = one lab = one notebook section, numbered identically (N.1, N.2, ...).
- The last scope row is always the build / run-all / mentor review block.
- Every lab ends with a bold **Acceptance** row the mentor can check yes/no.

WHAT CHANGES PER MODULE
- Title, week, builds-on / feeds-into links.
- Subjects, concepts, research prompts, hours.
- Stack constraints (runtime, tools, data) and lab steps.
- Module-specific deliverable items and diagrams.

SIZING RULES OF THUMB (taken from Module 1)
- 6-10 subjects per module, 1.0-2.5 h each, 12-16 h total.
- 3-6 steps per lab. Each step starts with a verb and names the artifact.
- Each lab builds on the previous one; the last lab integrates all of them.
=====================================================================
-->

# Module {{N}}: {{Module title}}

**Week:** {{week}} of {{total weeks}} · **Builds on:** {{previous module or "none (course entry point)"}} · **Feeds into:** {{next module or "none (course exit point)"}}

## Objective

<!-- AUTHOR NOTE: One or two sentences. Start with "By the end of this stage, the trainee ...".
     Name the concrete end product and how it is run, not the topics covered. -->

By the end of this stage, the trainee {{verb phrase describing what they can do}}, then {{packages / presents / ships it as the end product}}, {{how and where it is run}}.

## Scope

<!-- AUTHOR NOTE: One row per subject. Keep the numbering {{N}}.1, {{N}}.2, ...
     "Key concepts and terms" = vocabulary the mentor expects to hear.
     "Research" = ONE question the trainee must answer in writing (a judgement call, not a definition).
     Last subject row = build, run-all check and mentor review (Research = n/a).
     The Total must equal the sum of the Time column. -->

| # | Subject | Time | Key concepts and terms | Research |
| --- | --- | --- | --- | --- |
| {{N}}.1 | {{Subject 1}} | {{h}} h | {{term, term, term}} | {{judgement question}} |
| {{N}}.2 | {{Subject 2}} | {{h}} h | {{term, term, term}} | {{judgement question}} |
| {{N}}.3 | {{Subject 3}} | {{h}} h | {{term, term, term}} | {{judgement question}} |
| {{N}}.{{last-1}} | {{Subject}} | {{h}} h | {{term, term, term}} | {{judgement question}} |
| {{N}}.{{last}} | {{Build, run-all check, mentor review}} | {{h}} h | See Deliverable | n/a |
| | **Total** | **{{sum}} h** | | |

## Stack constraints

<!-- AUTHOR NOTE: Only rules that apply to the whole module. Keep the three base rows
     (Runtime, Environment, Data) and add rows for anything else that is locked down
     (cloud account, database, compose profile, naming convention, ...).
     Anything inherited from an earlier module: say "Same as Module X" instead of repeating. -->

| Constraint | Detail |
| --- | --- |
| Runtime | {{where everything runs, with minimum versions}} |
| Environment | {{how dependencies are installed and pinned; how changes are applied; what gets recorded in the notebook}} |
| Data | {{source of data, how it is created, where it lives and whether it is visible on the host}} |
| {{Extra constraint}} | {{detail}} |

## Labs

Each lab builds on the previous one. The Acceptance row is what the mentor checks.

<!-- AUTHOR NOTE: Copy the block below once per scope row (except the final build row).
     Acceptance must be checkable without asking the trainee ("both outputs identical",
     "no bare except", "counts add up"), plus at most one short written explanation. -->

### Lab {{N}}.{{x}}: {{Subject}}

| Step | What the trainee does |
| --- | --- |
| 1 | {{Verb + artifact + the specific input or constraint}} |
| 2 | {{Verb + artifact}} |
| 3 | {{Verb + artifact}} |
| Optional | {{Stretch task, if any. Delete the row if none}} |
| **Acceptance** | {{Checkable condition}}. {{Optional: the trainee explains X in one sentence}} |

<!-- ===== repeat the Lab block for every scope row ===== -->

### Lab {{N}}.{{y}}: {{Final integration lab}}

| Step | What the trainee does |
| --- | --- |
| 1 | {{Combines the previous labs into the end product}} |
| 2 | {{Runs it the way a user would (terminal, scheduler, pipeline trigger)}} |
| 3 | {{Shows the same result from the notebook}} |
| **Acceptance** | {{Nothing is duplicated between notebook and product. Both runs give identical output}} |

## Deliverable

<!-- AUTHOR NOTE: First line = exact file names. Then the checklist.
     Items marked [standard] are the same in every module: keep them.
     Items marked [module] are specific: replace or delete. -->

Files: `{{notebook_name}}.ipynb`, {{code folder / other files}}, and `{{dependency or config file}}`. The notebook imports the work and does not duplicate it.

The notebook contains:

- [ ] A title cell (trainee name, stage, date, {{runtime version}}, {{environment or image tag}})  <!-- [standard] -->
- [ ] One section per lab, {{N}}.1 to {{N}}.{{y}}, with code and printed output  <!-- [standard] -->
- [ ] An integration section: {{how the end product runs end to end, and from where}}  <!-- [module] -->
- [ ] Diagrams {{N}}-A {{name}}, {{N}}-B {{name}}, {{N}}-C {{name}} (source or image)  <!-- [module] -->
- [ ] Evidence: {{list of outputs, previews, trees, passing checks}}  <!-- [module] -->
- [ ] A debugging log with at least {{n}} real errors{{, including a named required error if any}}  <!-- [standard] -->
- [ ] A reflection of 3 to 5 sentences  <!-- [standard] -->
- [ ] A clean Kernel, Restart and Run All with no errors  <!-- [standard] -->

The review rubric and pass mark are in [`../README.md`](../README.md).

## Still open / ask the trainer

<!-- AUTHOR NOTE: Real ambiguities only, each with the reason it matters.
     Remove a row once the trainer has answered, and write the decision into the relevant lab instead.
     Keep the last row (depth of write-up) unless the course README already fixes it. -->

| Question | Why it matters |
| --- | --- |
| {{Open design or policy question}} | {{Which labs depend on the answer}} |
| {{Open question}} | {{Why it matters}} |
| How deep should the written explanations be? | Avoids over- or under-investing in the write-up |

<!--
=====================================================================
AUTHOR CHECKLIST (delete before publishing)
=====================================================================
[ ] No "{{" left in the file
[ ] Time column adds up to the Total
[ ] Every scope row has a lab, and lab numbers match scope numbers
[ ] Every lab has an Acceptance row that can be checked yes/no
[ ] Every Research prompt is a judgement question, not a definition
[ ] Every Research prompt is answered somewhere in a lab step or acceptance row
[ ] "Builds on" / "Feeds into" match the neighbouring module briefs
[ ] Stack constraints match the course compose file / environment actually in use
[ ] Deliverable section names every diagram and evidence item the labs produce
[ ] Debugging-log requirement is reachable (some lab creates or likely hits the error)
[ ] All author-note comments removed
=====================================================================
-->

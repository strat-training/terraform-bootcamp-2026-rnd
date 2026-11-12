# Terraform Bootcamp: Build-to-Teach Modules

Each folder here is one stage of the bootcamp. The trainee builds the stage and writes it up at the same time,
so the finished pair, a working build and a guide another trainee could follow, can go into the content library
for the next cohort. The framework is in [`../docs/build-to-teach-framework (1).md`](<../docs/build-to-teach-framework (1).md>).

## Order

Stage folders are numbered in the order they are done, so each checkpoint sits right after the module it follows: `04-cp1-nginx-frontend` after `03-m03-...`, and `10-cp3-multi-env-checkpoint` after `09-m07-...`. The capstone is the single file `CAPSTONE.md`.

## The cycle

1. **The trainer hands over the brief.** `brief.md` states the objective, scope, stack constraints, labs and
   deliverable. It contains no lessons and no answers: the trainee designs and builds the stage.
2. **The trainee builds and writes up the stage together.** Copy `write-up-template.md` to `write-up.md` and fill it in
   as the work goes, not afterwards.
3. **Both get reviewed together.** The trainer reviews the build against the lab Acceptance rows and the write-up
   for whether it is teachable, stage by stage, before the trainee moves on.
4. **The trainee revises** based on that review.
5. **The finished pair goes into the content library.** The trainer decides what carries into the next cohort.

## What is in a stage folder

| File | Written by | Purpose |
| --- | --- | --- |
| `brief.md` | Trainer | Curriculum handoff: objective, scope with time, stack constraints, labs with Acceptance rows (or a definition of done), deliverable, open questions |
| `write-up-template.md` | Trainer | Blank write-up (not for the two checkpoints), with the stage's questions, concept topics and evidence requests filled in |
| `write-up.md` | Trainee | The trainee's own write-up, copied from the template and filled in during the build |
| `tasks.md` | Generated | Checklist bridging the brief and the write-up, produced by `/trainee-task-planner` (not yet generated) |

## Definition-of-done IDs

A stage with a lab uses `DoD-<stage>.<n>`: the n-th sentence of the Acceptance row of that stage's lab, for example
`DoD-m03.4` is the fourth clause of the `m03` lab. A stage without a lab uses a flat `DoD-01`, `DoD-02`, ... list. The same ID is
used in the brief, in the verify tasks and in the write-up evidence.

## Stages

| Folder | Stage | Time | Lab |
| --- | --- | --- | --- |
| `01-m01-iac-foundations/` | IaC Foundations | 5.0 h | 1 lab, 14 steps, 10 Acceptance clauses |
| `02-m02-first-aws-resource/` | First AWS Resource | 5.0 h | 1 lab, 12 steps, 10 Acceptance clauses |
| `03-m03-variables-outputs-data-sources/` | Variables, Outputs and Data Sources | 6.0 h | 1 lab, 12 steps, 10 Acceptance clauses |
| `04-cp1-nginx-frontend/` | Nginx Frontend (Checkpoint 1) | 4.0 h | checkpoint: 5 steps, 6 checks |
| `05-m04-terraform-state/` | Terraform State | 6.0 h | 1 lab, 12 steps, 9 Acceptance clauses |
| `06-m05-loops-meta-arguments/` | Loops and Meta-Arguments | 6.0 h | 1 lab, 17 steps, 13 Acceptance clauses |
| `07-m05-5-conditionals-expressions/` | Conditionals, Expressions and Functions | 5.0 h | 1 lab, 10 steps, 8 Acceptance clauses |
| `08-m06-modules-and-dr/` | Modules and DR Copy | 10.0 h | 1 lab, 19 steps, 14 Acceptance clauses |
| `09-m07-multi-env-microstates/` | Multi-Environment Microstates | 10.5 h | 1 lab, 28 steps, 20 Acceptance clauses |
| `10-cp3-multi-env-checkpoint/` | Multi-Environment Setup (Checkpoint 3) | 3.0 h | checkpoint: 5 steps, 7 checks |
| `CAPSTONE.md` (single file) | Independent Multi-Tier AWS Deployment | 20.0 h | 1 lab (mission stages), 25 steps, 15 DoD items |
| | **Total** | **80.5 h** | |

## Notes

- The hours are the trainer's approved estimates. The source guide gives none.
- Each stage has one lab, as in the source guide. Every scope row names its chunk of that lab in the Lab chunk column, so the
  lab is part of the learning material and not an afterthought. The one exception is `cp3`, whose source is a submission
  table with no build steps, so it uses a definition-of-done table.
- The two checkpoint briefs (`04-cp1-…`, `10-cp3-…`) follow `docs/templates/Checkpoint_Template.md` instead of the standard brief format, and
  carry an instructor-only section at the end of the file. They have no write-up template: each checkpoint folder holds only `brief.md`.
- The capstone brief follows `docs/templates/Capstone_Template.md`, with its instructor-only content
  at the end of `CAPSTONE.md`. Its single lab is the template's six mission stages, and its definition of done is a flat
  `DoD-01` to `DoD-15` list. The scoring rubric is the source guide's. The source gives no pass mark and no rubric for the
  other stages, which are checked against their Acceptance rows.
- Instructor-provided values (region, authentication method, provider pin, instance type, VPC model, state bucket,
  capstone accounts) are listed as open questions in the briefs. They are not filled in here.

# m05: Loops and Meta-Arguments: Tasks

**Objective:** The trainee extends `main/` with a small set of repeatable S3 support buckets defined through loops with predictable names and reviewed changes, and inspects the dependency and lifecycle behaviour of the configuration.

> There is no solutions file for this stage. This checklist guides the work, it does not contain it. Source of every task: [`brief.md`](brief.md).

## ID scheme

| Prefix | Meaning | Source in the brief |
| --- | --- | --- |
| `R-<subject>` | Research task | One per row of the Scope table |
| `B-<lab>.<step>` | Build task | One per step of that lab |
| `V-<lab>.<n>` | Verify task, checks `DoD-<lab>.<n>` | The n-th clause of that lab's Acceptance row |

## Traceability

| Subject | Research | Build | Verify (DoD) |
| --- | --- | --- | --- |
| 5.1 count versus for_each | R-5.1 | B-5.1.1 to B-5.1.7 | V-5.1.1 to V-5.1.5 (DoD-5.1.1 to DoD-5.1.5) |
| 5.2 depends_on and lifecycle | R-5.2 | B-5.2.1 to B-5.2.5 | V-5.2.1 to V-5.2.4 (DoD-5.2.1 to DoD-5.2.4) |
| 5.3 Invalid for_each collection (debug) | R-5.3 | B-5.3.1 to B-5.3.5 | V-5.3.1 to V-5.3.4 (DoD-5.3.1 to DoD-5.3.4) |

## Research

- [ ] **R-5.1** (Scope 5.1): Answer in writing: when is `for_each` usually clearer than `count`, and what can happen when a key is removed from a `for_each` map? Look in the Terraform language docs on meta-arguments.
- [ ] **R-5.2** (Scope 5.2): Answer in writing: what is `depends_on` for, what does `prevent_destroy` protect against, and why inspect the dependency graph and the plan before applying? Look in the meta-arguments docs and the CLI `graph` command reference.
- [ ] **R-5.3** (Scope 5.3): Answer in writing: why is the corrected collection acceptable to `for_each` when the original was not? Look at the documented requirements for the `for_each` argument. Start early and finalise after B-5.3.3.

## Build

### Lab 5.1: Repeatable S3 support buckets

- [ ] **B-5.1.1** (Lab 5.1 step 1): Add the variable holding the small set or map of support bucket purposes.
- [ ] **B-5.1.2** (Lab 5.1 step 2): Define the single `aws_s3_bucket` resource looped over that collection, with names built from the unique prefix plus the key.
- [ ] **B-5.1.3** (Lab 5.1 step 3): Add the matching Block Public Access and versioning resources for each bucket, keyed the same way.
- [ ] **B-5.1.4** (Lab 5.1 step 4): Run `fmt`, `validate` and `plan`, and read the addresses to confirm one instance per key.
- [ ] **B-5.1.5** (Lab 5.1 step 5): Apply, then run `terraform state list` and identify each instance's address.
- [ ] **B-5.1.6** (Lab 5.1 step 6): Remove one key, run `plan`, identify which instance is proposed for destruction, and restore the key unless it is meant to go.
- [ ] **B-5.1.7** (Lab 5.1 step 7): Create the optional resource switched by `enable_archive`, toggle the variable and compare the plans before applying.

### Lab 5.2: Dependencies and lifecycle

- [ ] **B-5.2.1** (Lab 5.2 step 1): Inspect the resource references and note which dependencies Terraform infers.
- [ ] **B-5.2.2** (Lab 5.2 step 2): Run `terraform graph`, inspect it, and identify how a bucket relates to its configuration resources.
- [ ] **B-5.2.3** (Lab 5.2 step 3): Read the `depends_on` docs, then add it only if a dependency exists that cannot be inferred from references.
- [ ] **B-5.2.4** (Lab 5.2 step 4): Set `prevent_destroy = true` temporarily on a disposable lab bucket, run `terraform plan -destroy`, observe the result, and remove the setting before final cleanup.
- [ ] **B-5.2.5** (Lab 5.2 step 5): Work out what operational problem `create_before_destroy` and `ignore_changes` each address, without adding either.

### Lab 5.3: Debug exercise: invalid for_each collection

- [ ] **B-5.3.1** (Lab 5.3 step 1): Start from the list-typed `support_bucket_names` and temporarily point `for_each` at the variable directly.
- [ ] **B-5.3.2** (Lab 5.3 step 2): Run `terraform plan`, read the error, and do not apply.
- [ ] **B-5.3.3** (Lab 5.3 step 3): Diagnose the error and correct the configuration so the plan succeeds.
- [ ] **B-5.3.4** (Lab 5.3 step 4): Run `fmt`, `validate` and `plan` again.
- [ ] **B-5.3.5** (Lab 5.3 step 5): Restore the intended support-bucket set and confirm the plan proposes no unrelated changes.

## Verify

### Lab 5.1

- [ ] **V-5.1.1** (checks DoD-5.1.1): Check the plan shows one bucket instance per key, with matching Block Public Access and versioning instances for the same keys. *Validates R-5.1, B-5.1.2, B-5.1.3, B-5.1.4.*
- [ ] **V-5.1.2** (checks DoD-5.1.2): Check `terraform state list` shows an address for each instance. *Validates B-5.1.5.*
- [ ] **V-5.1.3** (checks DoD-5.1.3): Check removing a key plans the destruction of exactly that key's instance. *Validates R-5.1, B-5.1.6.*
- [ ] **V-5.1.4** (checks DoD-5.1.4): Check toggling `enable_archive` plans the optional resource to appear and disappear. *Validates B-5.1.7.*
- [ ] **V-5.1.5** (checks DoD-5.1.5): Check the choice of `count` or `for_each` for each repeated resource is explained in writing. *Validates R-5.1, B-5.1.2, B-5.1.7.*

### Lab 5.2

- [ ] **V-5.2.1** (checks DoD-5.2.1): Check the relationship between a bucket and its configuration resources is named from the `terraform graph` output. *Validates B-5.2.1, B-5.2.2.*
- [ ] **V-5.2.2** (checks DoD-5.2.2): Check a destroy plan against the protected bucket is blocked by `prevent_destroy` and the setting is removed afterwards. *Validates R-5.2, B-5.2.4.*
- [ ] **V-5.2.3** (checks DoD-5.2.3): Check any `depends_on` in the configuration is justified as a dependency Terraform cannot infer. *Validates B-5.2.1, B-5.2.3.*
- [ ] **V-5.2.4** (checks DoD-5.2.4): Check there is a one-sentence explanation for each of `create_before_destroy` and `ignore_changes`. *Validates R-5.2, B-5.2.5.*

### Lab 5.3

- [ ] **V-5.3.1** (checks DoD-5.3.1): Check the broken configuration produced a plan error and nothing was applied. *Validates B-5.3.1, B-5.3.2.*
- [ ] **V-5.3.2** (checks DoD-5.3.2): Check the corrected configuration plans successfully. *Validates B-5.3.3, B-5.3.4.*
- [ ] **V-5.3.3** (checks DoD-5.3.3): Check that after the intended set is restored, the plan proposes no unrelated changes. *Validates B-5.3.5.*
- [ ] **V-5.3.4** (checks DoD-5.3.4): Check there is an explanation of why the corrected collection is valid for `for_each`. *Validates R-5.3, B-5.3.3.*

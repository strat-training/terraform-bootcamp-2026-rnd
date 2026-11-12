# m05: Loops and Meta-Arguments

**Builds on:** `05-m04-terraform-state` · **Feeds into:** `07-m05-5-conditionals-expressions`

## Objective

By the end of this stage, the trainee has extended `main/` with a small set of repeatable S3 support buckets defined through loops with predictable names and reviewed changes, and has inspected the dependency and lifecycle behaviour of the configuration.

## Scope

| # | Subject | Time | Key concepts and terms | Research | Lab chunk |
| --- | --- | --- | --- | --- | --- |
| 5.1 | count versus for_each | 3.0 h | `count`, `for_each`, `each.key`, indexed and keyed addresses, DRY principle, removed-key behaviour, `terraform state list` | When is `for_each` usually clearer than `count`, and what can happen when a key is removed from a `for_each` map? | Steps 1–7: Repeatable S3 support buckets |
| 5.2 | depends_on and lifecycle | 1.5 h | implicit dependency, `depends_on`, `terraform graph`, `prevent_destroy`, `create_before_destroy`, `ignore_changes` | What is `depends_on` for, what does `prevent_destroy` protect against, and why inspect the dependency graph and the plan before applying? | Steps 8–12: Dependencies and lifecycle |
| 5.3 | Invalid for_each collection (debug) | 1.5 h | `for_each` argument types, list versus set, stable keys, plan error reading | Why is the corrected collection acceptable to `for_each` when the original was not? | Steps 13–17: Debug exercise: invalid for_each collection |
| | **Total** | **6.0 h** | | | |

## Stack constraints

| Constraint | Detail |
| --- | --- |
| Runtime | Same as m04. |
| Environment | Continue in `main/`. State is remote in S3. Keep the existing bucket and any active resources already managed by this root module. |
| Data | Support bucket names are built from the unique learner or project prefix plus the key. The support set is kept short to limit resource creation. |
| Lab conventions | Same as m02 (authentication, region, default tags, operating checklist, plan before apply, state handling, cost control, public access). |
| Cleanup | The support resources are kept if the instructor wants to continue with them, otherwise only the resources created in this stage are destroyed. The remote backend and shared state bucket are never removed. |

## Lab

This stage has one lab, built up in chunks. The Lab chunk column in the Scope table shows which steps practise each subject. The Acceptance row is what the trainer checks.

### Lab m05: Repeatable support buckets, dependencies and lifecycle

| Step | What the trainee does |
| --- | --- |
| 1 | Adds a variable holding a small set or map of support bucket purposes, such as logs and artifacts. |
| 2 | Defines one `aws_s3_bucket` resource with `for_each` over the supplied collection, building each name from the unique prefix plus the key. |
| 3 | Adds a matching Block Public Access resource and versioning configuration for each bucket, using the same key so related objects stay aligned. |
| 4 | Runs `fmt`, `validate` and `plan`, reads the addresses and confirms Terraform proposes one instance per key. |
| 5 | Applies, then runs `terraform state list` and identifies the address of each instance. |
| 6 | Removes one key from the input collection, runs `plan`, identifies which instance Terraform proposes to destroy, and restores the key unless it is meant to go. |
| 7 | Creates a separate optional resource whose existence is switched by `enable_archive`, toggles the variable and compares the plans before applying. |
| 8 | Inspects the resource references in the configuration and notes which dependencies Terraform infers from attribute references. |
| 9 | Runs `terraform graph`, inspects the dependency graph and identifies the relationship between a bucket and its associated configuration resources. |
| 10 | Reads the `depends_on` documentation and adds it only where a genuine dependency cannot be inferred from references. |
| 11 | Temporarily sets `prevent_destroy = true` on a disposable lab bucket, runs `terraform plan -destroy`, observes the safeguard and removes the setting before final cleanup. |
| 12 | Discusses `create_before_destroy` and `ignore_changes`, explaining what operational problem each is meant to solve without adding either blindly. |
| 13 | Starts from the support-bucket exercise in `main/`, where `support_bucket_names` is declared as a list of strings, and temporarily points `for_each` at the variable directly. |
| 14 | Runs `terraform plan`, reads the error and does not apply. |
| 15 | Corrects the configuration so the plan succeeds. |
| 16 | Runs `fmt`, `validate` and `plan` again. |
| 17 | Restores the intended support-bucket set and confirms the plan proposes no unrelated changes. |
| **Acceptance** | The plan shows one bucket instance per key, with matching Block Public Access and versioning instances for the same keys. `terraform state list` shows an address for each instance. Removing a key plans the destruction of exactly that key's instance. Toggling `enable_archive` plans the optional resource to appear and disappear. The choice of `count` or `for_each` for each repeated resource is explained in writing. The relationship between a bucket and its configuration resources is named from the `terraform graph` output. A destroy plan against the protected bucket is blocked by `prevent_destroy`, and the setting is removed afterwards. Any `depends_on` in the configuration is justified as a dependency Terraform cannot infer. The trainee explains in one sentence each what `create_before_destroy` and `ignore_changes` are meant to solve. The broken configuration produced a plan error, and nothing was applied. The corrected configuration plans successfully. After the intended set is restored, the plan proposes no unrelated changes. The trainee explains why the corrected collection is valid for `for_each`. |

## Deliverable

The deliverable contains:

- [ ] Updated `main/` files
- [ ] Plan screenshot showing keyed instances
- [ ] `README.md` for the stage
- [ ] `write-up.md`, filled in from `write-up-template.md`

## Still open / ask the trainer

| Question | Why it matters |
| --- | --- |
| Which support bucket purposes does the set start with? The source offers logs and artifacts as examples. | Lab chunk 5.1 |
| Does the README rationale cover every repeated resource, or only the support buckets and the archive bucket? | Acceptance of the lab (chunk 5.1) |
| Are the support resources kept or destroyed at the end of the stage? | Cleanup and the starting state of m05-5 |
| How deep should the written explanations be? | Avoids over- or under-investing in the write-up |

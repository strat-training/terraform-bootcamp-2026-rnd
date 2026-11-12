# m04: Terraform State

**Builds on:** `04-cp1-nginx-frontend` · **Feeds into:** `06-m05-loops-meta-arguments`

## Objective

By the end of this stage, the trainee has migrated the existing `main/` local state to a versioned S3 backend with native locking without recreating any resource, and a plan run immediately afterwards shows no unexpected changes.

## Scope

| # | Subject | Time | Key concepts and terms | Research | Lab chunk |
| --- | --- | --- | --- | --- | --- |
| 4.1 | State, drift and local-state risks | 1.5 h | state file, address-to-resource mapping, state drift, concurrent operations, sensitive state | What does Terraform state map, and why is local state difficult when several engineers work on the same infrastructure? | Steps 1–3: Take stock of the local state |
| 4.2 | Backend bucket preparation | 1.5 h | state bucket, bootstrap configuration, versioning, Block Public Access, encryption, unique state key | Why must the backend bucket exist before `terraform init`, and why should it not be created by the root module that uses it as its backend? | Steps 4–7: Prepare the backend bucket |
| 4.3 | State migration | 3.0 h | S3 backend, `use_lockfile`, `.tflock` object, `init -migrate-state`, `state list`, no-change plan | What does `use_lockfile = true` enable, and what should a no-change plan after migration tell the trainee? | Steps 8–12: Migrate the existing main state |
| | **Total** | **6.0 h** | | | |

## Stack constraints

| Constraint | Detail |
| --- | --- |
| Runtime | Same as m02. Terraform CLI 1.10 or later is required for S3 native lock files. |
| Environment | Continue in the existing `main/` root module. Do not start a fresh configuration. |
| Backend | S3 backend with `use_lockfile = true` for native locking. DynamoDB-based locking is deprecated and is not copied from older material. Backend configuration does not accept normal input variable references. Authentication stays outside the code. |
| Data | The state bucket is instructor-provided or created from a separate bootstrap configuration, and exists before backend initialisation. State files are sensitive: backed up outside Git, never uploaded to a public location, never shown in screenshots, never submitted. |
| Lab conventions | Same as m02 (authentication, region, default tags, operating checklist, plan before apply, state handling, cost control, public access). |
| Cleanup | The state bucket and remote state object are kept for m05 and m06. The backend bucket is a protected shared dependency and is excluded from routine destroy commands. |

## Lab

This stage has one lab, built up in chunks. The Lab chunk column in the Scope table shows which steps practise each subject. The Acceptance row is what the trainer checks.

### Lab m04: Prepare the backend bucket and migrate the main state

| Step | What the trainee does |
| --- | --- |
| 1 | Runs `terraform state list` against the local state and records the resources it tracks, as the baseline for the migration. |
| 2 | Confirms no other learner or process is running `terraform apply` against this state and finishes any in-progress operation. |
| 3 | Backs up the local `terraform.tfstate` to a secure location outside the Git repository. |
| 4 | Confirms the instructor provided a dedicated state bucket, or creates it from a separate bootstrap folder or configuration rather than from the root module that will use it as its backend. |
| 5 | Enables versioning, Block Public Access and encryption according to the sandbox standard and restricts access to the training participants who need it. |
| 6 | Chooses a state key for `main/` that differs from the keys of every other root and environment. |
| 7 | Confirms the AWS identity used by Terraform has the S3 permissions required for the state object and its lock file. |
| 8 | Adds a backend configuration to the Terraform block in `main/` with the approved bucket, the unique key, the region, encryption and native locking. |
| 9 | Runs `terraform init -migrate-state` and follows the prompt to copy the existing state to the S3 backend, without choosing a fresh or empty state. |
| 10 | Confirms initialisation completed and that the state object exists in the approved bucket, without downloading or exposing the state contents. |
| 11 | Runs `terraform state list` to confirm the previously managed resources are still tracked. |
| 12 | Runs `terraform plan` and checks it shows no infrastructure changes unless configuration was intentionally changed. |
| Optional | Shows a partner the backend key, without credentials or state contents, and confirms both identify the same key. |
| **Acceptance** | The resources tracked in the local state are recorded as a baseline. A backup of the local state is held outside the repository. The state bucket exists outside `main/`'s own configuration with versioning, Block Public Access and encryption enabled. The chosen state key is unique among all roots. The Terraform identity has permissions on the state object and its lock file. The state object exists in the S3 bucket under the chosen key. `terraform state list` shows the same resources as the baseline taken before the migration. A plan run straight after migration shows no infrastructure changes. The trainer has verified the remote key and locking configuration. |

## Deliverable

The deliverable contains:

- [ ] Backend configuration for `main/`, with the bucket name redacted if required
- [ ] Bootstrap configuration for the state bucket, if the trainee created it
- [ ] Screenshot of the successful `terraform init -migrate-state`
- [ ] `terraform state list` output
- [ ] No-change plan output
- [ ] `write-up.md`, filled in from `write-up-template.md`

## Still open / ask the trainer

| Question | Why it matters |
| --- | --- |
| Who provisions the state bucket, and which bucket name and encryption standard apply? | Lab chunk 4.2 |
| Which state key naming is expected for `main/`? | Lab chunks 4.2 and 4.3 |
| What is the safe location for the local state backup? | Lab chunk 4.1 |
| How deep should the written explanations be? | Avoids over- or under-investing in the write-up |

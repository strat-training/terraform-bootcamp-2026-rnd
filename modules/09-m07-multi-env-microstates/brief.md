# m07: Multi-Environment Microstates

**Builds on:** `08-m06-modules-and-dr` · **Feeds into:** `10-cp3-multi-env-checkpoint`

## Objective

By the end of this stage, the trainee has a fresh `multi-env/` repository with separate network and app root modules for dev and staging, each with its own remote state key, a documented deployment sequence and a deployed dev environment, with staging applied only if the instructor approves.

## Scope

| # | Subject | Time | Key concepts and terms | Research | Lab chunk |
| --- | --- | --- | --- | --- | --- |
| 7.1 | Directory structure and state keys | 2.5 h | root module per directory, backend key, environment tfvars, shared module source, blast radius | Why does each root module need a unique backend key, and why separate network and application state? | Steps 1–6: Create the directory structure |
| 7.2 | Network root | 3.0 h | network layer, deployment order, documented outputs, state read access | Which layer is deployed first, and why? | Steps 7–12: Deploy the network state first |
| 7.3 | App root and state sharing | 2.5 h | `terraform_remote_state`, full-state read access, access control, `web_app` module | What is the security trade-off of `terraform_remote_state`? | Steps 13–18: Deploy the application state |
| 7.4 | Promotion and environment comparison | 1.5 h | code promotion, environment tfvars, module version or source reference, deployment and teardown order | How does using the same module with different tfvars support code promotion? | Steps 19–23: Promote code and compare environments |
| 7.5 | Missing network outputs (debug) | 1.0 h | remote-state read error, state key mismatch, plan error reading | What should be checked first when the app root cannot read network values? | Steps 24–28: Debug exercise: app root cannot find network outputs |
| | **Total** | **10.5 h** | | | |

## Stack constraints

| Constraint | Detail |
| --- | --- |
| Runtime | Same as m04. |
| Environment | A new `multi-env/` directory created from scratch. The m06 main lab resources are already destroyed. The `modules/network` and `modules/web_app` source is reused, preferably by shared reference rather than duplication. Each root has its own backend key. |
| Data | Inter-state communication uses `terraform_remote_state` only. Read access to a state object can reveal the full state contents, not just outputs, so access is restricted accordingly. |
| Environments | Dev is deployed. Staging is applied only if the sandbox budget and the instructor allow it, otherwise it is plan-only. Production, if shown, is a directory and a plan-only exercise unless the instructor provides an approved production-like sandbox. |
| Lab conventions | Same as m02 (authentication, region, default tags, operating checklist, plan before apply, state handling, cost control, public access). |
| Cleanup | App state is destroyed first, then network state, for each environment, from each root directory, verifying only the intended state is affected. The remote backend bucket and its state objects are kept until the instructor confirms the lab is complete. |

## Lab

This stage has one lab, built up in chunks. The Lab chunk column in the Scope table shows which steps practise each subject. The Acceptance row is what the trainer checks.

### Lab m07: Multi-environment microstates

| Step | What the trainee does |
| --- | --- |
| 1 | Creates `multi-env/` and copies or references the network and web_app modules from m06, preferring a shared module source over manual duplication. |
| 2 | Creates `environments/dev/network`, `environments/dev/app`, `environments/staging/network` and `environments/staging/app`. |
| 3 | Treats each of the four directories as a separate root module with its own Terraform configuration and backend key. |
| 4 | Uses a clear file layout in each root: `versions.tf`, `backend.tf`, `providers.tf`, `main.tf`, `variables.tf`, `outputs.tf` and an environment-specific tfvars file where useful. |
| 5 | Assigns a unique S3 backend key to each root. |
| 6 | Checks that no two roots point at the same backend key and that network and application layers do not share a state file. |
| 7 | Opens `environments/dev/network` and configures its S3 backend with the approved shared state bucket and the dev network key. |
| 8 | Calls the network module and passes the dev CIDRs, Availability Zones and naming values. |
| 9 | Runs `init`, `fmt`, `validate` and `plan`, reviews the network-only resources and applies after approval. |
| 10 | Exposes the VPC and subnet information through documented outputs. |
| 11 | Prepares the outputs the app root will read across the state boundary with `terraform_remote_state`. |
| 12 | Notes that read access to the state object can reveal the full state contents, not just the outputs, and restricts access accordingly. |
| 13 | Opens `environments/dev/app` and configures a different S3 backend key for it. |
| 14 | Reads the network values through `terraform_remote_state`, without copying IDs from a terminal into resource arguments unless the activity asks for a manual-input test. |
| 15 | Calls the `web_app` module and passes the required VPC, subnet, AMI and instance type values. |
| 16 | Runs `init`, `fmt`, `validate` and `plan`, confirming the app plan proposes no changes to the network state. |
| 17 | Applies after approval and verifies the Nginx endpoint. |
| 18 | Creates the matching staging roots using the same module source but different tfvars and backend keys, runs `plan` for staging and applies staging only if the instructor approves the extra resources and cost. |
| 19 | Keeps module code the same for dev and staging and changes environment values only in the environment-specific tfvars files. |
| 20 | Compares the two plans and identifies which settings differ and which resources remain structurally identical. |
| 21 | Updates a shared module version or source reference in a controlled change, then plans dev and staging separately. |
| 22 | Documents the deployment order and the teardown order. |
| 23 | Adds no production environment to the live lab unless the instructor provides an approved production-like sandbox, otherwise representing production as a directory with a plan-only exercise. |
| 24 | Starts from the dev app root after the dev network root has been applied, and temporarily changes the remote-state key so it points to a network value that does not exist, without applying. |
| 25 | Runs `terraform plan` and inspects the error or missing value. |
| 26 | Diagnoses the cause by comparing the configured reference with the dev network root. |
| 27 | Fixes the reference and confirms the app root reads the intended VPC and subnet values. |
| 28 | Runs `plan` again and verifies the app root proposes only the expected application resources. |
| **Acceptance** | Four root directories exist, each with its own backend configuration. No two roots share a backend key. For each root, the trainee states the key it uses and the resources it owns. The network and application layers do not share a state file. The dev network plan creates only network resources. The dev network apply succeeds. The VPC and subnet outputs are documented and readable from another root. The dev app plan proposes no changes to network resources. The dev app root reads the VPC and subnet values from the network state without manual copying. The Nginx endpoint loads in a browser. The staging plan runs against its own state key. Staging is applied only with instructor approval. Module code is identical for dev and staging, and only the tfvars differ. The settings that differ between the two plans are listed. The module change is planned separately for dev and for staging. Deployment order is network first, app second, and teardown order is app first, network second. The plan with the broken reference produced an error, and nothing was created or destroyed. The cause is identified from the error and the network root. After the fix, the app plan reads the intended values and proposes only expected application resources. The correct reference is restored in source control. |

## Deliverable

The deliverable contains:

- [ ] `multi-env/` repository with shared modules and `environments/`
- [ ] Directory tree
- [ ] Backend key mapping
- [ ] Dev apply screenshot
- [ ] Nginx browser screenshot
- [ ] Staging plan output
- [ ] Short note on the inter-state method and its access implications
- [ ] `write-up.md`, filled in from `write-up-template.md`

## Still open / ask the trainer

| Question | Why it matters |
| --- | --- |
| Is staging applied or plan-only? | Cost and lab chunk 7.3; cp3 evidence depends on it |
| Who is allowed to read which state objects? | The access restriction in lab chunk 7.2 |
| Is a production directory shown as a plan-only exercise? | Lab chunk 7.4 |
| How deep should the written explanations be? | Avoids over- or under-investing in the write-up |

# m02: First AWS Resource

**Builds on:** `01-m01-iac-foundations` · **Feeds into:** `03-m03-variables-outputs-data-sources`

## Objective

By the end of this stage, the trainee has a `main/` AWS root module with provider configuration, default tags and one private, versioned S3 bucket, applies it after reviewing the plan and verifies the bucket in AWS, with no credentials in any Terraform file.

## Scope

| # | Subject | Time | Key concepts and terms | Research | Lab chunk |
| --- | --- | --- | --- | --- | --- |
| 2.1 | AWS authentication | 1.0 h | AWS CLI v2, IAM Identity Center profile, `AWS_PROFILE`, `aws sts get-caller-identity`, sandbox account, no long-lived access keys | Why should credentials stay outside `.tf` files, and how does the trainee confirm which account Terraform will act on? | Steps 1–4: Authenticate to the sandbox |
| 2.2 | Files, version pinning and the provider block | 1.5 h | HCL files, `versions.tf`, `providers.tf`, required Terraform version, provider source, `.terraform.lock.hcl`, AWS provider, region, `default_tags` | What does the provider block configure, why is it kept separate from resource definitions, and why are default tags useful in a shared sandbox? | Steps 5–8: Project files, version pinning and provider configuration |
| 2.3 | Private versioned S3 bucket | 2.5 h | S3 bucket, unique bucket name or `bucket_prefix`, S3 Block Public Access, versioning, plan review | What is S3 Block Public Access for, and why is bucket versioning useful for a bucket that will later hold Terraform state? | Steps 9–12: Create and verify the first bucket |
| | **Total** | **5.0 h** | | | |

## Stack constraints

| Constraint | Detail |
| --- | --- |
| Prerequisite knowledge | Introductory AWS exposure (S3, IAM and VPC basics) is assumed. Terraform is new to the trainee. |
| Runtime | Terraform CLI 1.10 or later (or a newer version the instructor approves), because m04 uses S3 native lock files. AWS CLI v2. The `hashicorp/aws` provider at an instructor-approved pinned version. |
| Environment | The instructor-provided sandbox account, approved region and authentication method. No IAM users or long-lived access keys. Work in a new `main/` root module that m03 to m06 extend. |
| Data | S3 bucket names are globally unique. State is local at this point and is migrated in m04. |
| Authentication | Credentials are never placed in `.tf` files, screenshots or Git. |
| Region | One approved region throughout the bootcamp unless an activity says otherwise. |
| Default tags | Project, Environment, ManagedBy and Owner on supported resources, using the provider `default_tags` block where appropriate. |
| Plan before apply | Every create, update, replacement and destroy action in `terraform plan` is read before apply. |
| State handling | State files and saved plan files are sensitive and are not committed to source control. |
| Cost control | Only instructor-approved instance types and resource counts. Lab resources are destroyed after evidence is captured. |
| Public access | HTTP is exposed only where an Nginx exercise needs browser testing. SSH and databases are never exposed to the internet. |
| Operating checklist | At every stage and checkpoint: before running Terraform, confirm the folder, the AWS identity, the backend key and that nobody else is applying to the same state. During plan review, confirm the account and region, read every action and check names, sizes, CIDRs and exposure. After apply, verify the resources, outputs and application health and capture only the evidence requested. The source guide's common-errors table is the first place to look when a command fails. |
| Cleanup | The bucket is kept for the next stages. It is destroyed only if the instructor asks for a reset. |

## Lab

This stage has one lab, built up in chunks. The Lab chunk column in the Scope table shows which steps practise each subject. The Acceptance row is what the trainer checks.

### Lab m02: Prepare the AWS environment and the first bucket

| Step | What the trainee does |
| --- | --- |
| 1 | Installs the AWS CLI v2 if missing and confirms it runs, and confirms the Terraform CLI from m01 is 1.10 or later, or the newer version the instructor approves. |
| 2 | Authenticates using the method approved for the sandbox. |
| 3 | Selects the correct profile where required, keeping access keys out of Terraform files. |
| 4 | Runs the caller-identity check and confirms the account is the training sandbox, not a personal or production account. |
| 5 | Creates `main/` with `versions.tf`, `providers.tf`, `main.tf`, `variables.tf` and `outputs.tf`, one clear responsibility per file. |
| 6 | Declares the required Terraform version range and the `hashicorp/aws` provider source in `versions.tf` and pins an approved provider version. |
| 7 | Configures the approved region and the default tags in `providers.tf`, declaring the region and owner as input variables in the same root module. |
| 8 | Runs `terraform init` to install the pinned provider and commits the generated `.terraform.lock.hcl`. |
| 9 | Creates one S3 bucket in `main.tf` with a unique name or prefix, enables Block Public Access and versioning, and configures no public website hosting. |
| 10 | Runs `fmt`, `validate` and `plan`, and checks the bucket name, region, tags, versioning and public-access settings in the plan. |
| 11 | Applies only after the plan matches the intended result, then verifies the bucket in the AWS console or with the AWS CLI. |
| 12 | Changes one non-sensitive tag, runs `plan` again and confirms the plan shows only the expected update before applying. |
| **Acceptance** | The AWS CLI runs and the Terraform CLI meets the required version. The account returned by the identity check is the training sandbox. No IAM user or long-lived access key was created for this stage. `versions.tf` declares the Terraform version range and the pinned `hashicorp/aws` provider. The provider block uses the approved region and the Project, Environment, ManagedBy and Owner default tags, with region and owner as variables. `.terraform.lock.hcl` is committed. Each of the five files has one clear responsibility. The bucket exists with public access blocked and versioning enabled. The tag change plans as the expected update only. No credentials or state files are in the submission. |

## Deliverable

The deliverable contains:

- [ ] `main/` source files (`versions.tf`, `providers.tf`, `main.tf`, `variables.tf`, `outputs.tf`)
- [ ] `.terraform.lock.hcl`
- [ ] Screenshot of the reviewed apply
- [ ] Evidence that the bucket exists, from the console or the CLI
- [ ] `write-up.md`, filled in from `write-up-template.md`

## Still open / ask the trainer

| Question | Why it matters |
| --- | --- |
| Which authentication method, profile name and region apply to the sandbox? | Lab chunks 2.1 and 2.2 depend on them |
| Which provider version is approved for pinning? | Lab chunk 2.2, and every later stage inherits the pin |
| What learner ID and bucket name prefix are used? | Owner tag in lab chunk 2.2 and globally unique bucket naming in lab chunk 2.3 |
| How deep should the written explanations be? | Avoids over- or under-investing in the write-up |

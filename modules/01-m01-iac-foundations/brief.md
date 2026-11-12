# m01: IaC Foundations

**Builds on:** none (course entry point) · **Feeds into:** `02-m02-first-aws-resource`

## Objective

By the end of this stage, the trainee runs the Terraform init, format, validate, plan, apply and destroy cycle against a local-only configuration that manages a text file, then submits it as the `intro/` project with a README recording the commands used and the expected result, without any cloud credentials.

## Scope

| # | Subject | Time | Key concepts and terms | Research | Lab chunk |
| --- | --- | --- | --- | --- | --- |
| 1.1 | IaC and the Terraform workflow | 1.0 h | Infrastructure as Code, configuration as intended state, recorded state, the init, plan, apply and destroy workflow, Terraform CLI | How does Infrastructure as Code differ from manual console changes, and what does Terraform compare the configuration against when it plans? | Steps 1–2: Install Terraform and create the project |
| 1.2 | HCL syntax | 1.0 h | HCL, block, block type, block label, argument, nested block, comment, expression, reference to another block, value types (string, number, bool, list, map) | How does a Terraform block say what it describes, and how does one block refer to another? | Steps 3–4: Read HCL |
| 1.3 | Providers, resources and modules | 1.0 h | provider, local provider, resource, `local_file`, module, root module, `init`, `fmt`, `validate` | What is the difference between a provider and a resource, and why does a new root module need initialising before anything else? | Steps 5–7: First configuration and initialisation |
| 1.4 | Plan, apply, state and destroy | 2.0 h | `plan` as a review step, plan actions (create, update in place, replace, destroy), resource address, `apply`, `destroy`, `terraform.tfstate`, `.terraform/` | What must the trainee verify in a plan before approving an apply or destroy, does a plan alone change anything, and why should `terraform.tfstate` not be committed to Git? | Steps 8–14: Plan, apply, update and destroy |
| | **Total** | **5.0 h** | | | |

## Stack constraints

| Constraint | Detail |
| --- | --- |
| Runtime | Terraform CLI with the local provider, installed in lab chunk 1.1. The version is instructor-provided; 1.10 or later is needed from m02. |
| Environment | Work only in `intro/`, which is a root module. No AWS login or credentials. |
| Data | One local text file, `hello.txt`. `terraform.tfstate` appears after apply and is observed, not submitted. |
| Cleanup | The source code is kept for reference after destroy. |

## Lab

This stage has one lab, built up in chunks. The Lab chunk column in the Scope table shows which steps practise each subject. The Acceptance row is what the trainer checks.

### Lab m01: Create a local text file with Terraform

| Step | What the trainee does |
| --- | --- |
| 1 | Installs the Terraform CLI if it is missing and confirms the command runs from the terminal. |
| 2 | Creates the project folder `terraform-bootcamp/intro` and works only inside it. |
| 3 | Reads the Terraform language overview in the HashiCorp documentation. |
| 4 | Takes an example configuration from the documentation and labels each part: block type, block labels, arguments, a nested block, comments, and any reference from one block to another. |
| 5 | Writes `main.tf` with the Terraform and local provider requirements and one `local_file` resource with a `./hello.txt` filename and a short content string. |
| 6 | Runs `terraform init` and confirms the local provider is downloaded and the directory is initialised. |
| 7 | Runs `terraform fmt`, then `terraform validate`, and fixes any formatting or syntax errors. |
| 8 | Runs `terraform plan`, reads the output and identifies the resource Terraform intends to create and its address. |
| 9 | Runs `terraform apply`, approves it, confirms `hello.txt` exists and inspects its contents. |
| 10 | Opens `terraform.tfstate` after the apply and identifies how it records the resource, without committing it. |
| 11 | Changes the content string, runs `fmt`, `validate` and `plan` again, identifies the proposed change, then applies it. |
| 12 | Runs `terraform destroy`, reviews the deletion plan, approves it and confirms the file is gone. |
| 13 | Repeats the workflow without copying commands, writing down what the plan proposes before each apply and destroy. |
| 14 | Writes `README.md` recording the commands used and the expected result. |
| **Acceptance** | The `terraform` command runs from the terminal. `intro/` exists and is the only folder used for this stage. The annotated example names the block type, labels, arguments, nested block and comments. The annotated example identifies at least one reference from one block to another. `terraform init` downloads the local provider and initialises the directory. `terraform fmt` makes no further changes and `terraform validate` reports the configuration valid. `hello.txt` is created by apply and removed by destroy. Plan alone creates no file. The content change appears as an in-place update in the plan before it is applied. `.terraform/` and `*.tfstate*` are excluded from version control. |

## Deliverable

The deliverable contains:

- [ ] `intro/` folder containing `main.tf`
- [ ] `intro/README.md` (commands used and expected result)
- [ ] Terminal screenshot of init, plan, apply and destroy (optional instructor evidence)
- [ ] `write-up.md`, filled in from `write-up-template.md`

## Still open / ask the trainer

| Question | Why it matters |
| --- | --- |
| Which Terraform CLI version is installed in the training environment? | All later stages assume 1.10 or later |
| How deep should the written explanations be? | Avoids over- or under-investing in the write-up |

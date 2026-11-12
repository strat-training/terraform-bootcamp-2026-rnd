**TERRAFORM BOOTCAMP**

**Practical Guide**

A continuous, hands-on path from Terraform basics to multi-environment AWS infrastructure

**LOCAL WORKFLOW → AWS RESOURCES → NGINX → REMOTE STATE → REUSABLE CODE → MODULES → MICROSTATES**

AWS-first \| Beginner-friendly \| Lab-driven

Prepared as a learner-facing guide with instructor checkpoints and cleanup instructions

Curriculum baseline: Terraform Bootcamp Curriculum (uploaded source)  
Practical sequencing and current official references added

# How to use this guide

This bootcamp is designed for learners who already have introductory AWS exposure and are new to Terraform. It moves from local-only practice to a small AWS deployment, then progressively improves that same body of work through variables, state management, expressions, modules, and environment separation.

| **The continuity rule** Use the same main lab repository from Module 2 through Module 6. Do not restart the project at every module. Module 1 is intentionally local-only. Module 7 is intentionally a fresh directory because it teaches how to establish independent roots and state files. |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|

The guide is written as a practical sequence. Each module tells the learner what they should already have, what to change, how to run it, what success looks like, what to submit, and what to clean up. The instructor should provide the approved AWS sandbox account, region, permissions, and network guidance before learners begin AWS labs.

## Bootcamp outcomes

- Explain the purpose of Infrastructure as Code and the Terraform plan/apply workflow.

- Create and update AWS resources using HCL without hardcoding credentials.

- Use variables, outputs, data sources, expressions, and functions to make configurations reusable.

- Explain Terraform state, protect it, and move a local state file to a remote S3 backend.

- Use count, for_each, depends_on, and lifecycle settings appropriately.

- Build and consume reusable Terraform modules.

- Separate environments and infrastructure layers into independent root modules and state files.

- Plan, deploy, validate, document, and safely tear down a multi-tier AWS capstone.

## Learning path and lab continuity

| Step | Module / checkpoint                        | Lab location          | What carries forward                                                                       |
|------|--------------------------------------------|-----------------------|--------------------------------------------------------------------------------------------|
| 1    | Module 1: IaC foundations                  | intro/                | A local-only Terraform configuration and familiarity with the CLI.                         |
| 2    | Module 2: First AWS resource               | main/                 | The main AWS root module, provider configuration, tags, and an S3 bucket.                  |
| 3    | Module 3: Variables, outputs, data sources | main/                 | The same S3 configuration becomes configurable; AWS data is queried rather than hardcoded. |
| 4    | Checkpoint 1: Nginx deployment             | main/                 | A frontend-only Nginx EC2 instance is added to the existing main lab.                      |
| 5    | Module 4: Terraform state                  | main/                 | The existing main lab state is migrated to an S3 backend.                                  |
| 6    | Module 5 and 5.5: Loops and expressions    | main/                 | The same project gains repeatable and conditional resources.                               |
| 7    | Module 6: Modules and DR copy              | main/ + modules/      | Reusable network and web-app modules are called more than once.                            |
| 8    | Checkpoint 2: DR-style deployment          | main/ + modules/      | Two matching Nginx deployments run in separate AZs in one region.                          |
| 9    | Module 7: Multi-environment microstates    | multi-env/            | A fresh lab uses separate root directories and remote state keys per layer/environment.    |
| 10   | Checkpoint 3 and capstone                  | multi-env/; capstone/ | A completed multi-environment setup, followed by an independent multi-tier design.         |

Table 1. The learning sequence and where each activity belongs.

## Lab conventions and safety rules

| Convention        | Required practice                                                                                                                                                |
|-------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| AWS access        | Use the approved sandbox account and the authentication method provided by the instructor. Never place access keys or secrets in .tf files, screenshots, or Git. |
| Region            | Use one approved AWS region throughout the bootcamp unless an activity explicitly says otherwise.                                                                |
| Default tags      | Apply Project, Environment, ManagedBy, and Owner tags to supported resources. Use the AWS provider default_tags block where appropriate.                         |
| Plan before apply | Run terraform plan and read every create, update, replacement, and destroy action before applying.                                                               |
| State handling    | Treat state files and saved plan files as sensitive. Do not commit them to source control.                                                                       |
| Cost control      | Use only the instance types and resource counts approved by the instructor. Destroy lab resources after evidence is captured.                                    |
| Public access     | Expose HTTP only where the Nginx exercise requires browser testing. Do not expose SSH or databases to the internet.                                              |
| Cleanup           | Follow the cleanup steps in each module. Do not destroy the remote state bucket as part of normal lab cleanup.                                                   |

## Repository structure to grow into

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th><p>terraform-bootcamp/</p>
<p>├── intro/ # Module 1: local-only practice</p>
<p>├── main/ # Modules 2-6: continuous AWS lab</p>
<p>│ ├── *.tf</p>
<p>│ └── modules/</p>
<p>│ ├── network/</p>
<p>│ └── web_app/</p>
<p>├── checkpoint-1/ # Evidence or README for the first checkpoint</p>
<p>├── checkpoint-2/ # Evidence or README for the DR checkpoint</p>
<p>├── multi-env/ # Module 7: fresh environment / microstate lab</p>
<p>│ ├── modules/</p>
<p>│ └── environments/</p>
<p>└── capstone/ # Independent capstone implementation</p></th>
</tr>
</thead>
<tbody>
</tbody>
</table>

Keep the Terraform root files inside the folder that owns the state. A folder that contains a separate backend configuration is a separate Terraform root module, even when it calls the same child modules.

**MODULE 1**

# The Terraform Cheat Sheet and IaC Foundations

*Start without cloud credentials. Learn the Terraform workflow locally before creating AWS resources.*

## Learning objectives

- Explain Infrastructure as Code and how it differs from manual console changes.

- Identify providers, resources, state, and modules at a high level.

- Run the core Terraform commands and read the plan before applying.

- Format code and keep credentials out of configuration files.

## Starting point

No AWS login is required. Work only in the intro/ folder. This module produces a small local artifact that proves the Terraform CLI workflow works.

## By the end of this module

A working local Terraform project, a basic understanding of the workflow, and a repeatable habit of formatting, validating, planning, applying, and destroying.

## Concepts to learn

| Concept                | What it means                                                                             | Your guide                                                                                                   |
|------------------------|-------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------|
| Infrastructure as Code | Infrastructure or supporting resources are described in version-controlled configuration. | The configuration is the intended state; Terraform compares it with recorded state and the real environment. |
| Provider               | A plugin that lets Terraform communicate with a platform or service.                      | The local provider is used in this module; the AWS provider is introduced in Module 2.                       |
| Resource               | An object Terraform creates and manages.                                                  | The local_file resource creates a text file on your computer.                                                |
| State                  | Terraform data that maps resource addresses in code to real objects.                      | Observe the local terraform.tfstate file after apply. Never commit it.                                       |
| Module                 | A collection of Terraform configuration managed together.                                 | The intro/ folder is a root module. Reusable child modules come later.                                       |
| Core workflow          | Initialize, plan, apply, and destroy.                                                     | Treat plan as a review step, not an optional formality.                                                      |

## Step-by-step lab: create a local text file

**1.** Create the project folder and move into it: mkdir -p terraform-bootcamp/intro && cd terraform-bootcamp/intro.

**2.** Create main.tf. Declare the Terraform and local provider requirements, then define one local_file resource with a filename such as ./hello.txt and a short content string.

**3.** Run terraform init. Confirm Terraform downloads the local provider and initializes the working directory.

**4.** Run terraform fmt, then terraform validate. Fix formatting or syntax errors before continuing.

**5.** Run terraform plan. Read the output and identify the resource Terraform intends to create. No file should be created by plan alone.

**6.** Run terraform apply and approve the plan. Confirm hello.txt exists and inspect its contents.

**7.** Change the content string in main.tf. Run fmt, validate, and plan again. Identify the proposed in-place update, then apply it.

**8.** Run terraform destroy, review the deletion plan, and approve it. Confirm the generated file is removed.

| **Expected result** You created, updated, and deleted a local file using Terraform without signing in to AWS. This gives you a safe place to practice the command sequence before cloud deployment. |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|

## Try it yourself

Repeat the workflow without copying commands blindly. Before each apply or destroy, explain what the plan is proposing. Add a README.md that records the commands you used and the expected result.

## Key concepts at a glance

| Tool / concept            | What it does                                                          | Your guide                                                                                     |
|---------------------------|-----------------------------------------------------------------------|------------------------------------------------------------------------------------------------|
| terraform init            | Initializes the working directory and installs the required provider. | Run first for a new root module, and again when provider/module/backend configuration changes. |
| terraform fmt             | Formats configuration to canonical Terraform style.                   | Run before committing or reviewing code.                                                       |
| terraform validate        | Checks whether configuration is syntactically and internally valid.   | Use after edits; it does not test cloud permissions or remote APIs.                            |
| terraform plan            | Previews proposed infrastructure changes.                             | Read create, update, replace, and destroy actions.                                             |
| terraform apply / destroy | Applies the reviewed configuration or removes managed resources.      | Approve only when the proposed actions match the lab objective.                                |

## Completion evidence

Submit the intro/ folder with main.tf and README.md. A terminal screenshot showing init, plan, apply, and destroy is optional instructor evidence. Do not submit terraform.tfstate or the .terraform directory.

## Cleanup / hand-off

The generated text file should be removed by terraform destroy. Keep the source code for reference. Add .terraform/ and \*.tfstate\* to .gitignore.

## Recommended references

- [<u>Terraform CLI commands</u>](https://developer.hashicorp.com/terraform/cli/commands) – Command overview and behavior.

- [<u>Terraform AWS Get Started</u>](https://developer.hashicorp.com/terraform/tutorials/aws-get-started) – Follow the learning path after local practice.

## End-of-module knowledge check \| 10 points

Answer each question in one or two sentences. Score 2 points per question.

> **1.** What is the difference between a provider and a resource?
>
> **2.** Which command initializes a working directory?
>
> **3.** Does terraform plan create the file or cloud resource?
>
> **4.** Why should terraform.tfstate not be committed to Git?
>
> **5.** What should you verify before approving terraform apply?

**MODULE 2**

# Environment Setup and First AWS Deployment

*Continue in main/. Configure AWS authentication safely and create the first managed AWS resource.*

## Learning objectives

- Connect Terraform to the approved AWS sandbox using the supported credential method.

- Understand the provider block and HCL file structure.

- Use provider default tags for resource traceability.

- Create and verify an S3 bucket using Terraform.

## Starting point

Module 1 is complete. Create a separate main/ root module. This is the project that Modules 2-6 will extend. The first AWS resource is an S3 bucket, not the Nginx server.

## By the end of this module

A working AWS root module with provider configuration, default tags, one private S3 bucket, and a successful apply/verification cycle.

| **Before starting** The instructor must provide the sandbox account, approved region, allowed resource types, and authentication instructions. Learners should not create IAM users or long-lived access keys for this activity. |
|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|

## Step-by-step lab: prepare the AWS environment

**1.** Install Terraform CLI and AWS CLI v2, if they are not already available. Confirm both commands run from the terminal.

**2.** Authenticate using the method approved for the sandbox. For an AWS IAM Identity Center profile, the instructor may direct you to run aws sso login --profile \<profile-name\>. For another approved method, follow the supplied instructions.

**3.** Select the correct profile when required, for example export AWS_PROFILE=\<profile-name\>. Do not put access keys in Terraform files.

**4.** Run aws sts get-caller-identity. Confirm the returned account is the training sandbox and not a personal or production account.

**5.** Create main/ and add versions.tf, providers.tf, main.tf, variables.tf, and outputs.tf. Empty files are acceptable initially; keep one clear responsibility per file.

**6.** In versions.tf, declare the required Terraform version range and the hashicorp/aws provider source. Because Module 4 uses S3 native lock files, use Terraform CLI 1.10 or later (or the newer version approved by the instructor). Pin an approved AWS provider version and commit .terraform.lock.hcl.

**7.** In providers.tf, configure the approved region and default_tags such as Project=TerraformBootcamp, Environment=dev, ManagedBy=Terraform, and Owner=\<learner-id\>.

**8.** In main.tf, create one S3 bucket using a unique bucket name or bucket_prefix. Enable S3 Block Public Access and bucket versioning. Do not configure public website hosting for this lab.

**9.** Run terraform init, terraform fmt, terraform validate, and terraform plan. Check the bucket name, region, tags, versioning, and public-access settings in the plan.

**10.** Apply only after the plan matches the intended result. Verify the bucket in the AWS console or with the AWS CLI.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th><p># Example provider pattern</p>
<p>provider "aws" {</p>
<p>region = var.aws_region</p>
<p>default_tags {</p>
<p>tags = {</p>
<p>Project = "TerraformBootcamp"</p>
<p>Environment = "dev"</p>
<p>ManagedBy = "Terraform"</p>
<p>Owner = var.owner</p>
<p>}</p>
<p>}</p>
<p>}</p></th>
</tr>
</thead>
<tbody>
</tbody>
</table>

This is valid provider-block syntax. Declare aws_region and owner as input variables in the same root module.

## Try it yourself

In the main/ root module, create the S3 bucket and verify that it is private and versioned. Change one non-sensitive tag and run plan again. Confirm the plan shows only the expected update before applying.

## Key concepts at a glance

| Tool / concept         | What it does                                                                           | Your guide                                                                        |
|------------------------|----------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------|
| AWS provider           | Terraform plugin that authenticates to AWS and exposes AWS resource/data-source types. | Keep the provider configuration separate from resource definitions.               |
| HCL files              | Terraform configuration is commonly split across .tf files in one root directory.      | Terraform loads all .tf files in the root module; filenames are for organization. |
| default_tags           | Applies standard tags to supported AWS resources managed by the provider.              | Use consistent project, environment, management, and owner tags.                  |
| S3 Block Public Access | Protects the bucket from unintended public access settings.                            | Enable for this lab; do not make the bucket public.                               |
| Versioning             | Retains object versions in the bucket.                                                 | Enable for the lab bucket and later for the state bucket.                         |

## Completion evidence

Submit the main/ source files, .terraform.lock.hcl, a screenshot of the reviewed apply, and evidence that the bucket exists with public access blocked and versioning enabled. Do not submit credentials or state files.

## Cleanup / hand-off

Keep the bucket for the next modules because the main lab will continue to evolve. Destroy it only if the instructor asks for a reset. The state file is local at this point; Module 4 will migrate it to the remote backend.

## Recommended references

- [<u>Terraform AWS Get Started</u>](https://developer.hashicorp.com/terraform/tutorials/aws-get-started) – Installation, authentication, provider, and first deployment.

- [<u>Configure Terraform providers</u>](https://developer.hashicorp.com/terraform/tutorials/configuration-language/configure-providers) – Provider setup and default tags.

- [<u>AWS S3 user guide</u>](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html) – S3 bucket configuration and security.

## End-of-module knowledge check \| 10 points

Answer each question in one or two sentences. Score 2 points per question.

> **1.** Why should credentials stay outside .tf files?
>
> **2.** What does the AWS provider block configure?
>
> **3.** Why are default tags useful in a shared sandbox?
>
> **4.** What is the purpose of S3 Block Public Access?
>
> **5.** Why is bucket versioning useful for Terraform state storage?

**MODULE 3**

# HCL Deep Dive: Variables, Outputs, and Data Sources

*Make the same main lab configurable and discover existing AWS information instead of hardcoding it.*

## Learning objectives

- Parameterize values using input variables.

- Expose useful deployment information with output values.

- Read existing AWS infrastructure using data sources.

- Understand the difference between resources and data sources.

## Starting point

Continue with the main/ directory and the S3 bucket from Module 2. Do not create a new root module or reset the bucket.

## By the end of this module

A parameterized main lab with input variables, meaningful outputs, and data sources that discover approved AWS information for the Nginx checkpoint.

## How the pieces connect

| HCL feature    | Question it answers                                      | Example in this lab                                                            |
|----------------|----------------------------------------------------------|--------------------------------------------------------------------------------|
| Input variable | What value may the learner or environment supply?        | AWS region, owner, environment, instance type, name prefix.                    |
| Output value   | What useful result should Terraform display after apply? | Bucket name, bucket ARN, Nginx public URL, VPC ID.                             |
| Data source    | What existing information can Terraform read?            | Current AWS account ID, selected VPC/subnet, latest approved Amazon Linux AMI. |
| Resource       | What should Terraform create or manage?                  | S3 bucket, security group, EC2 instance.                                       |

## Step-by-step lab: parameterize the existing bucket

**1.** Add variables for aws_region, environment, owner, and bucket_prefix. Give each variable a description and an appropriate type.

**2.** Replace hardcoded region, environment, owner, and naming values in the provider/resource configuration with variable references.

**3.** Create a terraform.tfvars file for non-secret lab values, or use the instructor-provided variable method. Do not put secrets in tfvars.

**4.** Add outputs for the S3 bucket name and ARN. Give each output a description.

**5.** Run terraform fmt, terraform validate, and terraform plan. The resource should remain the same bucket; do not accidentally rename it.

**6.** Apply if the plan is limited to intended changes, then run terraform output to inspect the values.

## Step-by-step lab: query AWS data

**1.** Add a data source for the current AWS caller identity. Output the account ID so you can confirm which account Terraform is using.

**2.** Use the approved sandbox network information supplied by the instructor. If the default VPC is permitted, query it with an AWS VPC data source. Otherwise, use the provided VPC ID as an input variable and look up the required subnet.

**3.** Add an AMI data source for the latest approved Amazon Linux image using filters and an explicit owner. Do not hardcode an AMI ID that may become unavailable.

**4.** Reference the data-source attributes from resource configuration. Remember: data sources read information; they do not create or modify the object being queried.

**5.** Run plan and identify which values come from variables, which come from data sources, and which are computed from managed resources.

| **Important checkpoint preparation** The Nginx checkpoint uses an existing approved VPC/subnet and a dynamically queried AMI. If the sandbox does not have a usable default VPC, the instructor must provide the VPC and public subnet identifiers or a network profile for the lab. |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|

## Checkpoint Activity 1: Frontend-only Nginx application

This checkpoint is the first end-to-end AWS deployment. Add a single EC2 instance running Nginx to the existing main/ root module. Keep the S3 bucket from Module 2 in the same state.

## Checkpoint 1 build steps

**1.** Add an EC2 security group in the approved VPC. Allow inbound TCP port 80 only for browser testing. Do not add public SSH access.

**2.** Use the AMI data source from Module 3 and the approved public subnet lookup/input. Confirm the subnet has a route to an Internet Gateway and that the instance receives a public IPv4 address for browser testing. Use the sandbox-approved small instance type.

**3.** Create one EC2 instance and pass a short user_data script that installs and starts Nginx on the selected Amazon Linux image.

**4.** Add outputs for the instance ID, public IP, and public URL. The public URL should use HTTP for this temporary training exercise only.

**5.** Run fmt, validate, and plan. Confirm the plan includes one EC2 instance and its security group, and no unintended changes to the S3 bucket.

**6.** Apply the plan. Open the output URL in a browser and confirm the Nginx welcome page is displayed.

**7.** Capture evidence, then follow the instructor’s cleanup instruction. Do not leave the EC2 instance running after the checkpoint if the next session is not immediate.

| Submission item      | What the reviewer should see                                                                   |
|----------------------|------------------------------------------------------------------------------------------------|
| Repository           | main/ Terraform configuration and a short README describing the resources.                     |
| Apply evidence       | Terminal screenshot showing a successful terraform apply without exposing account credentials. |
| Application evidence | Browser screenshot showing the Nginx page and the URL used for the test.                       |
| Cleanup evidence     | If requested, screenshot or terminal output showing the EC2 resources were destroyed.          |

## Try it yourself

Complete Checkpoint 1 using the existing main lab. Explain how the AMI and network values were discovered or supplied, and identify which values are outputs. After submission, retain the configuration and state for Module 4.

## Key concepts at a glance

| Tool / concept | What it does                                                                     | Your guide                                                                 |
|----------------|----------------------------------------------------------------------------------|----------------------------------------------------------------------------|
| variable       | Accepts a value supplied at runtime or from a tfvars file.                       | Declare a type and description. Avoid storing secrets in plain text files. |
| output         | Exposes a value from the root module after operations.                           | Use outputs for bucket identifiers and application endpoints.              |
| data source    | Reads an existing object or provider information without managing its lifecycle. | Use for account identity, approved network details, and image discovery.   |
| resource       | Creates or manages an object.                                                    | Use for the bucket, security group, and EC2 instance.                      |
| user_data      | Passes startup instructions to a new EC2 instance.                               | Use only for the small Nginx bootstrap required by this checkpoint.        |

## Completion evidence

Checkpoint 1 submission is the repository, successful apply screenshot, and deployed Nginx screenshot. The instructor may also request a short explanation of the security group and user_data script.

## Cleanup / hand-off

Because the main lab continues, keep the S3 bucket. Stop or destroy the EC2 instance based on instructor direction to avoid unnecessary runtime cost. Do not delete the local Terraform state before Module 4.

## Recommended references

- [<u>Input variables</u>](https://developer.hashicorp.com/terraform/language/values/variables) – Variable declarations and input values.

- [<u>Output values</u>](https://developer.hashicorp.com/terraform/language/values/outputs) – Output declarations and sensitive outputs.

- [<u>Data sources</u>](https://developer.hashicorp.com/terraform/language/data-sources) – Read existing infrastructure without managing it.

- [<u>AWS create infrastructure tutorial</u>](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/aws-create) – AWS authentication and a first EC2 resource.

## End-of-module knowledge check \| 10 points

Answer each question in one or two sentences. Score 2 points per question.

> **1.** What is the difference between an input variable and an output?
>
> **2.** When should you use a data source instead of a resource?
>
> **3.** Why is it better to query an approved AMI than hardcode an AMI ID?
>
> **4.** Which inbound rule is required for browser-based Nginx testing?
>
> **5.** What does user_data do when an EC2 instance starts?

**MODULE 4**

# Mastering Terraform State

*Protect the state for the existing main lab and migrate it from local storage to a remote S3 backend.*

## Learning objectives

- Explain how Terraform state maps configuration addresses to real AWS resources.

- Recognize the risks of local state, state drift, and concurrent operations.

- Configure an S3 backend with versioning and native S3 state locking.

- Migrate the existing main lab state without recreating resources.

## Starting point

Checkpoint 1 is complete. The main/ root module has already created AWS resources and currently uses local state. Do not start a fresh configuration for this module.

## By the end of this module

The same main lab now uses a remote S3 backend. A plan immediately after migration should show no unexpected resource changes.

| **Current backend guidance** Use the S3 backend with use_lockfile = true for native state locking. DynamoDB-based locking appears in older material but is deprecated in current HashiCorp documentation. If a company environment still uses DynamoDB, follow its approved migration plan rather than copying a legacy setup into this bootcamp. |
|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|

## Step-by-step lab: prepare the backend bucket

**1.** Confirm the instructor has provided a dedicated state bucket, or create it from a separate bootstrap folder/configuration. Do not try to create the backend bucket from the same root module that will use it as its backend.

**2.** Enable S3 bucket versioning, block public access, and encryption according to the sandbox standard. Restrict access to the training participants who need it.

**3.** Choose a unique state key such as terraform-bootcamp/main/terraform.tfstate. The key must be different from the keys used by other roots/environments.

**4.** Confirm the AWS identity used by Terraform has the required S3 permissions for the state object and its lock file.

## Step-by-step lab: migrate the existing main state

**1.** Confirm no other learner or process is running terraform apply against this state. Finish any in-progress operation before migrating.

**2.** Back up the existing local terraform.tfstate to a secure location outside the Git repository. Do not upload it to a public location or include it in screenshots.

**3.** Add a backend configuration to the Terraform block in main/. Use backend "s3" with the approved bucket, unique key, region, encryption, and use_lockfile = true. Backend configuration does not accept normal input variable references.

**4.** Run terraform init -migrate-state and follow the prompt to copy the existing state to the S3 backend. Do not choose a fresh/empty state unless instructed.

**5.** Confirm initialization completed and verify the state object exists in the approved bucket. Do not download or expose the state contents unnecessarily.

**6.** Run terraform state list to confirm the previously managed resources are still tracked.

**7.** Run terraform plan. The expected result is no infrastructure changes, unless you intentionally changed configuration during migration.

**8.** Have the instructor verify the remote key and locking configuration before continuing.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th><p>terraform {</p>
<p>backend "s3" {</p>
<p>bucket = "&lt;instructor-provided-state-bucket&gt;"</p>
<p>key = "terraform-bootcamp/main/terraform.tfstate"</p>
<p>region = "&lt;approved-region&gt;"</p>
<p>encrypt = true</p>
<p>use_lockfile = true</p>
<p>}</p>
<p>}</p></th>
</tr>
</thead>
<tbody>
</tbody>
</table>

Use literal backend settings or a supported partial backend configuration. Keep authentication outside the code. The bucket must exist before the backend is initialized.

## Try it yourself

Migrate the active main lab state. After migration, run terraform state list and terraform plan. Ask a partner to confirm that both of you can identify the same remote backend key without sharing credentials or state-file contents.

## Key concepts at a glance

| Tool / concept    | What it does                                                                                      | Your guide                                                                                  |
|-------------------|---------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------|
| State file        | Maps Terraform resource addresses to real infrastructure and stores metadata needed for planning. | Treat the file as sensitive; it may contain resource attributes and secrets.                |
| State drift       | Difference between recorded state/configuration and what exists in the provider.                  | Use plan to detect changes; agree on whether the difference is intentional before applying. |
| S3 backend        | Stores state remotely in an S3 object.                                                            | Use a unique key for each independent root module.                                          |
| S3 lock file      | Prevents concurrent state operations when native S3 locking is enabled.                           | Set use_lockfile = true and grant permissions on the .tflock object.                        |
| Bucket versioning | Retains prior versions of the state object.                                                       | Enable it to support recovery from accidental deletion or overwrite.                        |

## Completion evidence

Submit the backend configuration with bucket names redacted if required, screenshot of successful terraform init -migrate-state, terraform state list output, and a no-change plan. Do not submit the state file itself.

## Cleanup / hand-off

Keep the state bucket and remote state object for Modules 5-6. Destroy application resources only when instructed. The backend bucket is a protected shared dependency and should not be included in routine lab destroy commands.

## Recommended references

- [<u>Terraform state purpose</u>](https://developer.hashicorp.com/terraform/language/state/purpose) – Why Terraform needs state.

- [<u>S3 backend</u>](https://developer.hashicorp.com/terraform/language/backend/s3) – S3 configuration, versioning recommendation, native locking, and deprecated DynamoDB locking.

- [<u>Remote state</u>](https://developer.hashicorp.com/terraform/language/state/remote) – Why teams use remote state.

## End-of-module knowledge check \| 10 points

Answer each question in one or two sentences. Score 2 points per question.

> **1.** What information does Terraform state map?
>
> **2.** Why is local state difficult when multiple engineers work on the same infrastructure?
>
> **3.** Why should the backend bucket exist before terraform init?
>
> **4.** What does use_lockfile = true enable?
>
> **5.** What should a no-change plan after migration tell you?

**MODULE 5**

# Meta-Arguments and Resource Looping

*Scale the main lab without duplicating resource blocks.*

## Learning objectives

- Apply the DRY principle to repeated infrastructure.

- Use count for simple numeric repetition and for_each for keyed collections.

- Understand depends_on and lifecycle settings.

- Review how changes to collection keys affect resource addresses.

## Starting point

Continue in main/. The state is now remote in S3. Keep the existing bucket and any active resources already managed by this root module.

## By the end of this module

The main project contains a small set of repeatable support resources defined through loops, with predictable names and reviewed changes.

| Meta-argument | Purpose                                                   | Use it when                                                                           |
|---------------|-----------------------------------------------------------|---------------------------------------------------------------------------------------|
| count         | Creates a numeric number of instances, indexed from zero. | You need a simple fixed number of similar instances or an optional single resource.   |
| for_each      | Creates one instance for each key in a map or set.        | Each instance needs a stable, meaningful key such as logs, backups, or audit.         |
| depends_on    | Adds an explicit dependency edge.                         | Terraform cannot infer a real dependency from an attribute reference.                 |
| lifecycle     | Controls selected resource lifecycle behavior.            | You need a deliberate replacement or deletion safeguard and understand the trade-off. |

## Step-by-step lab: create repeatable S3 support buckets

**1.** Add a variable containing a small set/map of support bucket purposes, for example logs and artifacts. Keep the list short to limit resource creation.

**2.** Define one aws_s3_bucket resource with for_each over the supplied set/map. Build each bucket name from the unique learner/project prefix plus each.key.

**3.** Add a matching Block Public Access resource and versioning configuration for each bucket, using the same each.key so related objects stay aligned.

**4.** Run fmt, validate, and plan. Read the addresses and confirm Terraform proposes one instance per key.

**5.** Apply, then run terraform state list. Identify the indexed/keyed addresses for each instance.

**6.** Remove one key from the input collection and run plan. Identify which specific instance Terraform proposes to destroy. Restore the key if you do not want to remove it permanently.

**7.** Create a separate optional resource using count = var.enable_archive ? 1 : 0. Toggle the variable and compare the plan before applying.

## Step-by-step lab: understand dependencies and lifecycle

**1.** Inspect the resource references in your configuration. Note which dependencies Terraform can infer automatically because one resource references another resource attribute.

**2.** Run terraform graph and inspect the dependency graph. You do not need to understand every graph node; identify the relationship between the bucket and its associated configuration resources.

**3.** Read the depends_on documentation. Add it only if there is a genuine dependency Terraform cannot infer from references. Do not add depends_on to every resource.

**4.** Temporarily add prevent_destroy = true to a disposable lab bucket and run terraform plan -destroy. Observe the safeguard, then remove the setting before final cleanup.

**5.** Discuss create_before_destroy and ignore_changes. Do not add them blindly; explain what operational problem each is intended to solve.

| **Common mistake** Removing a key from a for_each map is not just a code cleanup. Terraform interprets it as removing that resource instance from the desired configuration and will generally plan to destroy it. Always inspect the plan before applying. |
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|

## Debug exercise: invalid for_each collection

Starting point: use the support-bucket exercise in main/. The variable support_bucket_names is declared as a list of strings.

## Reproduce and fix

**1.** Temporarily set for_each = var.support_bucket_names on the support-bucket resource while the variable remains a list.

**2.** Run terraform plan and read the error. Do not apply.

**3.** Change the expression to for_each = toset(var.support_bucket_names), or change the input to a map with stable keys.

**4.** Run fmt, validate, and plan again. Explain why the corrected collection is acceptable for for_each.

**5.** Restore the intended support-bucket set and confirm the plan does not propose unrelated changes.

Cleanup: no infrastructure should have been created by the broken configuration. If you applied the corrected test resources, remove only those test resources through a reviewed plan.

## Try it yourself

Use for_each for a keyed group of support buckets and count for one optional resource. Toggle one key and one boolean, then use plan to predict the exact resources that will be created or destroyed.

## Key concepts at a glance

| Tool / concept            | What it does                                                                                  | Your guide                                                                          |
|---------------------------|-----------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------|
| count                     | Creates zero or more resource instances using numeric indexes.                                | Good for an optional one-off resource or a simple quantity.                         |
| for_each                  | Creates instances from stable keys in a set or map.                                           | Prefer it when each instance has a name or identity.                                |
| depends_on                | Explicitly orders operations where implicit references are insufficient.                      | Use sparingly; normal references already create dependencies.                       |
| lifecycle.prevent_destroy | Blocks plans that would destroy a protected resource while the rule remains in configuration. | Use as a guardrail only when the protected resource should not be casually removed. |
| terraform graph           | Renders the dependency graph represented by the configuration.                                | Use it to investigate ordering and dependencies.                                    |

## Completion evidence

Submit the updated main/ files, a plan screenshot showing keyed instances, and a short README explaining why you selected count or for_each for each repeated resource.

## Cleanup / hand-off

Keep the support resources if the instructor wants to continue with them. Otherwise destroy only the resources created in this module. Do not remove the remote backend or shared state bucket.

## Recommended references

- [<u>Meta-arguments</u>](https://developer.hashicorp.com/terraform/language/meta-arguments) – count, for_each, depends_on, and lifecycle overview.

- [<u>Create and manage resources</u>](https://developer.hashicorp.com/terraform/language/resources) – How Terraform plans and manages resources.

- [<u>Terraform CLI commands</u>](https://developer.hashicorp.com/terraform/cli/commands) – Includes graph and state commands.

## End-of-module knowledge check \| 10 points

Answer each question in one or two sentences. Score 2 points per question.

> **1.** When is for_each usually clearer than count?
>
> **2.** What can happen when a key is removed from a for_each map?
>
> **3.** What is the purpose of depends_on?
>
> **4.** What does prevent_destroy protect against?
>
> **5.** Why should a learner inspect the dependency graph and plan before applying?

**MODULE 5.5**

# Conditionals, Expressions, and Functions

*Add decision-making and data transformation to the same Terraform configuration.*

## Learning objectives

- Use conditional expressions to control whether a resource is created or how an argument is set.

- Use for expressions to transform and filter lists/maps.

- Use built-in functions such as length, merge, lookup, and element.

- Use local values to make repeated expressions easier to read.

## Starting point

Continue in main/. The prior module introduced repeated S3 support resources. This module makes their configuration more flexible.

## By the end of this module

A configuration that computes names/tags, conditionally creates an optional resource, and exposes a useful map of repeated resource outputs.

| Expression / function | What it does                                                         | Practical example                                                         |
|-----------------------|----------------------------------------------------------------------|---------------------------------------------------------------------------|
| Conditional           | Chooses one value when a condition is true and another when false.   | Use a smaller instance type in dev and a larger approved type in staging. |
| For expression        | Transforms or filters a collection into a new list, set, or map.     | Build a map of support bucket names to bucket IDs.                        |
| length                | Returns the number of characters or elements in a value.             | Check whether a collection contains any entries.                          |
| merge                 | Combines maps/objects; later values override earlier duplicate keys. | Combine common tags with environment-specific tags.                       |
| lookup                | Reads a map value by key and can return a default when missing.      | Select a region-specific or environment-specific setting.                 |
| element               | Returns an item at a selected index in a list/tuple.                 | Understand indexed selection; use normal indexing when clearer and safe.  |

## Step-by-step lab: add locals and tag composition

**1.** Create a locals block for common tags, including Project, Environment, ManagedBy, and Owner.

**2.** Create another local map for environment-specific tags or settings. Use merge to combine maps where needed.

**3.** Reference the resulting local map from resources that support tags. Confirm provider default tags and resource-specific tags do not conflict unexpectedly.

**4.** Run terraform console and experiment with length, merge, lookup, and element using small sample values. Exit the console when finished.

## Step-by-step lab: conditional resource and output transformation

**1.** Add an enable_archive input variable with a boolean type and a safe default of false.

**2.** Use a conditional expression with count to create the optional archive bucket only when enable_archive is true.

**3.** Use a for expression to create an output map that lists each support bucket key and its bucket name.

**4.** Use lookup to retrieve an environment setting with a default value. Use length to check whether a supplied list/map is empty before relying on it.

**5.** Run plan with enable_archive=false, then change it to true and compare the plan. Apply only the configuration required by the instructor.

**6.** Restore the approved final variable value and verify terraform plan shows the expected result.

| **Expression readability rule** Use expressions to make intent clearer, not to compress everything into one line. If a learner or reviewer needs several minutes to decode a nested expression, introduce a local value or split the logic into a simpler expression. |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|

## Try it yourself

Create common and environment-specific tag maps, conditionally create the optional archive bucket, and expose the repeated support buckets through a for-expression output. Use terraform console to test functions before placing them in resource arguments.

## Key concepts at a glance

| Tool / concept         | What it does                                            | Your guide                                                       |
|------------------------|---------------------------------------------------------|------------------------------------------------------------------|
| locals                 | Names an expression/value for reuse within a module.    | Use for common tags, computed names, and repeated configuration. |
| conditional expression | Returns one of two values based on a boolean condition. | Use for optional resource counts or environment-specific values. |
| for expression         | Transforms or filters a collection.                     | Use to generate a map/list of outputs from repeated resources.   |
| merge()                | Combines maps or objects.                               | Put baseline tags first and intended overrides later.            |
| lookup()               | Finds a map key with an optional default.               | Provide a default so missing keys are handled intentionally.     |
| terraform console      | Evaluates Terraform expressions interactively.          | Test function behavior before committing expressions.            |

## Completion evidence

Submit the updated HCL plus a short before/after plan showing the conditional resource toggling. Include the output map in your README or terminal evidence.

## Cleanup / hand-off

Return the optional resource to the instructor-approved setting. If it was created, destroy it when asked. Keep the main state backend and the persistent resources required for Module 6.

## Recommended references

- [<u>Terraform expressions</u>](https://developer.hashicorp.com/terraform/language/expressions) – Conditionals, for expressions, functions, and related syntax.

- [<u>Built-in functions</u>](https://developer.hashicorp.com/terraform/language/functions) – Function reference and console examples.

- [<u>Named values</u>](https://developer.hashicorp.com/terraform/language/expressions/references) – Variables, locals, data sources, resources, and outputs.

## End-of-module knowledge check \| 10 points

Answer each question in one or two sentences. Score 2 points per question.

> **1.** What problem does a conditional expression solve?
>
> **2.** What does a for expression return?
>
> **3.** How does merge handle duplicate map keys?
>
> **4.** Why should lookup use a default for optional keys?
>
> **5.** When is a local value preferable to repeating a long expression?

**MODULE 6**

# Terraform Modules and Code Reusability

*Turn the Nginx pattern into reusable components and deploy a second matching copy.*

## Learning objectives

- Explain root modules and child modules.

- Build a reusable network module using input variables and outputs.

- Build a reusable web-app module for a simple Nginx instance.

- Call the same module more than once to create a DR-style second deployment.

- Review a Terraform Registry module before using it.

## Starting point

Modules 2-5.5 are complete. Capture the Checkpoint 1 evidence before changing the Nginx resources. This module remains in the main lab, but replaces the initial one-off Nginx deployment with a modular version.

## By the end of this module

The main lab calls a network module and two instances of a web-app module. Both Nginx endpoints work in separate Availability Zones in the same AWS region.

| **Scope clarification** The second deployment is a DR-style learning exercise, not a production disaster-recovery design. It uses two Availability Zones in one region and does not include automated failover, replicated data, or a recovery-time objective. |
|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|

## Step-by-step lab: create a reusable network module

**1.** After Checkpoint 1 evidence is captured, remove the original one-off Nginx EC2 instance and its security group from the main configuration. Apply the reviewed destroy for those resources only. Keep the S3 lab bucket and remote backend.

**2.** Create modules/network/ with main.tf, variables.tf, outputs.tf, and README.md.

**3.** In the network module, define inputs for VPC CIDR, two public subnet CIDRs, two Availability Zones, and a name prefix.

**4.** Create one VPC, an Internet Gateway, two public subnets in separate Availability Zones, a public route table, and subnet associations. Use tags consistently.

**5.** Output the VPC ID and the list of public subnet IDs. Add descriptions to all variables and outputs.

**6.** Do not hardcode a specific AZ name if the instructor expects portability. Pass the two approved AZs from the root module.

## Step-by-step lab: create the reusable web-app module

**1.** Create modules/web_app/ with main.tf, variables.tf, outputs.tf, and README.md.

**2.** Define inputs for name, VPC ID, subnet ID, AMI ID, and approved instance type.

**3.** Create a security group that allows HTTP port 80 for the sandbox test. Do not open SSH to the internet.

**4.** Create one EC2 instance using the supplied subnet and AMI, and install/start Nginx using user_data.

**5.** Output the instance ID, public IP, and public URL. Add descriptions to all inputs and outputs.

**6.** Keep the module focused on the web-app concept. Do not bury networking resources inside this module.

## Step-by-step lab: call the modules from the root module

**1.** In main/, call module.network once and pass the approved VPC/subnet inputs.

**2.** Call module.web_app twice. Give the first call a primary name and the first public subnet ID. Give the second call a DR name and the second public subnet ID.

**3.** Pass the same VPC ID and approved AMI to both calls. Keep the instance size small and consistent with the sandbox policy.

**4.** Expose the two web URLs as root outputs so the reviewer can test both deployments.

**5.** Run terraform init because local module sources have been added, then run fmt, validate, and plan.

**6.** Review the plan for one VPC, two public subnets, two web security groups, and two EC2 instances, plus the associated network resources. Confirm the old one-off instance is not unexpectedly recreated.

**7.** Apply the plan. Open both URLs and confirm both Nginx pages load.

**8.** Read the Terraform Registry page for a comparable AWS VPC module. Check its source, version, inputs, outputs, examples, and maintenance status before using a third-party module in real work.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th><p># Root module pattern: one network module, two calls to one web-app module</p>
<p>module "network" {</p>
<p>source = "./modules/network"</p>
<p># Pass the approved CIDRs, AZs, and name prefix.</p>
<p>}</p>
<p>module "web_primary" {</p>
<p>source = "./modules/web_app"</p>
<p># Pass module.network outputs for the first subnet.</p>
<p>}</p>
<p>module "web_dr" {</p>
<p>source = "./modules/web_app"</p>
<p># Pass module.network outputs for the second subnet.</p>
<p>}</p></th>
</tr>
</thead>
<tbody>
</tbody>
</table>

The snippet shows the composition pattern only. Complete the required input arguments based on the variables declared by your modules.

## Try it yourself

Build both modules, call the network module once, and call the web-app module twice. The same module source should create both Nginx deployments; only the input values should differ.

## Key concepts at a glance

| Tool / concept     | What it does                                                          | Your guide                                                                                 |
|--------------------|-----------------------------------------------------------------------|--------------------------------------------------------------------------------------------|
| Root module        | The configuration in the current working directory.                   | main/ is the root module that calls child modules.                                         |
| Child module       | A directory of Terraform configuration called through a module block. | Keep modules focused on a reusable infrastructure concept.                                 |
| Input variables    | Allow the calling root module to supply values.                       | Describe each input and choose an appropriate type.                                        |
| Outputs            | Return selected child-module values to the caller.                    | Expose only what the caller needs, such as VPC ID and subnet IDs.                          |
| Terraform Registry | A directory of reusable Terraform modules and providers.              | Review source, version, variables, outputs, examples, and security/maintenance before use. |

## Completion evidence

Submit the repository, module README files, a screenshot of the reviewed apply, and browser screenshots proving both Nginx URLs are reachable. Include a simple architecture diagram showing one VPC, two public subnets in separate AZs, and two web instances.

## Cleanup / hand-off

After evidence is accepted, run terraform destroy from the main/ root module and verify the network and both web deployments are removed. Do not destroy the remote S3 state bucket. Keep the reusable module source for Module 7.

## Recommended references

- [<u>Modules overview</u>](https://developer.hashicorp.com/terraform/language/modules) – How root and child modules work.

- [<u>Creating modules</u>](https://developer.hashicorp.com/terraform/language/modules/develop) – How to design a reusable module.

- [<u>Standard module structure</u>](https://developer.hashicorp.com/terraform/language/modules/develop/structure) – Recommended main.tf, variables.tf, outputs.tf, and README layout.

- [<u>Terraform Registry</u>](https://registry.terraform.io/) – Search and review reusable modules.

## End-of-module knowledge check \| 10 points

Answer each question in one or two sentences. Score 2 points per question.

> **1.** What is the difference between a root module and a child module?
>
> **2.** Why should module inputs and outputs have descriptions?
>
> **3.** Why should the same web-app module be called twice instead of copying its resource blocks?
>
> **4.** What should you review before adopting a public Registry module?
>
> **5.** Why is the second Nginx deployment not a complete production DR solution?

**MODULE 7**

# Multi-Environment Setups, Directory Segregation, and Microstates

*Start a fresh lab root. Reuse the modules, but separate environments and infrastructure layers into independent states.*

## Learning objectives

- Separate development and staging configuration directories.

- Separate network and application components into independent root modules and state files.

- Share required network outputs safely between state boundaries.

- Promote the same shared module code with environment-specific variable files.

- Understand why directory segregation and microstates can reduce blast radius and team contention.

## Starting point

Module 6 is complete and its main lab resources have been destroyed. Reuse the modules/network and modules/web_app source, but create a new multi-env/ directory from scratch. The independent roots in this module must each have their own backend key.

## By the end of this module

A multi-environment repository with separate network and app roots per environment, unique remote state keys, and a documented deployment sequence. At minimum, deploy dev; staging can be applied only if sandbox budget and instructor approval allow it.

| **Why this is a fresh lab** A microstate is an independently managed state boundary. Module 7 intentionally starts a new root instead of modifying the single main state. The network and application layers can then be planned, applied, and operated independently. |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|

## Step-by-step lab: create the directory structure

**1.** Create multi-env/ and copy or reference the network and web_app modules built in Module 6. Prefer a shared module source rather than manually duplicating module code.

**2.** Create environments/dev/network, environments/dev/app, environments/staging/network, and environments/staging/app.

**3.** Treat each of these four directories as a separate root module. Each root must contain its own Terraform configuration and backend key.

**4.** Use a clear file layout in each root: versions.tf, backend.tf, providers.tf, main.tf, variables.tf, outputs.tf, and an environment-specific tfvars file where useful.

**5.** Assign unique S3 backend keys. For example: terraform-bootcamp/multi-env/dev/network.tfstate, dev/app.tfstate, staging/network.tfstate, and staging/app.tfstate.

**6.** Do not point two roots at the same backend key. Do not use the same state file for network and application layers.

| Root directory               | Owns                                                     | Example backend key                                  |
|------------------------------|----------------------------------------------------------|------------------------------------------------------|
| environments/dev/network     | Development VPC, subnets, routes, and network outputs.   | terraform-bootcamp/multi-env/dev/network.tfstate     |
| environments/dev/app         | Development web application and related security groups. | terraform-bootcamp/multi-env/dev/app.tfstate         |
| environments/staging/network | Staging network layer.                                   | terraform-bootcamp/multi-env/staging/network.tfstate |
| environments/staging/app     | Staging application layer.                               | terraform-bootcamp/multi-env/staging/app.tfstate     |

Table 2. Each row is a separate Terraform root module and must initialize against its own state key.

## Step-by-step lab: deploy the network state first

**1.** Open environments/dev/network. Configure its S3 backend using the approved shared state bucket and the dev/network key.

**2.** Call the network module and pass the dev CIDRs, AZs, and naming values.

**3.** Run terraform init, fmt, validate, and plan. Review the network-only resources and apply after approval.

**4.** Expose the VPC and subnet information through documented outputs.

**5.** Choose the instructor-approved inter-state communication method: Parameter Store for selected values, or terraform_remote_state for a controlled training example.

**6.** If using SSM Parameter Store, publish only the specific network values the app root needs, such as VPC ID and subnet IDs. Scope IAM access to the relevant parameters.

**7.** If using terraform_remote_state, understand that read access to the state object can reveal the full state contents, not just the outputs. Restrict access accordingly.

## Step-by-step lab: deploy the application state

**1.** Open environments/dev/app. Configure a different S3 backend key for dev/app.

**2.** Read the network values through the chosen sharing method. Do not copy/paste IDs from a terminal into resource arguments unless the activity specifically asks you to test a manual input.

**3.** Call the web_app module and pass the required VPC, subnet, AMI, and instance type values.

**4.** Run terraform init, fmt, validate, and plan. Confirm the app plan does not propose changes to the network state.

**5.** Apply after approval and verify the Nginx endpoint.

**6.** Create the matching staging roots using the same module source but different tfvars and backend keys. Run plan for staging. Apply staging only if the instructor approves the additional resources and cost.

## Step-by-step lab: promote code and compare environments

**1.** Keep module code the same for dev and staging. Change environment values in the environment-specific tfvars files.

**2.** Compare the two plans. Identify which settings differ and which resources remain structurally identical.

**3.** Update a shared module version or source reference in a controlled change, then plan dev and staging separately.

**4.** Document the deployment order: network first, app second. Document the teardown order: app first, network second.

**5.** Do not add a production environment to the live lab unless the instructor provides an approved production-like sandbox. Production can be represented as a directory and a plan-only exercise.

| **State-sharing decision** Use SSM Parameter Store when the application needs only a few explicit network values. Use terraform_remote_state only when the team accepts the broader state-read access model and has appropriate access controls. |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|

## Debug exercise: app root cannot find network outputs

Starting point: use the dev app root after the dev network root has been applied. The app root reads VPC/subnet values from the selected inter-state sharing method.

## Reproduce and fix

**1.** Temporarily change the SSM parameter path or remote-state key in the dev app root so that it points to a non-existent network value. Do not apply.

**2.** Run terraform plan and inspect the data-source error or missing value.

**3.** Compare the configured path/key with the dev network root output or published parameter name.

**4.** Correct the path/key and confirm that the app root reads the intended VPC and subnet values.

**5.** Run plan again and verify that the app root proposes only the expected application resources.

Cleanup: restore the correct path/key in source control. No resources should be created or destroyed by this debugging exercise.

## Try it yourself

Complete the dev network root, then the dev app root. Build the staging network/app directory structure with unique backend keys and run plans. Apply staging only when authorized. Demonstrate that each root has its own state and that the app reads network information across the boundary.

## Key concepts at a glance

| Tool / concept          | What it does                                                                 | Your guide                                                                            |
|-------------------------|------------------------------------------------------------------------------|---------------------------------------------------------------------------------------|
| Environment segregation | Separates dev, staging, and production configuration and release lifecycles. | Use different directories and tfvars; never point environments at the same state key. |
| Microstate              | A smaller independent state boundary for a component or layer.               | Use distinct network and app roots to isolate changes and locking.                    |
| terraform_remote_state  | Reads root outputs from another Terraform state.                             | Protect access because the reader may access the full state snapshot.                 |
| SSM Parameter Store     | Stores selected values that another root can read.                           | Share only the VPC/subnet values required by the app layer.                           |
| Code promotion          | Uses common module code with environment-specific configuration.             | Promote the same reviewed module version through dev and staging.                     |

## Completion evidence

Submit the multi-env/ repository, a directory tree, backend key mapping, dev apply screenshot, Nginx browser screenshot, and plan output for staging. Include a short note explaining the selected inter-state communication method and its access implications.

## Cleanup / hand-off

Destroy application state first, then network state, for each environment. Run destroy from each root directory and verify the intended state only is affected. Keep the remote backend bucket and its state objects until the instructor confirms that the lab is complete.

## Recommended references

- [<u>Remote state</u>](https://developer.hashicorp.com/terraform/language/state/remote) – Sharing outputs and decomposing infrastructure.

- [<u>S3 backend</u>](https://developer.hashicorp.com/terraform/language/backend/s3) – Unique state keys and locking.

- [<u>AWS Systems Manager Parameter Store</u>](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-parameter-store.html) – Store and retrieve configuration parameters.

## End-of-module knowledge check \| 10 points

Answer each question in one or two sentences. Score 2 points per question.

> **1.** Why does each root module need a unique backend key?
>
> **2.** Why separate network and application state?
>
> **3.** Which layer should be deployed first and why?
>
> **4.** What is the security trade-off of terraform_remote_state?
>
> **5.** How does using the same module with different tfvars support code promotion?

# Checkpoint Activity 3: Complete Multi-Environment Setup

This checkpoint confirms that the learner can operate more than one independent Terraform root module and can explain how the network and application layers connect.

| Required submission             | Acceptance criteria                                                                                         |
|---------------------------------|-------------------------------------------------------------------------------------------------------------|
| Completed multi-env/ repository | Contains shared modules and independent dev/staging network and app roots.                                  |
| Backend/state map               | Shows a unique remote state key for every root. No two roots share a key.                                   |
| Deployment evidence             | Shows a successful dev network apply followed by a successful dev app apply.                                |
| Application evidence            | Shows the deployed Nginx application in a browser.                                                          |
| Isolation evidence              | Shows that app plan/app does not manage the network resources, and that staging has its own plan/state key. |
| Cleanup evidence                | Shows app resources are destroyed before network resources when the lab is retired.                         |

| **Completion standard** The learner should be able to explain the folder they are in, which backend key it uses, which resources it owns, and which state or parameter source it reads from. A successful apply alone is not enough. |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|

**MODULE Capstone**

# Independent Multi-Tier AWS Deployment

*Bring the full learning path together. Plan independently, deploy only with approval, verify, and tear down safely.*

The capstone is an independent deployment challenge. It intentionally provides an architecture target rather than click-by-click instructions. Learners should use the patterns from Modules 1-7: modules, variables, outputs, data sources, remote state, environment/layer segregation, and careful plan review.

## Architecture requirements

| Area               | Required capability                                                                  | Design / review points                                                                                        |
|--------------------|--------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------|
| Networking         | VPC, public/private subnets, security groups, ALB connectivity.                      | ALB is internet-facing only if approved. Keep RDS and private compute paths away from direct internet access. |
| Compute            | ECS service with two tasks/instances behind an ALB; Lambda for a decoupled function. | Document task/service sizing and how ECS invokes Lambda.                                                      |
| Database           | RDS used by the application or Lambda workflow.                                      | Place in private subnets. Restrict access to the application security group.                                  |
| Storage            | S3 for application objects or artifacts.                                             | Block public access and enable appropriate encryption/versioning settings.                                    |
| Monitoring         | CloudWatch dashboards and alerts.                                                    | Show application/compute/database signals and at least one actionable alarm.                                  |
| Governance         | IAM roles and policies.                                                              | Use least privilege. Avoid long-lived access keys and broad wildcard permissions where practical.             |
| Account separation | Multi-account deployment.                                                            | Use instructor-provided accounts/roles. Keep the single-region constraint for cost control.                   |

## Target application flow

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th><p>User / Client</p>
<p>|</p>
<p>v</p>
<p>Application Load Balancer</p>
<p>|</p>
<p>v</p>
<p>ECS Service (desired count: 2)</p>
<p>|</p>
<p>v</p>
<p>Lambda function</p>
<p>|</p>
<p>+------&gt; RDS database</p>
<p>|</p>
<p>+------&gt; S3 bucket</p>
<p>CloudWatch: dashboards, logs, metrics, and alerts</p>
<p>IAM: task roles, Lambda role, and least-privilege access</p></th>
</tr>
</thead>
<tbody>
</tbody>
</table>

Treat the flow as the required logical relationship, not a fixed implementation detail. The learner must explain how ECS invokes Lambda, how the application accesses RDS/S3, and which security groups and IAM permissions permit each interaction.

## Recommended implementation sequence

**1.** Clarify the account model with the instructor. Identify which account owns the network/shared services and which account hosts the application. Use role assumption or the approved access path; never embed account credentials in code.

**2.** Design the state boundaries before writing resources. At minimum, separate network foundation from application resources. Use a separate state key for each independently managed component and account/environment.

**3.** Create and review the network layer first: VPC, subnets, route tables, security groups, and any required connectivity. Publish only the values required by dependent roots.

**4.** Build reusable modules for repeated patterns such as network, ECS service, Lambda execution role, and observability components. Avoid over-modularizing single-resource wrappers.

**5.** Deploy the data layer and storage with private networking and least-privilege access. Review encryption, backup/retention, and deletion protection settings with the instructor.

**6.** Deploy the ECS service with a desired count of two behind the ALB. Confirm target health checks and the application path.

**7.** Add the Lambda function and its invocation permissions. Verify the function can access only the required RDS/S3 resources.

**8.** Configure CloudWatch logs, dashboard widgets, and alarms. Verify that the alarm thresholds and notification path make operational sense.

**9.** Run fmt, validate, and plan separately in every root module. Review the full set of creates, replacements, and destroys. Apply only after instructor approval.

**10.** Verify the application end-to-end. Capture the ALB response, ECS service health, Lambda invocation evidence, RDS connectivity evidence, S3 object evidence, and CloudWatch dashboard/alert evidence.

**11.** Document the account/region layout, state keys, module inputs/outputs, IAM roles, data paths, known limitations, and cleanup order.

**12.** Tear down in reverse dependency order: application entry points and services, Lambda, database/storage resources as approved, then network resources. Preserve the state backend and any shared account foundations.

## Capstone deliverables

| Deliverable                | What to include                                                                                          |
|----------------------------|----------------------------------------------------------------------------------------------------------|
| Source repository          | Terraform code, lock files, module README files, and environment/layer directory structure.              |
| Architecture documentation | Diagram, account/region boundaries, application flow, security-group paths, and state boundaries.        |
| Deployment evidence        | Reviewed plans and successful apply evidence for each authorized root.                                   |
| Functional verification    | ALB response, two healthy ECS tasks, Lambda execution, RDS connectivity, S3 object evidence.             |
| Operations evidence        | CloudWatch dashboard, at least one alarm, and short explanation of what action the alarm should trigger. |
| Security evidence          | IAM role/policy summary, private database placement, and confirmation that no credentials are committed. |
| Teardown evidence          | Reviewed destroy plans and evidence of application/network cleanup in the correct order.                 |

## Capstone scoring rubric

| Area                                    | Weight | What good looks like                                                                                       |
|-----------------------------------------|--------|------------------------------------------------------------------------------------------------------------|
| Architecture and functional correctness | 25%    | Required components and application flow work in the approved single region.                               |
| Terraform quality and reuse             | 20%    | Clear HCL, reusable modules, variables/outputs, pinned provider/module versions, and readable expressions. |
| State and environment separation        | 15%    | Independent root/state boundaries, unique keys, and documented state-sharing approach.                     |
| Security and governance                 | 15%    | Private data tier, least-privilege IAM, no hardcoded credentials, and suitable S3 controls.                |
| Monitoring and operations               | 10%    | Useful dashboard, actionable alert, and clear verification steps.                                          |
| Troubleshooting and documentation       | 10%    | Learner can explain design choices, interpret plans, diagnose errors, and document the solution.           |
| Cleanup and cost control                | 5%     | Resources are safely destroyed in dependency order and the state backend is preserved.                     |

| **Cost and approval gate** RDS, NAT gateways, ALBs, and continuously running compute can incur charges. The capstone must be deployed only in an approved sandbox window with explicit instructor approval. Destroy temporary resources after the demonstration. |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|

# Practical operating checklist

Use this short checklist at every module, checkpoint, and capstone deployment.

| Before running Terraform                                | During plan review                                          | After apply                                                |
|---------------------------------------------------------|-------------------------------------------------------------|------------------------------------------------------------|
| Confirm the current folder is the intended root module. | Confirm the selected AWS account and region.                | Verify resources in the AWS console or CLI.                |
| Confirm AWS identity with the approved CLI command.     | Read every create, update, replacement, and destroy action. | Check outputs and application health.                      |
| Confirm backend key and workspace/state boundary.       | Confirm tags, names, sizes, CIDRs, and exposure.            | Capture only the evidence requested.                       |
| Run terraform fmt and terraform validate.               | Check cost and permissions before approval.                 | Record any manual verification or known limitation.        |
| Confirm no other person is applying to the same state.  | Ask for review if the plan is unexpected.                   | Destroy temporary resources when the activity is complete. |

## Common errors and first checks

| Symptom                                           | First checks                                                                                                                                                           |
|---------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Unable to locate credentials / access denied      | Confirm the selected AWS profile, login/session status, account ID, and IAM permissions. Do not paste credentials into code.                                           |
| Resource already exists / bucket name unavailable | S3 bucket names are globally unique. Use a unique prefix and inspect whether the object is already managed by another state.                                           |
| No matching VPC, subnet, or AMI                   | Confirm region, filters, account, and the instructor-provided sandbox network/image requirements.                                                                      |
| terraform init requests reinitialization          | Check whether provider requirements, module sources, or backend settings changed. Re-run init only after confirming the intended backend.                              |
| State lock cannot be acquired                     | Confirm no apply is running. Ask the state owner/instructor to inspect the lock. Never force-unlock without proving the lock is stale.                                 |
| Plan proposes unexpected destroy/replacement      | Check for removed for_each keys, changed resource names/identities, changed immutable arguments, and configuration drift. Do not apply until understood.               |
| Nginx URL does not load                           | Check instance health, user_data status/logs, subnet route, public IP, security group port 80, and whether the app process is running.                                 |
| App root cannot read network values               | Check the network apply completed, the correct parameter/state key is referenced, IAM permissions allow the read, and the value exists in the expected region/account. |

## Suggested .gitignore

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th><p>.terraform/</p>
<p>*.tfstate</p>
<p>*.tfstate.*</p>
<p>*.tfplan</p>
<p>*.plan</p>
<p>crash.log</p>
<p>crash.*.log</p>
<p>*.auto.tfvars</p>
<p># Keep a non-secret example file if needed:</p>
<p># !terraform.tfvars.example</p>
<p># Commit .terraform.lock.hcl. Never commit secrets, state files, or saved plans.</p></th>
</tr>
</thead>
<tbody>
</tbody>
</table>

# Appendix A: Reusable module lesson template

Use the following template if the curriculum is expanded with additional topics or if an instructor creates an extra lab.

| Section              | What to include                                                                                            |
|----------------------|------------------------------------------------------------------------------------------------------------|
| Objectives and scope | What the learner should know or produce by the end of the module.                                          |
| Diagram              | What the learner will build and how the pieces connect. Use a diagram only when it improves understanding. |
| Topic explanation    | A short definition, one clear explanation, and a practical example.                                        |
| Key concepts table   | Tool/concept, what it does, and the learner’s guide or decision rule.                                      |
| Try it yourself      | Numbered steps the learner can execute in order, including commands and expected results.                  |
| Debug exercise       | A deliberately broken configuration, clear reproduction steps, expected symptom, and cleanup instructions. |
| End-of-module quiz   | A 10-point knowledge check tied to the objectives and activity.                                            |

## Debug exercise design rules

- State the starting directory and the prerequisite resources.

- List only the steps needed to reproduce the fault. Avoid long narrative instructions.

- Describe the expected symptom without giving away the fix.

- Tell learners what evidence to capture.

- Provide exact cleanup instructions and identify any resources that must remain.

# Appendix B: Official references

The curriculum is based on the uploaded Terraform Bootcamp Curriculum and has been sequenced into a continuous practical path. The links below point directly to official HashiCorp or AWS documentation/tutorial pages. Online documentation changes over time; use the version pinned in the lab and check the linked page when implementing outside the bootcamp.

- [<u>Terraform AWS Get Started</u>](https://developer.hashicorp.com/terraform/tutorials/aws-get-started) – Step-by-step AWS learning path: install, create, manage, destroy, and collaborate.

- [<u>Terraform CLI commands</u>](https://developer.hashicorp.com/terraform/cli/commands) – Reference for init, validate, plan, apply, destroy, fmt, graph, and other CLI commands.

- [<u>Terraform format and validate</u>](https://developer.hashicorp.com/terraform/cli/code) – Formatting and configuration validation guidance.

- [<u>Terraform language overview</u>](https://developer.hashicorp.com/terraform/language) – HCL configuration language overview.

- [<u>Input variables</u>](https://developer.hashicorp.com/terraform/language/values/variables) – Declare and use input variables.

- [<u>Output values</u>](https://developer.hashicorp.com/terraform/language/values/outputs) – Declare values exposed by a root or child module.

- [<u>Data sources</u>](https://developer.hashicorp.com/terraform/language/data-sources) – Read information from provider APIs and other sources.

- [<u>Expressions</u>](https://developer.hashicorp.com/terraform/language/expressions) – Conditional expressions, for expressions, references, and function calls.

- [<u>Built-in functions</u>](https://developer.hashicorp.com/terraform/language/functions) – Function reference and terraform console examples.

- [<u>Meta-arguments</u>](https://developer.hashicorp.com/terraform/language/meta-arguments) – count, for_each, depends_on, and lifecycle.

- [<u>Terraform state purpose</u>](https://developer.hashicorp.com/terraform/language/state/purpose) – Why state is required to map configuration to real infrastructure.

- [<u>Remote state</u>](https://developer.hashicorp.com/terraform/language/state/remote) – Remote state and sharing outputs.

- [<u>S3 backend</u>](https://developer.hashicorp.com/terraform/language/backend/s3) – S3 backend configuration, native lock files, and the deprecated DynamoDB locking option.

- [<u>Modules overview</u>](https://developer.hashicorp.com/terraform/language/modules) – Root modules, child modules, and module sources.

- [<u>Creating modules</u>](https://developer.hashicorp.com/terraform/language/modules/develop) – Guidance for designing reusable modules.

- [<u>Standard module structure</u>](https://developer.hashicorp.com/terraform/language/modules/develop/structure) – Recommended module file and directory structure.

- [<u>AWS provider configuration</u>](https://developer.hashicorp.com/terraform/tutorials/configuration-language/configure-providers) – AWS provider authentication approaches and default tags.

- [<u>AWS create infrastructure tutorial</u>](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/aws-create) – AWS credentials, provider configuration, and EC2 deployment.

- [<u>AWS S3 user guide</u>](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html) – S3 bucket concepts and security.

- [<u>AWS Systems Manager Parameter Store</u>](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-parameter-store.html) – Publish and retrieve selected configuration values between layers.

Source note: The original uploaded curriculum supplied the module sequence, AWS-first direction, Nginx checkpoints, S3 state backend intent, module/DR activity, multi-environment microstate activity, and capstone components. This guide makes the activity sequence explicit and updates the S3 state-locking recommendation to match current HashiCorp documentation.

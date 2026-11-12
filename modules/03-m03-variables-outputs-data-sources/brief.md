# m03: Variables, Outputs and Data Sources

**Builds on:** `02-m02-first-aws-resource` · **Feeds into:** `04-cp1-nginx-frontend`

## Objective

By the end of this stage, the trainee has a parameterised `main/` lab whose existing bucket is configurable through input variables and reports its details through outputs, and which discovers the AWS account, network information and Amazon Linux image through data sources for the Nginx checkpoint.

## Scope

| # | Subject | Time | Key concepts and terms | Research | Lab chunk |
| --- | --- | --- | --- | --- | --- |
| 3.1 | Variables and outputs | 2.5 h | input variable, variable types (string, number, bool, list, set, map), description, `terraform.tfvars`, output value, `terraform output`, sensitive values and outputs | What is the difference between an input variable and an output, what must never go into a tfvars file, and when should a value be marked sensitive? | Steps 1–7: Parameterise the existing bucket |
| 3.2 | Data sources versus resources | 3.5 h | data source, caller identity, VPC and subnet lookup, AMI filters and explicit owner, attribute references (`type.name.attribute`), computed values | When should a data source be used instead of a resource, and why is querying an approved AMI better than hardcoding an AMI ID? | Steps 8–12: Query AWS data |
| | **Total** | **6.0 h** | | | |

## Stack constraints

| Constraint | Detail |
| --- | --- |
| Runtime | Same as m02. |
| Environment | Continue in the existing `main/` root module and keep the S3 bucket from m02. Do not create a new root module or reset the bucket. |
| Data | Network information is instructor-provided: the default VPC if permitted, otherwise a provided VPC ID as an input variable. The AMI is the latest approved Amazon Linux image, found by filters and an explicit owner. Non-secret lab values only in tfvars, or the instructor-provided variable method. |
| Lab conventions | Same as m02 (authentication, region, default tags, operating checklist, plan before apply, state handling, cost control, public access). |

## Lab

This stage has one lab, built up in chunks. The Lab chunk column in the Scope table shows which steps practise each subject. The Acceptance row is what the trainer checks.

### Lab m03: Parameterise the bucket and query AWS data

| Step | What the trainee does |
| --- | --- |
| 1 | Adds variables for `aws_region`, `environment`, `owner` and `bucket_prefix`, each with a description and an appropriate type. |
| 2 | Replaces the hardcoded region, environment, owner and naming values in the provider and resource configuration with variable references. |
| 3 | Creates a `terraform.tfvars` file for non-secret lab values, or uses the instructor-provided variable method. |
| 4 | Adds outputs for the S3 bucket name and ARN, each with a description. |
| 5 | Reads the output values documentation on sensitive outputs and decides, for each output in this lab, whether it should be marked sensitive, with the reason. |
| 6 | Runs `fmt`, `validate` and `plan`, confirming the resource remains the same bucket and is not renamed. |
| 7 | Applies if the plan is limited to intended changes, then runs `terraform output` to inspect the values. |
| 8 | Adds a data source for the current AWS caller identity and outputs the account ID. |
| 9 | Uses the sandbox network information supplied by the instructor: queries the default VPC with an AWS VPC data source if permitted, otherwise takes the provided VPC ID as an input variable and looks up the required subnet. |
| 10 | Adds an AMI data source for the latest approved Amazon Linux image using filters and an explicit owner. |
| 11 | References the data-source attributes from resource configuration. |
| 12 | Runs `plan` and identifies which values come from variables, which from data sources and which are computed from managed resources. |
| **Acceptance** | The plan after parameterising proposes no rename or replacement of the existing bucket. No hardcoded region, environment, owner or naming values remain in the configuration. Every variable and output has a description. `terraform output` shows the bucket name and ARN. No secrets are in the tfvars file. For each output, the trainee states whether it is sensitive and why. The account ID output matches the sandbox account. No AMI ID is hardcoded. Every value in the plan is labelled as variable, data source or computed. The data sources add no managed resources to the plan. |

## Deliverable

The deliverable contains:

- [ ] `main/` configuration with variables, outputs, non-secret `terraform.tfvars` and the data sources
- [ ] `terraform output` result
- [ ] Plan output annotated with the origin of each value
- [ ] `write-up.md`, filled in from `write-up-template.md`

## Still open / ask the trainer

| Question | Why it matters |
| --- | --- |
| Does the sandbox have a usable default VPC, or are VPC and public subnet identifiers provided? | Lab chunk 3.2 and the Nginx checkpoint depend on it |
| Which Amazon Linux image counts as approved, and which owner does the filter use? | Lab chunk 3.2 |
| Which variable method does the instructor expect instead of tfvars, if any? | Lab chunk 3.1 |
| How deep should the written explanations be? | Avoids over- or under-investing in the write-up |

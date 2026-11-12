# m06: Modules and DR Copy

**Builds on:** `07-m05-5-conditionals-expressions` · **Feeds into:** `09-m07-multi-env-microstates`

## Objective

By the end of this stage, the trainee has replaced the one-off Nginx deployment in `main/` with a reusable network module and two calls to a reusable web-app module, giving two matching Nginx endpoints in separate Availability Zones of one region, as a DR-style learning exercise rather than a production disaster-recovery design.

## Scope

| # | Subject | Time | Key concepts and terms | Research | Lab chunk |
| --- | --- | --- | --- | --- | --- |
| 6.1 | Root and child modules | 1.0 h | root module, child module, `module` block, local module source, standard module structure | What is the difference between a root module and a child module, and why call one web-app module twice instead of copying its resource blocks? | Steps 1–2: Retire the one-off deployment and lay out the modules |
| 6.2 | Network module | 3.0 h | VPC, Internet Gateway, public subnets, public route table, subnet associations, module inputs and outputs | Why should module inputs and outputs have descriptions, and which values should the network module expose to its caller? | Steps 3–6: Reusable network module |
| 6.3 | Web-app module | 2.5 h | security group, `user_data`, instance outputs, module focus | Why should networking resources stay out of the web-app module? | Steps 7–11: Reusable web-app module |
| 6.4 | Root composition and DR deployment | 3.0 h | `module.network`, two `web_app` calls, `init` for local sources, plan review, DR-style deployment | Why is the second Nginx deployment not a complete production DR solution? | Steps 12–18: Call the modules from the root module |
| 6.5 | Registry module review | 0.5 h | Terraform Registry, module source, version, inputs, outputs, examples, maintenance status | What should be reviewed before adopting a public Registry module? | Step 19: Review a Registry module |
| | **Total** | **10.0 h** | | | |

## Stack constraints

| Constraint | Detail |
| --- | --- |
| Runtime | Same as m04. |
| Environment | Continue in `main/`; local child modules live in `modules/network/` and `modules/web_app/`. The Checkpoint 1 evidence is captured before the Nginx resources are changed. |
| Data | VPC and subnet CIDRs, the two Availability Zones, the AMI and the instance type are instructor-approved. The two AZs are passed in from the root module, not hardcoded in a module. |
| Scope limit | The second deployment uses two Availability Zones in one region. It has no automated failover, replicated data or recovery-time objective. |
| Lab conventions | Same as m02 (authentication, region, default tags, operating checklist, plan before apply, state handling, cost control, public access). |
| Cleanup | After the evidence is accepted, `terraform destroy` runs from `main/` and the network and both web deployments are verified as removed. The remote state bucket is not destroyed. The module source is kept for m07. |

## Lab

This stage has one lab, built up in chunks. The Lab chunk column in the Scope table shows which steps practise each subject. The Acceptance row is what the trainer checks.

### Lab m06: Reusable network and web-app modules, deployed twice

| Step | What the trainee does |
| --- | --- |
| 1 | After the Checkpoint 1 evidence is captured, removes the original one-off Nginx instance and its security group from the `main/` configuration and applies the reviewed destroy for those resources only, keeping the S3 lab bucket and the remote backend. |
| 2 | Creates `modules/network/` and `modules/web_app/`, each with `main.tf`, `variables.tf`, `outputs.tf` and `README.md`. |
| 3 | Defines inputs for the VPC CIDR, two public subnet CIDRs, two Availability Zones and a name prefix. |
| 4 | Creates one VPC, an Internet Gateway, two public subnets in separate Availability Zones, a public route table and subnet associations, tagged consistently. |
| 5 | Outputs the VPC ID and the list of public subnet IDs, with descriptions on all variables and outputs. |
| 6 | Passes the two approved Availability Zones in from the root module instead of hardcoding an AZ name. |
| 7 | Defines inputs for name, VPC ID, subnet ID, AMI ID and the approved instance type. |
| 8 | Creates a security group that allows HTTP port 80 for the sandbox test and does not open SSH to the internet. |
| 9 | Creates one EC2 instance in the supplied subnet using the supplied AMI, installing and starting Nginx through `user_data`. |
| 10 | Outputs the instance ID, public IP and public URL, with descriptions on all inputs and outputs. |
| 11 | Keeps the module focused on the web-app concept, with no networking resources inside it. |
| 12 | Calls `module.network` once in `main/` and passes the approved VPC and subnet inputs. |
| 13 | Calls `module.web_app` twice, giving the first call a primary name and the first public subnet ID and the second call a DR name and the second public subnet ID. |
| 14 | Passes the same VPC ID and approved AMI to both calls and keeps the instance size small and consistent with sandbox policy. |
| 15 | Exposes the two web URLs as root outputs so a reviewer can test both deployments. |
| 16 | Runs `terraform init` because local module sources were added, then `fmt`, `validate` and `plan`. |
| 17 | Reviews the plan for one VPC, two public subnets, two web security groups and two EC2 instances plus the associated network resources, confirming the old one-off instance is not unexpectedly recreated. |
| 18 | Applies the plan, opens both URLs and confirms both Nginx pages load. |
| 19 | Reads the Terraform Registry page for a comparable AWS VPC module and checks its source, version, inputs, outputs, examples and maintenance status. |
| **Acceptance** | The destroy of the one-off instance and its security group touched only those resources, and the S3 bucket and backend remain. Both module directories contain `main.tf`, `variables.tf`, `outputs.tf` and `README.md`. Every variable and output has a description. No Availability Zone name is hardcoded in the module. The module outputs the VPC ID and the list of public subnet IDs. Inputs cover name, VPC ID, subnet ID, AMI ID and instance type, all with descriptions. The security group allows port 80 and no SSH from the internet. The module outputs the instance ID, public IP and public URL. The module contains no networking resources. The plan before apply shows one VPC, two public subnets, two web security groups and two instances plus the associated network resources. The old one-off instance is not recreated. Both Nginx URLs load. Both web deployments come from the same module source, and only the input values differ. The written review covers the module's source, version, inputs, outputs, examples and maintenance status. |

## Deliverable

The deliverable contains:

- [ ] Repository with `main/`, `modules/network/` and `modules/web_app/`
- [ ] `README.md` in each module
- [ ] Screenshot of the reviewed apply
- [ ] Browser screenshots proving both Nginx URLs are reachable
- [ ] Architecture diagram showing one VPC, two public subnets in separate AZs and two web instances
- [ ] `write-up.md`, filled in from `write-up-template.md`

## Still open / ask the trainer

| Question | Why it matters |
| --- | --- |
| Which two Availability Zones, CIDR ranges and name prefix are approved? | Lab chunks 6.2 and 6.4 |
| Which Registry VPC module is reviewed? The source only says a comparable AWS VPC module. | Lab chunk 6.5 |
| How deep should the written explanations be? | Avoids over- or under-investing in the write-up |

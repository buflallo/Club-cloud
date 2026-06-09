# Beginner Project 05 - Cloud Primitives and Infrastructure as Code

## Introduction

You have already built servers, automated them, containerized applications, and operated small Kubernetes clusters. This project introduces the next layer: thinking like a cloud engineer before touching a real cloud bill.

The goal is to model core cloud primitives on a low-end machine using Terraform and Docker. You will build an infrastructure that behaves like a small cloud environment: isolated networks, compute units, a private database, a public entrypoint, secrets, state, and reproducible changes.

This project is intentionally broad. You are expected to research Terraform documentation, provider documentation, Docker networking, and infrastructure design tradeoffs.

This is not a cloud replacement. It prepares the mental model. Real cloud IAM, remote state locking, managed databases, regional networking, and provider limits still belong to the core curriculum.

---

## Learning Goals

By the end of this project, you should be able to:

- Explain what Terraform state is and why it must be protected.
- Use Terraform providers, variables, outputs, modules, and resource dependencies.
- Model cloud concepts such as VPCs, public/private networks, compute, load balancing, security boundaries, and managed databases using local resources.
- Understand the difference between changing infrastructure manually and changing it through code.
- Detect and explain configuration drift.
- Design a small infrastructure without being given every command.

---

## Mandatory Infrastructure

You must create a local infrastructure using Terraform.

Your infrastructure must include:

- At least two isolated Docker networks:
  - one public-facing network
  - one private/internal network
- At least one reverse proxy or load balancer container.
- At least two application containers behind the entrypoint.
- At least one database container that is not directly reachable from the host.
- Persistent database storage.
- Environment-based configuration.
- A clear separation between reusable Terraform modules and environment-specific configuration.
- At least two environments, such as `dev` and `staging`, using different variable values.

You may use any simple web application stack. The application itself is not the focus. The infrastructure model is the focus.

---

## Terraform Requirements

Your Terraform code must use:

- A pinned provider version.
- Variables for names, ports, image tags, and environment-specific values.
- Outputs that expose useful connection information.
- At least one reusable module.
- Explicit resource dependencies only where they are truly needed.
- A local state file ignored by Git.
- A documented state backup strategy.
- A written remote-state design for a future team setup.

You must demonstrate:

- `terraform plan` before applying changes.
- A successful rebuild from clean state.
- A controlled infrastructure change, such as changing replica count, image tag, or exposed port.
- One drift scenario and how you detected or corrected it.

### Cloud Translation Requirement

You must include a short `CLOUD_MAPPING.md` or equivalent section in your README that maps your local design to AWS or GCP.

It must answer:

- Which local resource represents a VPC?
- Which local resource represents a public subnet?
- Which local resource represents a private subnet?
- Which local rule represents a security group or firewall rule?
- Which local container represents compute?
- Which local database represents a managed database?
- Where would remote state live in a team project?
- Which IAM permissions would Terraform need at minimum?
- What parts of your local design do not translate cleanly to real cloud?

---

## Constraints

- No real AWS/GCP/Azure resources are required.
- No cloud account is required.
- No manual `docker run` infrastructure is accepted for final delivery.
- Do not commit secrets, state files, generated credentials, or local override files.
- Do not use `latest` image tags.
- Do not expose the database port to the host.
- Do not place all Terraform code in one giant file.

---

## Documentation Requirements

Your README must explain:

- What cloud concepts your local infrastructure represents.
- How your Terraform code is organized.
- What your modules do.
- How state is handled.
- How to build, change, destroy, and rebuild the infrastructure.
- What drift test you performed.
- What design choices you made and why.
- How your local design maps to AWS or GCP.

You should also include a small diagram showing network boundaries and traffic flow.

---

## Evaluation Focus

You will be evaluated on your understanding, not on copying a specific solution.

Be ready to explain:

- Why state matters.
- What happens when a resource is changed outside Terraform.
- Which resources are public and which are private.
- How one service discovers another.
- How secrets and environment values move through the system.
- What would change if this design moved to AWS or GCP.
- Why local Terraform state is acceptable for this project but risky for a team cloud project.

---

## Suggested Research Areas

- Terraform providers, resources, data sources, modules, variables, outputs, and state.
- Docker networks, volumes, DNS-based service discovery, and container isolation.
- Cloud VPCs, subnets, security groups, load balancers, and managed databases.
- Infrastructure drift and immutable infrastructure.

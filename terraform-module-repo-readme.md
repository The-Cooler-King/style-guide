# Terraform Modules Repository

## Overview
This repository contains reusable Terraform modules maintained by the <Your Team Name> team.
Modules are designed to be composable, consistent, and production-ready.

## Structure
```
terraform-modules/
├── <module-name>/
│   ├── main.tf
│   ├── variables.tf
│   ├── outputs.tf
│   └── README.md
└── README.md   <-- this file
```

## Conventions
- Each module must include:
  - A `README.md` using the standard template
  - A `variables.tf` and `outputs.tf` file
  - Example usage in the README
- Inputs and outputs must be documented using [terraform-docs](https://terraform-docs.io/).

## Available Modules
- [VPC](./vpc/README.md) — Creates a VPC with subnets and NAT gateways
- [ECS Service](./ecs-service/README.md) — Deploys an ECS service with autoscaling

## Contribution Guidelines
1. Create a feature branch.
2. Update or create module README using the template.
3. Run `terraform-docs` to refresh Inputs/Outputs.
4. Submit a pull request for review.

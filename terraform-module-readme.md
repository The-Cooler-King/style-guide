# VPC Module

## Overview
This module provisions a VPC with public and private subnets, routing tables, and optional NAT gateways.
Designed for AWS environments requiring multi-AZ redundancy.

## Usage
```hcl
module "vpc" {
  source = "../vpc"

  name   = "my-app"
  cidr   = "10.0.0.0/16"
  azs    = ["us-west-2a", "us-west-2b"]
}
```

## Requirements
- Terraform >= 1.3
- AWS Provider >= 5.0

## Providers
<!-- terraform-docs:insert generated providers -->

## Inputs
<!-- terraform-docs:insert generated inputs -->

## Outputs
<!-- terraform-docs:insert generated outputs -->

## Notes
- NAT gateways incur additional costs.
- Private subnets are not directly accessible from the internet.

# Day 61 - Terraform Introduction

## What is Infrastructure as Code (IaC)?

Infrastructure as Code (IaC) is the process of managing and provisioning cloud infrastructure using code instead of manually creating resources through a web console. With IaC, infrastructure becomes repeatable, version-controlled, and easy to automate. Terraform allows us to define resources like EC2 instances, S3 buckets, and VPCs in configuration files, making deployments consistent and reliable.

---

## Terraform Apply Output

**Screenshot:** *(Insert screenshot of `terraform apply` showing the creation of the S3 bucket and EC2 instance.)*

Example:

```bash
terraform apply

aws_s3_bucket.my_bucket: Creating...
aws_instance.web_server: Creating...
aws_s3_bucket.my_bucket: Creation complete
aws_instance.web_server: Creation complete

Apply complete! Resources: 2 added, 0 changed, 0 destroyed.
```

---

## AWS Console Resources

**Screenshot:** *(Insert screenshots showing:*
- *S3 Bucket in the AWS S3 Console*
- *EC2 Instance in the AWS EC2 Console*)*

---

## Terraform Commands

| Command | Purpose |
|---------|---------|
| `terraform init` | Initializes the Terraform project by downloading the required providers and creating the `.terraform` directory. |
| `terraform plan` | Shows what Terraform will create, update, or destroy before making any changes. |
| `terraform apply` | Creates or updates infrastructure based on the Terraform configuration. |
| `terraform destroy` | Deletes all infrastructure managed by the current Terraform configuration. |
| `terraform show` | Displays the current state of the managed infrastructure in a human-readable format. |
| `terraform state list` | Lists all resources currently tracked in the Terraform state file. |

---

## Terraform State File

Terraform stores information about the infrastructure it manages in a file called **terraform.tfstate**. This file maps the resources defined in the Terraform configuration to the actual resources in the cloud. Terraform uses the state file to determine what changes are needed during future `plan` and `apply` operations. Because it contains important infrastructure data, the state file should be stored securely and should not be modified manually.

---

## Summary

Today I learned the basics of Terraform and Infrastructure as Code. I created an AWS S3 bucket and an EC2 instance using Terraform, understood the purpose of the main Terraform commands, and learned how the state file helps Terraform track and manage cloud resources efficiently.

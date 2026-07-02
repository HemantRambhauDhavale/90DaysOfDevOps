# Day 64 – Terraform State Management and Remote Backends

## What I Learned

Today I learned how Terraform manages infrastructure state and why remote state is important for team environments.

---

# Terraform State

Explored:

- terraform show
- terraform state list
- terraform state show

Learned:

terraform.tfstate stores:

- resource IDs
- metadata
- attributes
- current infrastructure state
- dependencies

---

# Remote Backend

Created:

- S3 bucket
- DynamoDB table

Configured:

Terraform Remote Backend

Migrated:

Local state → S3

Verified:

terraform state stored successfully in S3.

---

# State Locking

Tested:

Two Terraform operations simultaneously.

Observed:

Terraform locked the state using DynamoDB.

Learned:

State locking prevents multiple users from modifying infrastructure at the same time.

---

# Import Existing Resources

Created S3 bucket manually.

Imported using:

terraform import

Learned:

Terraform can manage existing infrastructure without recreating it.

---

# Terraform State Commands

Practiced:

terraform state list

terraform state show

terraform state mv

terraform state rm

terraform import

Learned:

State operations modify Terraform state without changing AWS resources.

---

# State Drift

Manually modified EC2 tag from AWS Console.

Executed:

terraform plan

Terraform detected configuration drift.

Applied:

terraform apply

Infrastructure returned to the desired state.

---

# Important Commands

terraform show

terraform state list

terraform state show

terraform import

terraform state mv

terraform state rm

terraform init

terraform apply

terraform plan

terraform force-unlock

---

# Key Learning

Terraform state is the source of truth that connects Terraform configuration with real cloud infrastructure. Managing state properly is essential for safe and reliable Infrastructure as Code.

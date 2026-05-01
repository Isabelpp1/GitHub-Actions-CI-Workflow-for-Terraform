# oyd-exercise-2-2 — GitHub Actions CI Workflow for Terraform

This repository contains a Terraform workspace that provisions an S3 bucket, along with a GitHub Actions CI pipeline that validates every pull request targeting `main` and posts the full Terraform plan as a collapsible comment on the PR.

## Workflow Overview

The pipeline (`.github/workflows/terraform-ci.yml`) runs the following steps on every PR targeting `main`:

1. **fmt** — `terraform fmt --check -recursive` fails the PR on formatting errors.
2. **init** — `terraform init -backend=false` (no remote state required).
3. **validate** — `terraform validate` checks configuration correctness.
4. **plan** — `terraform plan -var-file=envs/dev/dev.tfvars` captures output to `plan.txt`.
5. **comment** — Posts the plan inside a collapsible `<details>` block on the PR.

## Repository Secrets Required

| Secret | Description |
|---|---|
| `AWS_ACCESS_KEY_ID` | AWS access key |
| `AWS_SECRET_ACCESS_KEY` | AWS secret access key |
| `AWS_REGION` | AWS region (e.g. `us-east-1`) |

## Evidence

<!-- Replace the URL below with the actual PR link after the pipeline runs successfully -->
Pull Request: [PR #1 — Initial CI pipeline run](../../pull/1)

![PR comment](evidence/pr-comment.png)

# AWS Public EKS (Free Tier Optimized)

This project provides a simple, publicly accessible Amazon EKS cluster designed to be as cost-effective as possible. It utilizes AWS Free Tier eligible resources, cost-saving measures like Spot instances, and is optimized for high density with **Prefix Delegation** enabled, allowing up to **110 pods per node** even on small instance types like `t3.small`.

## Architecture

The infrastructure consists of:
- **VPC**: A simple VPC with 2 public subnets across 2 Availability Zones.
- **EKS Cluster**: Amazon EKS version 1.34 with public endpoint access.
- **Node Group**: A managed node group using **SPOT** instances (`t3.small`) to minimize costs.
- **IAM Roles**: Minimum necessary roles for EKS cluster and node group operations.

## Cost Optimization Measures

To keep costs low, this project implements:
- **Spot Instances**: Uses EC2 Spot instances for the node group, potentially saving up to 90% compared to On-Demand prices.
- **Minimal Scaling**: Defaulted to 1 node, with a maximum of 2.
- **Free Tier Usage**: Uses standard VPC resources and `t3.small` instances (while not strictly free tier for EKS, they are among the cheapest options and EKS itself has a fixed hourly cost).
  - *Note: EKS cluster management has a fixed cost of $0.10 per hour.*

## Prerequisites

- [Terraform](https://www.terraform.io/downloads.html) (~> 1.0)
- [AWS CLI](https://aws.amazon.com/cli/) configured with appropriate credentials.
- [kubectl](https://kubernetes.io/docs/tasks/tools/) for interacting with the cluster.

## Deployment

1. **Initialize Terraform**:
   ```bash
   terraform init
   ```

2. **Plan the Infrastructure**:
   ```bash
   terraform plan
   ```

3. **Apply the Configuration**:
   ```bash
   terraform apply
   ```

4. **Update Kubeconfig**:
   After the apply is complete, update your local `kubeconfig` using the outputted cluster name:
   ```bash
   aws eks update-kubeconfig --region $(terraform output -raw region) --name $(terraform output -raw cluster_name)
   ```
   *(Note: You may need to manually specify the region if not using the output).*

## Cleanup

To avoid ongoing charges, destroy the infrastructure when finished:
```bash
terraform destroy
```

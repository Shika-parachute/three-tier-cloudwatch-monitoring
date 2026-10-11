# AWS Cleanup Guide

## Purpose

This document provides a cleanup checklist for removing AWS resources created or used during the three-tier CloudWatch monitoring project.

Cleanup is important to avoid unnecessary AWS charges after completing the project.

## 1. ECS Services

Stop or delete the ECS services that are no longer required.

```text
three-tier-frontend-service-3
three-tier-backend-service
three-tier-mongodb-service-2
```

## 2. ECS Cluster

Delete the ECS cluster after all services and tasks have been removed.

```text
three-tier-cluster-new
```

## 3. Application Load Balancer

Remove the Application Load Balancer if it is no longer required.

Also review and remove:

- Target groups
- Listeners
- Listener rules

## 4. CloudWatch

Review and remove monitoring resources that are no longer required:

- CloudWatch dashboards
- CloudWatch alarms
- Unused CloudWatch log groups

Example dashboard:

```text
Three-Tier-ECS-Monitoring
```

Example alarms:

```text
Three-Tier-Backend-High-CPU
Three-Tier-ALB-5XX-Errors
Three-Tier-ALB-Unhealthy-Target
```

## 5. Cloud Map

Remove unused Cloud Map services and namespaces after the ECS services have been deleted.

Example namespace:

```text
three-tier.local
```

## 6. Amazon EFS

Delete the EFS file system only after confirming that its data is no longer required.

## 7. Amazon ECR

Review the ECR repositories and remove unused images or repositories if they are no longer required.

Example repositories:

```text
three-tier-frontend
three-tier-backend
three-tier-mongodb
```

## 8. IAM

Review IAM roles and policies created specifically for the project.

Remove resources only when they are no longer required by other workloads.

Example role:

```text
GitHubActionsThreeTierDeploy
```

## 9. Final Review

Before finishing cleanup, verify that unnecessary resources have been removed from the AWS Console.

```text
ECS Services        → Reviewed
ECS Cluster         → Reviewed
Load Balancer       → Reviewed
Target Groups       → Reviewed
CloudWatch          → Reviewed
Cloud Map           → Reviewed
EFS                 → Reviewed
ECR                 → Reviewed
IAM                 → Reviewed
```

## Cost Management

AWS resources can continue to generate charges while they remain active.

Always review the AWS Console after completing the project to confirm that no unnecessary resources are still running.

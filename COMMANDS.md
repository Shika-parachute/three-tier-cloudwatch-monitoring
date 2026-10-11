# Commands Reference

This file contains the main commands used while working with the CloudWatch monitoring project.

## Navigate to Project

```powershell
cd "C:\Users\anush\OneDrive\Desktop\three-tier-cloudwatch-monitoring"
```

## Git Commands

Check repository status:

```powershell
git status
```

Add project files:

```powershell
git add .
```

Commit changes:

```powershell
git commit -m "Update CloudWatch monitoring documentation"
```

Push changes to GitHub:

```powershell
git push origin main
```

Check configured remote:

```powershell
git remote -v
```

## Docker Commands

Check running containers:

```powershell
docker ps
```

Check Docker images:

```powershell
docker images
```

## AWS CLI

Check AWS identity:

```powershell
aws sts get-caller-identity
```

Check AWS region:

```powershell
aws configure get region
```

## ECS Commands

List ECS clusters:

```powershell
aws ecs list-clusters
```

List services in a cluster:

```powershell
aws ecs list-services --cluster three-tier-cluster-new
```

Check running tasks:

```powershell
aws ecs list-tasks --cluster three-tier-cluster-new
```

Describe an ECS service:

```powershell
aws ecs describe-services --cluster three-tier-cluster-new --services three-tier-backend-service
```

## CloudWatch Commands

List CloudWatch dashboards:

```powershell
aws cloudwatch list-dashboards
```

List CloudWatch alarms:

```powershell
aws cloudwatch describe-alarms
```

List CloudWatch log groups:

```powershell
aws logs describe-log-groups
```

## Important Note

AWS CLI commands require valid AWS credentials and appropriate IAM permissions.

Resource names and identifiers may vary depending on the AWS environment.

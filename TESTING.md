# Testing & Verification

## Purpose

This document describes the testing performed to verify the CloudWatch monitoring setup for the three-tier ECS application.

## 1. ECS Service Verification

The three ECS services were checked to confirm that the application tasks were running.

```text
Backend Service    → 1/1 Running
Frontend Service   → 1/1 Running
MongoDB Service    → 1/1 Running
```

## 2. ECS Metrics Verification

The following metrics were checked in Amazon CloudWatch:

- CPUUtilization
- MemoryUtilization
- LiveTaskCount

Metrics were reviewed for the frontend, backend, and MongoDB ECS services.

## 3. ALB Metrics Verification

Application Load Balancer metrics were checked for:

- RequestCount
- TargetResponseTime
- HTTPCode_ELB_4XX_Count
- HTTPCode_ELB_5XX_Count
- HTTPCode_ELB_503_Count
- HealthyHostCount
- UnHealthyHostCount

## 4. CloudWatch Logs Verification

Container log groups were opened and reviewed.

Backend logs confirmed:

```text
Backend running on port 5000
Connected to MongoDB
```

This verified successful backend startup and database connectivity.

## 5. CloudWatch Alarm Verification

The following alarms were checked:

```text
Three-Tier-Backend-High-CPU
Three-Tier-ALB-5XX-Errors
Three-Tier-ALB-Unhealthy-Target
```

The backend CPU alarm reached the `OK` state during verification.

## 6. Dashboard Verification

The `Three-Tier-ECS-Monitoring` dashboard was checked to verify that ECS metrics, task-count widgets, and alarm status information were available.

## 7. Final Verification

```text
ECS Services         → PASS
ECS Metrics          → PASS
ALB Metrics          → PASS
CloudWatch Logs      → PASS
CloudWatch Alarms    → PASS
CloudWatch Dashboard → PASS
```

## Result

The CloudWatch monitoring setup was successfully tested and verified for the three-tier ECS application.

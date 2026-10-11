# Troubleshooting Guide

This document covers common issues that may occur while monitoring the three-tier ECS application with Amazon CloudWatch.

## 1. ECS Task Not Running

### Symptoms

- ECS service shows fewer running tasks than expected.
- `LiveTaskCount` is lower than the desired count.

### Checks

Verify the ECS service:

```powershell
aws ecs describe-services --cluster three-tier-cluster-new --services three-tier-backend-service
```

Check the ECS task status and service events from the AWS Console.

---

## 2. High CPU Utilization

### Symptoms

The `CPUUtilization` metric increases significantly.

### Checks

- Open the ECS service in CloudWatch.
- Check the CPU utilization graph.
- Review the backend container logs.
- Check whether the workload has increased.

The configured backend CPU alarm threshold was:

```text
CPU Utilization > 70%
```

---

## 3. High Memory Utilization

### Symptoms

`MemoryUtilization` increases significantly.

### Checks

- Review ECS memory metrics.
- Check container logs for errors.
- Verify the task definition memory configuration.
- Check whether the application is consuming more memory than expected.

---

## 4. ALB 5XX Errors

### Symptoms

The `Three-Tier-ALB-5XX-Errors` alarm may enter an alarm state.

### Checks

Review:

```text
HTTPCode_ELB_5XX_Count
HTTPCode_ELB_503_Count
TargetResponseTime
HealthyHostCount
UnHealthyHostCount
```

Also check the frontend and backend ECS services.

---

## 5. Unhealthy ALB Target

### Symptoms

The `Three-Tier-ALB-Unhealthy-Target` alarm may indicate an unhealthy target.

### Checks

- Check the target group health status.
- Verify the frontend container is running.
- Verify the frontend is listening on port `80`.
- Review ECS task logs.
- Check the target group's health-check configuration.

---

## 6. Backend Cannot Connect to MongoDB

### Symptoms

The backend may fail to connect to MongoDB.

### Checks

Verify the Cloud Map service name:

```text
mongodb.three-tier.local:27017
```

Review backend CloudWatch Logs for database connection errors.

Successful connectivity was verified with:

```text
Connected to MongoDB
```

---

## 7. CloudWatch Logs Not Appearing

### Checks

Verify that the expected log group exists:

```text
/ecs/three-tier-backend-task
/ecs/three-tier-frontend-task
/ecs/three-tier-mongodb-task
```

Then check whether new log streams and events are being generated.

---

## 8. Alarm Shows Insufficient Data

An alarm can temporarily show `INSUFFICIENT_DATA` when CloudWatch does not have enough recent metric data.

### Checks

- Confirm the monitored resource is running.
- Confirm the selected metric is receiving data.
- Check the alarm period and evaluation settings.
- Wait for sufficient metric datapoints.

---

## Troubleshooting Approach

```text
Problem
   |
   v
Check ECS Service
   |
   v
Check CloudWatch Metrics
   |
   v
Check ALB Health
   |
   v
Check CloudWatch Logs
   |
   v
Check CloudWatch Alarm
   |
   v
Identify Root Cause
```

CloudWatch metrics, logs, dashboards, and alarms provide a centralized starting point for troubleshooting application and infrastructure issues.

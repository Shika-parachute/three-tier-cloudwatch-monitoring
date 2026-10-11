# Setup Guide

## Project Overview

This project documents the monitoring setup for a three-tier application running on Amazon ECS with AWS Fargate.

Amazon CloudWatch was used to monitor ECS services, Application Load Balancer metrics, container logs, dashboards, and alarms.

## AWS Region

```text
us-east-1
```

## Prerequisites

Before setting up the monitoring environment, the following were required:

- AWS Account
- Amazon ECS application
- ECS services running on AWS Fargate
- Application Load Balancer
- Amazon CloudWatch
- AWS Cloud Map
- Amazon ECR
- Amazon EFS

## ECS Services

The monitored application consisted of three ECS services:

```text
Frontend
Nginx :80

Backend
Node.js + Express :5000

MongoDB
MongoDB :27017
```

## CloudWatch Metrics

The following ECS metrics were monitored:

- CPUUtilization
- MemoryUtilization
- LiveTaskCount

ALB metrics were also monitored for traffic, errors, response time, and target health.

## CloudWatch Logs

Container logs were collected using CloudWatch Logs.

Example log groups:

```text
/ecs/three-tier-backend-task
/ecs/three-tier-frontend-task
/ecs/three-tier-mongodb-task
```

## Dashboard Setup

A CloudWatch dashboard named:

```text
Three-Tier-ECS-Monitoring
```

was created to provide centralized visibility into ECS metrics, task counts, and alarm states.

## Alarm Setup

The following alarms were configured:

```text
Three-Tier-Backend-High-CPU
Three-Tier-ALB-5XX-Errors
Three-Tier-ALB-Unhealthy-Target
```

## Verification

After setup, ECS metrics, ALB metrics, CloudWatch Logs, alarms, and dashboard widgets were verified from the AWS Console.

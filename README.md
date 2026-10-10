# Three-Tier CloudWatch Monitoring

A professional monitoring and observability project for a containerized three-tier application running on Amazon ECS Fargate and monitored using Amazon CloudWatch.

---

## Project Overview

This project focuses on monitoring a three-tier application consisting of:

- Frontend
- Backend
- MongoDB

The application was deployed using Amazon ECS Fargate and monitored using Amazon CloudWatch.

The monitoring solution provides centralized visibility through:

- CloudWatch Metrics
- CloudWatch Logs
- CloudWatch Dashboard
- CloudWatch Alarms
- ECS monitoring
- Application Load Balancer monitoring

---

## Objectives

The main objectives of this project are:

- Monitor ECS Fargate services
- Monitor CPU utilization
- Monitor memory utilization
- Monitor running ECS tasks
- Monitor Application Load Balancer metrics
- Collect and inspect container logs
- Create CloudWatch alarms
- Build a centralized monitoring dashboard
- Verify application health
- Document the monitoring architecture

---

## Architecture

```text
User / Browser
      |
      v
Application Load Balancer
HTTP :80
      |
      v
ECS Fargate - Frontend
Nginx :80
      |
      | backend.three-tier.local:5000
      v
ECS Fargate - Backend
Node.js + Express :5000
      |
      | mongodb.three-tier.local:27017
      v
ECS Fargate - MongoDB
MongoDB :27017
      |
      v
Amazon CloudWatch
Metrics | Logs | Dashboard | Alarms

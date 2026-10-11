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

## Application Flow

The monitoring flow follows the deployed three-tier ECS application:

```text
User Request
     |
     v
Application Load Balancer
     |
     v
Frontend ECS Service
(Nginx :80)
     |
     | API Requests
     v
Backend ECS Service
(Node.js + Express :5000)
     |
     v
MongoDB ECS Service
(:27017)
     |
     v
Persistent Application Data

        |
        v
Amazon CloudWatch
-------------------------
• ECS Metrics
• ALB Metrics
• Container Logs
• Dashboard
• Alarms
```

Amazon CloudWatch provides centralized visibility into the health and performance of the application infrastructure.

## AWS Cloud Map

AWS Cloud Map was used for service discovery between the ECS services.

The application uses the private namespace:

```text
three-tier.local
```

The ECS services were registered with the following service-discovery names:

```text
Backend
backend.three-tier.local:5000

MongoDB
mongodb.three-tier.local:27017
```

This allows the application components to communicate using service names instead of hard-coded IP addresses.

### Service Discovery Flow

```text
Frontend
   |
   | backend.three-tier.local:5000
   v
Backend
   |
   | mongodb.three-tier.local:27017
   v
MongoDB
```

Cloud Map therefore provides dynamic service discovery for the backend and MongoDB services.

## Technology Stack

| Category | Technology |
|---|---|
| Cloud Platform | Amazon Web Services (AWS) |
| Container Orchestration | Amazon ECS |
| Compute | AWS Fargate |
| Monitoring | Amazon CloudWatch |
| Load Balancer | Application Load Balancer (ALB) |
| Service Discovery | AWS Cloud Map |
| Container Logs | CloudWatch Logs |
| Application | Node.js + Express |
| Frontend Server | Nginx |
| Database | MongoDB |
| Persistent Storage | Amazon EFS |
| Container Images | Amazon ECR |

## AWS Services Used

### Amazon ECS

Amazon ECS was used to run and manage the three-tier application containers.

The monitored ECS services were:

- Frontend Service
- Backend Service
- MongoDB Service

### AWS Fargate

AWS Fargate provided the serverless compute environment for running the ECS tasks without managing EC2 servers.

### Amazon CloudWatch

CloudWatch was the main monitoring service used in this project. It was used for:

- ECS CPU utilization
- ECS memory utilization
- ECS live task count
- Application Load Balancer metrics
- Container logs
- CloudWatch dashboards
- CloudWatch alarms

### Application Load Balancer

The Application Load Balancer was monitored for application traffic and availability-related metrics such as:

- Request count
- Target response time
- HTTP 4XX errors
- HTTP 5XX errors
- Healthy target count
- Unhealthy target count

### AWS Cloud Map

Cloud Map provided service discovery between the ECS services using DNS-based service names.

### Amazon ECR

Amazon ECR stored the container images used by the ECS application.

### Amazon EFS

Amazon EFS provided persistent storage for the MongoDB container.

## ECS Monitoring

Amazon CloudWatch was used to monitor the health and performance of the ECS services.

The following ECS metrics were monitored:

### CPU Utilization

`CPUUtilization` shows the percentage of CPU resources being used by an ECS service.

This helps identify:

- High CPU usage
- CPU-intensive workloads
- Potential performance issues

### Memory Utilization

`MemoryUtilization` shows the percentage of memory being used by an ECS service.

This helps identify:

- High memory consumption
- Memory pressure
- Potential resource limitations

### Live Task Count

`LiveTaskCount` was used to verify whether ECS tasks were running as expected.

This helps confirm:

- Tasks are running
- ECS services are maintaining the expected task count
- Service availability can be monitored

### Monitored ECS Services

```text
Frontend ECS Service
        |
        +-- CPU Utilization
        +-- Memory Utilization
        +-- Live Task Count

Backend ECS Service
        |
        +-- CPU Utilization
        +-- Memory Utilization
        +-- Live Task Count

MongoDB ECS Service
        |
        +-- CPU Utilization
        +-- Memory Utilization
        +-- Live Task Count
```

## Application Load Balancer Monitoring

The Application Load Balancer was monitored using Amazon CloudWatch to observe application traffic, response performance, and target health.

### Key ALB Metrics

| Metric | Purpose |
|---|---|
| `RequestCount` | Monitors the number of requests received by the load balancer |
| `TargetResponseTime` | Measures the response time of the registered targets |
| `HTTPCode_ELB_4XX_Count` | Tracks client-side 4XX errors generated by the load balancer |
| `HTTPCode_ELB_5XX_Count` | Tracks 5XX errors generated by the load balancer |
| `HTTPCode_ELB_503_Count` | Helps identify service-unavailable responses |
| `HealthyHostCount` | Shows the number of healthy targets |
| `UnHealthyHostCount` | Shows the number of unhealthy targets |

### ALB Monitoring Flow

```text
Internet
   |
   v
Application Load Balancer
   |
   +-- Request Count
   +-- Response Time
   +-- 4XX Errors
   +-- 5XX Errors
   +-- Healthy Targets
   +-- Unhealthy Targets
   |
   v
Frontend ECS Service
```

These metrics help identify traffic-related issues, target health problems, and application availability issues.

## CloudWatch Logs

Amazon CloudWatch Logs was used to collect and inspect container logs generated by the ECS services.

### Log Groups

The following log groups were used for the application:

```text
/ecs/three-tier-backend-task
/ecs/three-tier-frontend-task
/ecs/three-tier-mongodb-task
```

An older log group was also present:

```text
/ecs/three-tier-ecs-task
```

### Backend Log Verification

The backend logs were checked to verify that the application started successfully and established a database connection.

Example log messages included:

```text
Backend running on port 5000
Connected to MongoDB
```

CloudWatch Logs made it possible to troubleshoot application startup, container behavior, and database connectivity without directly accessing the running containers.

### Logging Flow

```text
ECS Containers
      |
      v
CloudWatch Logs
      |
      +-- Frontend Logs
      +-- Backend Logs
      +-- MongoDB Logs
      |
      v
Troubleshooting & Verification
```

## CloudWatch Alarms

CloudWatch Alarms were configured to automatically monitor important application and infrastructure conditions.

### Configured Alarms

| Alarm | Metric | Condition | Purpose |
|---|---|---|---|
| `Three-Tier-Backend-High-CPU` | `CPUUtilization` | Greater than 70% | Detects high backend CPU usage |
| `Three-Tier-ALB-5XX-Errors` | `HTTPCode_ELB_5XX_Count` | Greater than 0 | Detects ALB 5XX errors |
| `Three-Tier-ALB-Unhealthy-Target` | `UnHealthyHostCount` | Greater than 0 | Detects unhealthy ALB targets |

### Alarm Configuration

The backend CPU alarm was configured with:

```text
Namespace: AWS/ECS
Metric: CPUUtilization
Statistic: Average
Period: 5 minutes
Threshold: > 70%
Evaluation: 1 out of 1 datapoints
```

The ALB alarms were configured to monitor load balancer errors and target health.

### Alarm Status

The backend high-CPU alarm reached an `OK` state during verification, confirming that the backend CPU utilization was below the configured threshold.

The ALB alarms were also added to the monitoring dashboard so that their current state could be viewed centrally.

### Alarm Monitoring Flow

```text
CloudWatch Metrics
        |
        v
   Alarm Evaluation
        |
        +---- Normal ----> OK
        |
        +---- Threshold Breached
                    |
                    v
                 ALARM
```
## CloudWatch Dashboard

A centralized CloudWatch dashboard named `Three-Tier-ECS-Monitoring` was created to provide a single view of the application's monitoring metrics.

### Dashboard Metrics

The dashboard includes ECS CPU and memory metrics for:

- Backend ECS Service
- Frontend ECS Service
- MongoDB ECS Service

Additional monitoring widgets were added for:

- Backend Live Task Count
- MongoDB Live Task Count
- CloudWatch Alarm Status

### Dashboard Overview

```text
Three-Tier-ECS-Monitoring
          |
          +-- Backend CPU
          +-- Backend Memory
          |
          +-- Frontend CPU
          +-- Frontend Memory
          |
          +-- MongoDB CPU
          +-- MongoDB Memory
          |
          +-- Backend Live Task Count
          +-- MongoDB Live Task Count
          |
          +-- Alarm Status
```

The dashboard provides centralized visibility into resource utilization, task availability, and alarm conditions.

### Dashboard Benefits

- Centralized monitoring
- Faster issue identification
- ECS resource utilization visibility
- Service availability monitoring
- Alarm status visibility
- Easier troubleshooting

  ## Testing & Verification

The monitoring setup was verified by running the ECS services and checking their metrics, logs, dashboard widgets, and alarm states.

### ECS Service Verification

All three application services were running successfully during monitoring verification:

```text
Backend Service    → 1/1 Running
Frontend Service   → 1/1 Running
MongoDB Service    → 1/1 Running
```

### Metrics Verification

The following metrics were successfully observed in CloudWatch:

- CPU Utilization
- Memory Utilization
- Live Task Count
- ALB Request Count
- Target Response Time
- HTTP 4XX errors
- HTTP 5XX errors
- Healthy Target Count
- Unhealthy Target Count

### Log Verification

CloudWatch Logs were checked to verify successful application startup and database connectivity.

```text
Backend running on port 5000
Connected to MongoDB
```

### Alarm Verification

The configured alarms were checked from the CloudWatch console.

The backend CPU alarm reached the `OK` state during verification, confirming that the monitored CPU utilization was below the configured threshold.

### Dashboard Verification

The `Three-Tier-ECS-Monitoring` dashboard was verified with ECS resource metrics, task-count widgets, and alarm-status information.

### Verification Result

```text
ECS Services        → Verified
ECS Metrics         → Verified
ALB Metrics         → Verified
CloudWatch Logs     → Verified
CloudWatch Alarms   → Verified
CloudWatch Dashboard→ Verified
```
## Project Structure

```text
three-tier-cloudwatch-monitoring/
│
├── cloudwatch/
│   └── CloudWatch monitoring configuration and related files
│
├── docs/
│   └── Project documentation
│
├── screenshots/
│   ├── 01-cloudwatch-dashboard.png
│   ├── 02-ecs-metrics.png
│   ├── 03-log-groups.png
│   ├── 04-backend-logs.png
│   ├── 05-cloudwatch-alarm.png
│   ├── 06-alarm-state.png
│   └── 07-ecs-monitoring.png
│
├── scripts/
│   └── Monitoring and supporting scripts
│
├── .gitignore
├── ARCHITECTURE.md
├── CLEANUP.md
├── COMMANDS.md
├── README.md
├── SETUP.md
├── TESTING.md
├── TROUBLESHOOTING.md
└── VERIFICATION.md
```

The repository is organized to keep monitoring configuration, documentation, screenshots, scripts, and verification information separate and easy to navigate.

## Security & Best Practices

The monitoring setup followed AWS best practices wherever applicable.

- CloudWatch was used for centralized monitoring and logging.
- ECS services were monitored through service-level metrics.
- CloudWatch alarms were configured for important conditions.
- Container logs were collected in CloudWatch Logs for troubleshooting.
- AWS Cloud Map was used for service-to-service discovery instead of hard-coded service IP addresses.
- Persistent MongoDB data was stored using Amazon EFS.
- IAM permissions were used to control access to AWS resources.
- Sensitive credentials and environment-specific values were not committed to the repository.
- `.gitignore` was configured to prevent sensitive or unnecessary files from being committed.

  ## Cost Management & Cleanup

AWS resources used for monitoring were intended for project and learning purposes.

After completing the monitoring and verification activities, unnecessary AWS resources should be removed to avoid unexpected charges.

### Cleanup Checklist

- Delete CloudWatch dashboards that are no longer required.
- Delete CloudWatch alarms that are no longer required.
- Delete unused CloudWatch log groups.
- Delete unused ECS services and tasks.
- Delete unused ECS clusters.
- Delete unused Application Load Balancer resources.
- Delete unused target groups.
- Delete unused EFS resources.
- Delete unused Cloud Map services and namespaces.
- Delete unused ECR repositories and images.
- Review IAM resources created specifically for the project.

### Cost Awareness

Monitoring resources such as CloudWatch dashboards, logs, and alarms can have associated AWS costs depending on usage.

Regular cleanup and resource review help prevent unnecessary charges.

## Key Learnings

This project provided practical experience with AWS monitoring and observability for a containerized three-tier application.

### Technical Learnings

- Learned how to monitor ECS services using Amazon CloudWatch.
- Learned how to analyze CPU and memory utilization.
- Learned how to monitor ECS task availability using Live Task Count.
- Learned how to monitor Application Load Balancer metrics.
- Learned how to collect and analyze ECS container logs.
- Learned how to create and configure CloudWatch alarms.
- Learned how to build a centralized CloudWatch dashboard.
- Learned how AWS Cloud Map supports service discovery between ECS services.
- Learned how monitoring helps identify application and infrastructure issues.
- Learned the importance of cleaning up AWS resources after completing a project.

### Overall Learning

The project demonstrated how Amazon CloudWatch can provide centralized visibility into the health, performance, availability, and logs of a containerized application running on Amazon ECS.

## Final Status

The CloudWatch monitoring implementation was successfully completed and verified for the three-tier ECS application.

### Completed Components

- ECS service monitoring
- CPU utilization monitoring
- Memory utilization monitoring
- Live task count monitoring
- Application Load Balancer monitoring
- CloudWatch container logging
- Backend log verification
- CloudWatch dashboard
- CloudWatch alarms
- Alarm status monitoring
- Monitoring documentation
- Project screenshots and verification

### Final Result

```text
Three-Tier ECS Application
          |
          v
Amazon CloudWatch
          |
          +-- ECS Metrics
          +-- ALB Metrics
          +-- Container Logs
          +-- Dashboard
          +-- Alarms
```

The project demonstrates centralized monitoring and observability of a containerized three-tier application using AWS monitoring services.

## Conclusion

This project successfully implemented monitoring and observability for a containerized three-tier application using Amazon CloudWatch.

The monitoring setup provided visibility into ECS resource utilization, task availability, Application Load Balancer performance, container logs, dashboards, and alarm conditions.

The project demonstrates how CloudWatch can be used to centrally monitor an AWS-based containerized application and support troubleshooting, performance analysis, and service availability.

---

## Repository

**GitHub Repository:**  
https://github.com/Shika-parachute/three-tier-cloudwatch-monitoring


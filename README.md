\# Three-Tier CloudWatch Monitoring



Monitoring and observability project for a containerized three-tier application using Amazon ECS, AWS Fargate, and Amazon CloudWatch.



\## Overview



This project focuses on monitoring a three-tier application consisting of:



\- Frontend — Nginx

\- Backend — Node.js + Express

\- Database — MongoDB

\- Service Discovery — Amazon Cloud Map

\- Monitoring — Amazon CloudWatch



The monitoring environment was created, tested, and verified using AWS CloudWatch metrics, logs, dashboards, and alarms.



\## Architecture



```text

User / Browser

&#x20;     |

&#x20;     v

Application Load Balancer :80

&#x20;     |

&#x20;     v

ECS Fargate - Frontend

Nginx :80

&#x20;     |

&#x20;     | backend.three-tier.local:5000

&#x20;     v

ECS Fargate - Backend

Node.js + Express :5000

&#x20;     |

&#x20;     | mongodb.three-tier.local:27017

&#x20;     v

ECS Fargate - MongoDB

MongoDB :27017



&#x20;     |

&#x20;     v

Amazon CloudWatch

Metrics | Logs | Dashboard | Alarms


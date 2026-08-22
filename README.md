# DevOps & Cloud Projects Portfolio

## About

Welcome to my **DevOps & Cloud Projects Portfolio**.

This repository contains hands-on projects demonstrating practical experience with **AWS Cloud, Linux, Python, Git, Jenkins, CI/CD, databases, networking, and cloud automation**.

Each project includes implementation details, configuration steps, code, testing, troubleshooting, and screenshots.

---

## Projects

### 1. Automated File Upload Pipeline

**Local Machine → Amazon S3 → AWS Lambda → Amazon RDS → Amazon SNS**

An event-driven AWS data ingestion pipeline that automatically uploads CSV files from a local machine to Amazon S3. An S3 event triggers Lambda to process the file and insert the data into Amazon RDS, with SNS notifications for success or failure.

**Technologies:**

* Amazon S3
* AWS Lambda
* Amazon RDS
* Amazon SNS
* AWS CloudWatch
* AWS IAM
* Python
* SQL

[View Project →](./Automated-File-Upload-Pipeline/)

---

### 2. EFS → S3 Near-Real-Time File Synchronization

**EC2 → EFS → inotify → AWS CLI → Amazon S3**

A centralized AWS file-sharing and backup solution using Amazon EFS across two EC2 instances. Linux `inotify` monitors file changes on the shared EFS filesystem and automatically synchronizes changes to Amazon S3 using the AWS CLI.

**Technologies:**

* Amazon EC2
* Amazon EFS
* Amazon S3
* Amazon VPC
* AWS IAM
* Linux
* AWS CLI
* inotify

[View Project →](./EFS-S3 Realtime Sync/)

---

### 3. Jenkins CI/CD Pipeline: Controller → Agent → Tomcat

**GitHub → Jenkins Controller → Jenkins Agent → Maven → WAR → Tomcat**

A CI/CD pipeline that retrieves a Java application from GitHub, builds it using Maven, packages it as a WAR file, and automatically deploys the application to Apache Tomcat.

**Technologies:**

* Jenkins
* GitHub
* Java
* Maven
* Apache Tomcat
* Linux
* Groovy
* cURL

[View Project →](./Jenkins-CI-CD-Tomcat-Deployment/)

---

## Skills Demonstrated

### Cloud & AWS

* Amazon EC2
* Amazon S3
* Amazon EFS
* AWS Lambda
* Amazon RDS
* Amazon SNS
* AWS CloudWatch
* AWS IAM
* Amazon VPC

### DevOps & CI/CD

* Jenkins
* Jenkins Pipelines
* Git & GitHub
* Maven
* Apache Tomcat
* CI/CD automation

### Programming & Automation

* Python
* Groovy
* Bash
* SQL
* AWS CLI
* Linux automation

### Infrastructure & Systems

* Linux system administration
* VPC networking
* Security Groups
* IAM policies
* EFS/NFS
* Application deployment
* Cloud infrastructure

---

## Repository Structure

```text
devops-cloud-projects-portfolio/
│
├── README.md
│
├── Automated-File-Upload-Pipeline/
│   ├── README.md
│   └── images/
│
├── EFS-S3-Realtime-Sync/
│   ├── README.md
│   └── images/
│
└── Jenkins-CI-CD-Tomcat-Deployment/
    ├── README.md
    └── images/
```

---

## Project Focus

These projects were built to develop practical hands-on experience in:

* Cloud Engineering
* DevOps
* AWS infrastructure
* Linux administration
* CI/CD
* Cloud automation
* Networking
* Application deployment
* Monitoring and troubleshooting

The projects focus on building solutions manually and understanding how the underlying AWS and DevOps components work together.

---

## Future Projects

Planned areas for future projects include:

* AWS Infrastructure as Code with Terraform
* Docker containerization
* Kubernetes
* GitHub Actions
* AWS networking
* Monitoring and observability
* Advanced CI/CD automation

---

## Goal

The goal of this portfolio is to demonstrate practical, hands-on experience designing, implementing, troubleshooting, and documenting **AWS Cloud and DevOps solutions**.

Each project is documented separately with its architecture, implementation steps, configuration, testing results, challenges, and lessons learned.

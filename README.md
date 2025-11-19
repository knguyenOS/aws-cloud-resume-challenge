# AWS Cloud Resume Challenge
![banner](/Diagrams/AWS-Architecture-Design.png)

# Cloud Resume Hosting Project
This project was developed to further my understanding of cloud concepts upon taking the AWS Cloud Practitioner. See my website [here](http://khangcloud.com)

The [Cloud Resume Challenge](https://cloudresumechallenge.dev/) is a hands-on project designed by [Forrest Brazeal](https://forrestbrazeal.com/) to provide a pathway from obtaining a cloud certification to implementing practical skills in a workcloud job. By deploying your own personal website and utilizing DevOps practices. it gives you hands-on experiences to prepare for a job in the cloud industry. I will be using Amazon Web Services (AWS) to attempt the challenge and document my progress.


# Table of Contents
- [Project Overview]
  - 

## Benefits of the challenge
- Provides practical experience designing and deploying a full end-to-end cloud project using key AWS services
- Demonstrates core serverless skills like API integration, event-driven compute, and scalable storage.
- Strengthens DevOps fundamentals including CI/CD, automated testing, and Infrastructure as Code (IaC).
- Improve troubleshooting skills and ability to read documentation and follow best practices.

## Challenge Stages
### Stage 1 — Foundations
First challenge is to obtain a certification which ensures you understand cloud fundamentals before continuing. For this project, I worked towards AWS services and so took the [AWS Cloud Practitioner](https://aws.amazon.com/certification/certified-cloud-practitioner/) Certification. [Here](https://www.credly.com/badges/81a987db-7829-4954-88d2-c5cf20330e22/public_url) is my credly badge

### Stage 2 — Front-End Resume Website
The resume should be created using HTML. It does not have to be pretty or contain sublime styling, since the challenge is not about perfect styling and responsive web design

CSS – Style the resume using CSS.



Static Website – Deploy the webpage as an S3 Static Website.

The resume page is accessible only via CloudFront Distribution.
The S3 Bucket serving the static content has all all public access blocked - OAC is configured with said S3 bucket as the origin with the bucket only allowing requests from CloudFront OAC.
The requests from HTTP are redirected to HTTPS.
DNS – Point a custom domain (via Route 53 or another provider) to the CloudFront distribution.

### Stage 3 — Front-End Interactivity
Add a dynamic visitor counter to your resume.
JavaScript – Add client-side code to display the visitor count.

### Stage 4 — Back-End & Database
Create the architecture behind the visitor counter.
Database – Use DynamoDB to store and update the visitor counter.

API – Build an API using API Gateway + Lambda to communicate with DynamoDB.
Python – Write the Lambda function in Python using boto3.
Tests – Write tests for your Python Lambda code.
I had it where vistor counter was updated via fetching a lambda function which updated and returned the counter

### Stage 5 — Infrastructure as Code

Automate creation of the backend resources.

Infrastructure as Code – Build and deploy all backend resources using AWS SAM or Terraform.

### Stage 6 — CI/CD Automation
Automate deployments for both backend and frontend.
Source Control – Store backend code in GitHub.

CI/CD (Backend) – Use GitHub Actions to test and deploy SAM/Terraform changes automatically.

CI/CD (Frontend) – Use GitHub Actions to auto-update the S3 website (and invalidate CloudFront cache when needed).

### Stage 7 — Reflection & Documentation

Showcase what you learned.

This journey was filled with challenges, but the Cloud Resume Challenge helped me transition from AWS certifications to hands-on experience in the cloud industry.

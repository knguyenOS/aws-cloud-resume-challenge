# AWS Cloud Resume Challenge
![banner](/Diagrams/AWS-Architecture-Design.png)

# Cloud Resume Hosting Project
This project was developed to further my understanding of cloud concepts upon taking the AWS Cloud Practitioner. See my website [here](http://khangcloud.com)

The [Cloud Resume Challenge](https://cloudresumechallenge.dev/) is a hands-on project designed by [Forrest Brazeal](https://forrestbrazeal.com/) to provide a pathway from obtaining a cloud certification to implementing practical skills in a workcloud job. By deploying your own personal website and utilizing DevOps practices. it gives you hands-on experiences to prepare for a job in the cloud industry. I will be using Amazon Web Services (AWS) to attempt the challenge and document my progress.


# Table of Contents
- [AWS Cloud Resume Challenge](#aws-cloud-resume-challenge)
  - [Cloud Resume Hosting Project](#cloud-resume-hosting-project)
  - [Benefits of the Challenge](#benefits-of-the-challenge)
  - [Challenge Stages](#challenge-stages)
    - [Stage 1 — Foundations](#stage-1--foundations)
    - [Stage 2 — Front-End Resume Website](#stage-2--front-end-resume-website)
      - [2.1 HTML/CSS](#21-htmlcss)
      - [2.2 JavaScript](#22-javascript)
      - [2.3 S3](#23-s3)
      - [2.4 CloudFront](#24-cloudfront)
      - [2.5 Route53 (DNS)](#25-route53-dns)
      - [2.6 AWS Certificate Manager (ACM)](#26-aws-certificate-manager-acm)
    - [Stage 3 — Back-End & Database](#stage-3--back-end--database)
      - [3.1 Database](#31-database)
      - [3.2 API + Lambda](#32-api--lambda)
    - [Stage 4 — Frontend & Backend Integration](#stage-4--frontend--backend-integration)
      - [4.1 Dynamic Counter Value](#41-dynamic-counter-value)
    - [Stage 5 — CI/CD Automation](#stage-5--cicd-automation)
      - [5.1 CodePipeline CI/CD (Backend)](#51-codepipeline-cicd-backend)
    - [Stage 6 — Reflection & Documentation](#stage-6--reflection--documentation)


## Benefits of the challenge
- Provides practical experience designing and deploying a full end-to-end cloud project using key AWS services
- Demonstrates core serverless skills like API integration, event-driven compute, and scalable storage.
- Strengthens DevOps fundamentals including CI/CD, automated testing, and Infrastructure as Code (IaC).
- Improve troubleshooting skills and ability to read documentation and follow best practices.

## Challenge Stages
### Stage 1 — Foundations
First challenge is to obtain a certification which ensures you understand cloud fundamentals before continuing. For this project, I worked towards AWS services and so took the [AWS Cloud Practitioner](https://aws.amazon.com/certification/certified-cloud-practitioner/) Certification. [Here](https://www.credly.com/badges/81a987db-7829-4954-88d2-c5cf20330e22/public_url) is my credly badge.
![Certification]()

### Stage 2 — Front-End Resume Website
Build the visual representation of resume using plain HTML, CSS and JavaScript (which gets more important at stage 2).

#### 2.1 HTML/CSS
The resume website is built using HTML to structure the content and CSS to control the visual presentation. Although the Cloud Resume Challenge does not focus on perfect UI/UX, the site should still look polished and professional. I used a combination of a simple resume template and generative AI assistance to refine the layout, styling, and readability.

#### 2.2 JavaScript
JavaScript is used to add minimal interactivity to an otherwise static website, specifically for implementing the visitor counter feature. A small script sends a request to the backend API to retrieve and update the number of views stored in DynamoDB. This demonstrates how the front-end can interact with AWS services using asynchronous HTTP calls. The updated count is then displayed directly on the webpage.

#### 2.3 AWS S3 (simple storage service)
After completing the website, I stored the files into an S3 bucket which has the ability to host static websites. To make the site publicly accessible, I configure the bucket for public access, added a bucket policy granting read permissions, and enable static website hosting with an index document. Once set up, I was able to access the website via the S3 bucket’s website endpoint.

#### 2.4 AWS CloudFront
While hosting a static resume website directly from S3 works, it’s considered less secure as it lacks HTTPS, rovides no layer for protection, and allows outsiders direct access. Instead, we can use CloudFront with Origin Access Control (OAC) to make the page only accessible only via CloudFront Distribution link. This allows our S3 to be kept private and secure. This ensures all traffic is routed through CloudFront, where HTTPS is enabled by default. CloudFront generates a unique domain such as d123abcd89ef0.cloudfront.net, which can later be replaced with a more human-friendly custom domain name

#### 2.5 AWS Route53 (DNS) & AWS Certificate Manager (ACM)
To make the resume website accessible through a user-friendly custom domain, I registered the domain through Route 53 and pointed it to the CloudFront distribution using DNS records. I then used AWS Certificate Manager to issue an SSL/TLS certificate for my domain, enabling secure HTTPS communication. After attaching the certificate to CloudFront, the website could be accessed securely using the custom domain. This completes the front-end hosting setup with proper DNS and encryption.

### Stage 3 — Back-End & Database
This section is about extending local visitor counter (written in JavaScript) to a full API which saves the values in AWS DynamoDB database.

#### 3.1 Database
Use of DynamoDB
The visitor counter is saved and retrieved from a single Table in AWS DynamoDB.
Database – Use DynamoDB to store and update the visitor counter.

#### 3.2 API + Lambda
Python – Write the Lambda function in Python using boto3.
Tests – Write tests for your Python Lambda code.

### Stage 4 — Frontend & Backend integration
This section is about embedding the value coming from DynamoDB through AWS Lambda into the JavaScript code, making the page dynamically count and display the visitors number.

#### 4.1 Dynamic counter value
I had it where vistor counter was updated via fetching a lambda function which updated and returned the counter

### Stage 5 — CI/CD Automation
Automate deployments for both backend and frontend.
Source Control – Store backend code in GitHub.

#### 5.1 CodePipeline CI/CD (Backend)
Use GitHub Actions to test and deploy SAM/Terraform changes automatically.
CI/CD (Frontend) – Use GitHub Actions to auto-update the S3 website (and invalidate CloudFront cache when needed).

### Stage 6 — Reflection & Documentation
Showcase what you learned.

This journey was filled with challenges, but the Cloud Resume Challenge helped me transition from AWS certifications to hands-on experience in the cloud industry.

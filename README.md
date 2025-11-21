# AWS Cloud Resume Challenge
![banner](/Assets/AWS-Architecture-Design.png)

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

<p align="center">
  <img src="https://images.credly.com/size/340x340/images/00634f82-b07f-4bbd-a6bb-53de397fc3a6/image.png" width="200">
</p>


### Stage 2 — Front-End Resume Website
Build the visual representation of resume using plain HTML, CSS and JavaScript (which gets more important at stage 2).

#### 2.1 HTML/CSS
The resume website is built using HTML to structure the content and CSS to control the visual presentation. Although the Cloud Resume Challenge does not focus on perfect UI/UX, the site should still look polished and professional. I used a combination of a simple resume template and generative AI assistance to refine the layout, styling, and readability.

#### 2.2 JavaScript
JavaScript is used to add minimal interactivity to an otherwise static website, specifically for implementing the visitor counter feature. A small script sends a request to the backend API to retrieve and update the number of views stored in DynamoDB. This demonstrates how the front-end can interact with AWS services using asynchronous HTTP calls. The updated count is then displayed directly on the webpage.

#### 2.3 AWS S3 (simple storage service)
After completing the website, I stored the files into an S3 bucket which has the ability to host static websites. To make the site publicly accessible, I configure the bucket for public access, added a bucket policy granting read permissions, and enable static website hosting with an index document. Once set up, I was able to access the website via the S3 bucket’s website endpoint.

![S3](/Assets/S3.png)

#### 2.4 AWS CloudFront
While hosting a static resume website directly from S3 works, it’s considered less secure as it lacks HTTPS, rovides no layer for protection, and allows outsiders direct access. Instead, we can use [CloudFront with Origin Access Control (OAC)](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/private-content-restricting-access-to-s3.html) to make the page only accessible only via CloudFront Distribution link. This allows our S3 to be kept private and secure. This ensures all traffic is routed through CloudFront, where HTTPS is enabled by default. 

CloudFront generates a unique domain such as `d123abcd89ef0.cloudfront.net`, which can later be replaced with a more human-friendly custom domain name

#### 2.5 AWS Route53 (DNS) & AWS Certificate Manager (ACM)
To make the resume website accessible through a user-friendly custom domain, I registered the domain through [Route 53](https://aws.amazon.com/route53/) and pointed it to the CloudFront distribution using DNS records. I then used [Certificate Manager](https://aws.amazon.com/certificate-manager/) to issue an SSL/TLS certificate for my domain, enabling secure HTTPS communication. After attaching the certificate to CloudFront, the website could be accessed securely using the custom domain. This completes the front-end hosting setup with proper DNS and encryption.

![Route53](/Assets/Route53.png)

### Stage 3 — Back-End & Database
This section is about extending local visitor counter (written in JavaScript) to a full API which saves the values in AWS DynamoDB database.

#### 3.1 Database + Lambda
For the database layer, I implemented [DynamoDB](https://aws.amazon.com/dynamodb/) as a highly scalable NoSQL data store to track visitor counts reliably. I created a single table with a partition key representing the counter ID, which allowed for simple lookups and atomic updates. The [Lambda function](https://aws.amazon.com/lambda/) uses DynamoDB’s UpdateItem operation with an increment expression so the counter value is updated safely even under concurrent requests. 

The Lambda function handles two responsibilities:
- incrementing the counter in DynamoDB
- returning the updated value to the caller.

![DynamoDB](/Assets/DynamoDB.png)

#### 3.2 API Gateway
[API Gateway](https://aws.amazon.com/api-gateway/) was used to expose the Lambda function through a secure, public REST API endpoint that the frontend could call. I created a `/visitor-counter` GET route and connected it to the Lambda function using Lambda Proxy Integration, allowing JSON responses to pass through directly. After deploying the API to the `prod` stage, it generated the URL that my JavaScript uses to fetch and update the visitor count. I also enabled CORS and ensured the Lambda execution role had permission to update the DynamoDB table, completing the backend integration.


### Stage 4 — Frontend & Backend integration
This stage brings the backend logic into the user interface, enabling real-time dynamic updates to the resume website.

#### 4.1 Dynamic counter value
The frontend JavaScript was modified to fetch the visitor count directly from the Lambda-powered API each time the website loads. Using fetch(), the script sends an HTTP request to the API Gateway endpoint, triggers the Lambda function, and receives the updated count from DynamoDB. The number is then injected into the HTML DOM, allowing visitors to instantly see the real-time total.

``` HTML
    <script>
        // Invokes the URL which triggers Lambda
        const functionUrl = "https://52geyqwktk.execute-api.us-east-1.amazonaws.com/prod/visitor-counter";
      
        async function fetchViewCount() {
          try {
            const response = await fetch(functionUrl);
            const data = await response.json();
      
            // Display the view count
            document.getElementById("viewCount").innerText = data.views;
          } catch (error) {
            console.log(error)
            console.error("Error fetching view count:", error);
            document.getElementById("viewCount").innerText = "error";
          }
        }
      
        // Run when the page loads
        window.onload = fetchViewCount;
  </script>
```

### Stage 5 — CI/CD Automation
This stage automates deployments for both backend and frontend, eliminating manual uploads and ensuring consistent, repeatable updates.

#### 5.1 CodePipeline CI/CD (Backend)
For the backend, I configured [CodePipeline](https://aws.amazon.com/codepipeline/) to automatically detect changes pushed to GitHub using the [AWS Connector for GitHub](https://github.com/marketplace/aws-connector-for-github). This integration allows CodePipeline to monitor the repository and trigger builds whenever new commits are pushed. CodeBuild handles running the tests and packaging the Lambda code, while the final stage deploys the updated function automatically. This workflow ensures that every backend update is tested, validated, and deployed with no manual intervention, improving reliability and speeding up development.

![CodePipeline](/Assets/CICD-Pipeline.png)

### Stage 6 — Reflection & Documentation
In this final stage, I documented the project and reflected on the practical skills I developed throughout the process. Taking on the Cloud Resume Challenge bridged the gap between theory and hands-on implementation, reinforcing my understanding of core AWS services. By building a fully serverless application from scratch, I gained experience in real-world workflows, troubleshooting cloud components, and designing secure and scalable cloud architectures. This project complemented my certification and strengthened my confidence in working with AWS in a practical setting.

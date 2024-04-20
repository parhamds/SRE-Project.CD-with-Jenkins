# Project Overview: CD with Jenkins

## Description
This project focuses on implementing continuous deployment (CD) with Jenkins, automating the build, testing, and deployment processes for staging and production environments using AWS ECS (Elastic Container Service) and ECR (Elastic Container Registry).

## Setup Instructions

### Jenkins Configuration
1. Install necessary Jenkins plugins:
   - Docker Pipeline
   - CloudBees Docker Build and Push
   - Amazon ECR
   - Pipeline: AWS Steps

2. Manage credentials:
   - Add IAM user's access key credentials for AWS access.

3. SSH into Jenkins server:
   - Install AWS CLI
   - Install Docker
   - Add Jenkins user to Docker group
   - Restart Jenkins service

### AWS Configuration
1. IAM Setup:
   - Create an IAM user with access keys attached with permissions:
     - `AmazonEC2ContainerRegistryFullAccess`
     - `AmazonECS_FullAccess`

2. ECR Setup:
   - Create a private ECR repository.

3. ECS Configuration:
   - For Staging:
     - Create an ECS cluster for staging.
     - Create a task definition (selecting the image from ECR).
     - Create a service (enable load balancer and select created task definition).
     - Edit security group to allow port 8080 from everywhere.
     - Edit health check of the target group to port 8080.

   - For Production:
     - Create a separate branch for prod.
     - Create an ECS cluster for production.
     - Create a task definition (selecting the image from ECR).
     - Create a service (enable load balancer and select created task definition).
     - Edit security group to allow port 8080 from everywhere.
     - Edit health check of the target group to port 8080.

### Jenkins Pipeline Setup
1. Create 2 Jenkinsfiles for staging and prod environments in the codebase (push to GitHub).

2. Add ECR information to pipeline environment variables.

3. Add stages to the pipeline for:
   - Building Docker image
   - Pushing image to ECR
   - Deploying to ECS staging environment
   - Testing by testers
   - Merging to prod branch triggering the prod pipeline
   - Deploying to prod ECS cluster

4. Create a new job based on the Jenkinsfile for each environment.

### Additional Notes
- Staging and prod Jenkinsfiles differ only in the image push step since prod uses the same image as staging.
- The project follows a continuous deployment workflow where developers push code changes triggering automated build, test, and deployment processes.
- Detailed steps for each stage are provided in the practical documentation.

## Project Structure
```
project/
│
├── staging/
│   ├── Jenkinsfile
│   └── ...
│
├── prod/
│   ├── Jenkinsfile
│   └── ...
│
├── README.md
└── ...
```

## Usage
- Clone the repository.
- Follow the setup instructions for Jenkins and AWS configurations.
- Create Jenkins jobs based on the provided Jenkinsfiles for staging and production environments.
- Make necessary adjustments to fit your project requirements.

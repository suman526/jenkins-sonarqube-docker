# Automated CI/CD Pipeline with Jenkins, SonarQube, and Docker on AWS

## Building a Complete CI/CD Pipeline on AWS
## A practical to automating code deployment from source control to a live containerised application.

This project demonstrates a complete, automated CI/CD pipeline designed to provision infrastructure on AWS EC2 and deploy a web application through a series of automated stages.

<img width="1292" height="518" alt="Screenshot (447)" src="https://github.com/user-attachments/assets/d0fefb71-cc0d-4326-b1c2-72771a80a295" />


Project Overview
The pipeline follows a specific automated workflow to ensure code quality and seamless deployment:
1. Code Push: Application code is pushed to this GitHub repository.
2. Jenkins Trigger: A GitHub webhook automatically notifies the Jenkins server to pull the latest changes.
3. Static Analysis: SonarQube scans the source code to detect bugs, security vulnerabilities, and code quality issues.
4. Deployment: Once the code passes quality gates, it is transferred to a Docker server, where a new image is built and deployed as a running container.
5. Verification: The deployment is verified by accessing the application via a web browser.

--------------------------------------------------------------------------------

Requirements & Infrastructure

The project utilises three separate Ubuntu EC2 instances on AWS to isolate the different stages of the pipeline:

• Jenkins Server: Acts as the primary CI/CD orchestration tool.

• SonarQube Server: Dedicated to static code analysis; requires at least 2GB of RAM.

• Docker Server: Used for the final application deployment environment.

--------------------------------------------------------------------------------

Key Components & Ports

To ensure the pipeline functions correctly, the following ports must be opened in your AWS Security Groups:

• 8080: Jenkins web interface.

• 9000: SonarQube dashboard.

• 8085: The running web application on the Docker server.

• 22: SSH access for server configuration and Jenkins-to-Docker communication.

--------------------------------------------------------------------------------

Setup Instructions

1. Repository Preparation

• Upload your website template files and a Dockerfile to this repository.

• The provided Dockerfile uses Nginx to serve the content: FROM nginx COPY . /usr/share/nginx/html.

2. Jenkins Configuration

• Install Jenkins on the dedicated EC2 instance.

• Create a Freestyle project and link it to this GitHub repository URL.

• Enable the GitHub hook trigger for GITScm polling.

3. SonarQube Integration

• Install SonarQube and Java 17 on the SonarQube instance.

• Generate a security token in SonarQube and add it to Jenkins as a Secret Text credential.

• Add the "Execute SonarQube Scanner" build step in your Jenkins job.

4. Docker Deployment Setup

• Install Docker on the deployment server.

• Establish SSH passwordless authentication between the Jenkins server and the Docker server to allow automated transfers.

• Configure the Jenkins job to use an SSH Agent with your .pem key to execute remote commands.

5. Final Pipeline Step

The Jenkins job executes a shell script to deploy the app:

1. Transfers files using SCP.

2. Builds the Docker image: docker build -t mywebsite ..

3. Runs the container: docker run -d -p 8085:80 --name onix-website mywebsite.


Verification
Once the pipeline build is successful, you can access your website at: http://<DOCKER_SERVER_PUBLIC_IP>:8085.

--------------------------------------------------------------------------------
Analogy for Understanding
Think of this automated pipeline like an automated car factory:

• AWS EC2 is the factory building that provides the space and electricity.

• GitHub is the blueprint for the car that the designer sends in.

• Jenkins is the assembly line manager who sees the new blueprint and tells the machines to start.

• SonarQube is the safety inspector who checks the parts for defects before they are put together.

• Docker is the shipping container that holds the finished car, ensuring it reaches the customer in perfect condition, ready to drive.

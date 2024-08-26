### ASSIGNMENTs
These assignments will give students hands-on experience with real-world tools and processes used in DevOps and Cloud environments, including GitLab CI/CD, Docker, and Kubernetes on AWS.
---

### **Assignment 1: Deploying a Python Web Application Using Docker and GitLab CI/CD**

#### **Objective:**
Students will create a Dockerized Python web application, set up a CI/CD pipeline using GitLab, and deploy the application to a Docker container running on an AWS EC2 instance.

#### **Instructions:**

1. **Create a Python Web Application:**
   - Develop a simple Flask-based web application that includes at least one route (e.g., `/`, `/hello`).
   - Implement basic functionality such as returning a JSON response.

2. **Dockerize the Application:**
   - Write a `Dockerfile` to containerize the Python application.
   - Ensure the application is running in the container when executing `docker run`.

3. **Set Up GitLab CI/CD Pipeline:**
   - Create a `.gitlab-ci.yml` file in your repository.
   - The pipeline should have two stages: `build` and `deploy`.
   - The `build` stage should:
     - Build the Docker image.
     - Push the Docker image to Docker Hub.
   - The `deploy` stage should:
     - Pull the Docker image from Docker Hub.
     - Deploy the Docker container on an AWS EC2 instance.

4. **Deploy the Docker Container:**
   - Set up an AWS EC2 instance with Docker installed.
   - Use the GitLab CI/CD pipeline to deploy the Docker container to the EC2 instance.
   - The application should be accessible via the public IP of the EC2 instance.

5. **Submission Requirements:**
   - A link to the GitLab repository containing the code.
   - The EC2 instance public IP address with the application running.
   - A brief report explaining the steps taken, challenges faced, and how they were overcome.

6. **Grading Criteria:**
   - Application functionality and correctness (20 points).
   - Correct Dockerization of the application (20 points).
   - Proper setup of the GitLab CI/CD pipeline (30 points).
   - Successful deployment on AWS EC2 (20 points).
   - Report quality and clarity (10 points).

---

### **Assignment 2: Continuous Deployment of a Python Application on AWS EKS Using GitLab**

#### **Objective:**
Students will create a Kubernetes-based deployment of a Python application using AWS Elastic Kubernetes Service (EKS) and automate the deployment process using GitLab CI/CD.

#### **Instructions:**

1. **Set Up the Python Application:**
   - Develop a simple Python application (Flask or Django) with a basic route.
   - Ensure that the application is containerized using Docker.

2. **Create Kubernetes Manifest Files:**
   - Write a `deployment.yaml` file to deploy the Python application on a Kubernetes cluster.
   - Write a `service.yaml` file to expose the application via a Kubernetes Service.

3. **Set Up an EKS Cluster:**
   - Create an EKS cluster using AWS.
   - Configure the `kubectl` CLI to connect to your EKS cluster.

4. **GitLab CI/CD Pipeline Configuration:**
   - Create a `.gitlab-ci.yml` file that includes:
     - A `build` stage to build and push the Docker image to a container registry.
     - A `deploy` stage to apply the Kubernetes manifest files using `kubectl`.
   - The pipeline should utilize the `docker:dind` service for Docker operations and manage Kubernetes deployments using the `kubectl` CLI.

5. **Deploy the Application:**
   - Ensure that the application is successfully deployed to the EKS cluster and is accessible via a LoadBalancer or NodePort service.
   - Validate that the application is running by accessing the application through the provided external IP or DNS.

6. **Submission Requirements:**
   - A link to the GitLab repository with the application code and CI/CD pipeline configuration.
   - The external IP or DNS name to access the deployed application.
   - A report detailing the steps, challenges, and solutions.

7. **Grading Criteria:**
   - Application and Docker image functionality (20 points).
   - Correct setup of Kubernetes manifests (20 points).
   - Proper configuration of the EKS cluster (20 points).
   - Successful automation of deployment using GitLab CI/CD (30 points).
   - Report quality and clarity (10 points).

---


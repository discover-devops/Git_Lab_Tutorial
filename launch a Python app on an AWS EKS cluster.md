To help you launch a Python app on an AWS EKS cluster using GitLab CI/CD in an automated way, here’s a step-by-step document that outlines the process:

### Step 1: Write the Python Application Code
1. **Create a directory for your project:**
   ```bash
   mkdir my-python-app
   cd my-python-app
   ```

2. **Create the main Python file:**
   ```python
   # app.py
   from flask import Flask
   
   app = Flask(__name__)
   
   @app.route('/')
   def hello_world():
       return 'Hello, World!'
   
   if __name__ == '__main__':
       app.run(host='0.0.0.0', port=80)
   ```

3. **Create a requirements file:**
   ```bash
   touch requirements.txt
   ```
   - Add the dependencies:
   ```
   Flask==2.0.3
   ```

### Step 2: Create a Dockerfile
1. **Create a `Dockerfile` in the root directory:**
   ```dockerfile
   # Use the official Python image from the Docker Hub
   FROM python:3.9-slim
   
   # Set the working directory
   WORKDIR /app
   
   # Copy the current directory contents into the container at /app
   COPY . /app
   
   # Install any needed packages specified in requirements.txt
   RUN pip install --no-cache-dir -r requirements.txt
   
   # Make port 80 available to the world outside this container
   EXPOSE 80
   
   # Run app.py when the container launches
   CMD ["python", "app.py"]
   ```

### Step 3: Build the Docker Image
1. **Build the Docker image:**
   ```bash
   docker build -t my-python-app .
   ```

2. **Test the Docker image locally:**
   ```bash
   docker run -p 4000:80 my-python-app
   ```

   - Navigate to `http://localhost:4000` in your browser to see if the app is working.

### Step 4: Define CI/CD Variables in GitLab
1. **Set up necessary environment variables in GitLab CI/CD settings:**
   - Navigate to your GitLab project.
   - Go to `Settings > CI / CD > Variables`.
   - Define the following variables:
     - `AWS_ACCESS_KEY_ID`: Your AWS access key.
     - `AWS_SECRET_ACCESS_KEY`: Your AWS secret key.
     - `AWS_REGION`: The AWS region where your EKS cluster is deployed.
     - `DOCKER_USERNAME`: Your Docker Hub username.
     - `DOCKER_PASSWORD`: Your Docker Hub password.
     - `EKS_CLUSTER_NAME`: The name of your EKS cluster.
     - `KUBECONFIG_DATA`: Base64 encoded content of your kubeconfig file for the EKS cluster.

### Step 5: Push the Image to Docker Hub
1. **Log in to Docker Hub:**
   ```bash
   docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD
   ```

2. **Tag the Docker image:**
   ```bash
   docker tag my-python-app $DOCKER_USERNAME/my-python-app:latest
   ```

3. **Push the image to Docker Hub:**
   ```bash
   docker push $DOCKER_USERNAME/my-python-app:latest
   ```

### Step 6: Write the Kubernetes Deployment YAML
1. **Create a Kubernetes deployment YAML file:**
   ```yaml
   # deployment.yaml
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     name: python-app-deployment
   spec:
     replicas: 3
     selector:
       matchLabels:
         app: python-app
     template:
       metadata:
         labels:
           app: python-app
       spec:
         containers:
         - name: python-app
           image: $DOCKER_USERNAME/my-python-app:latest
           ports:
           - containerPort: 80
   ---
   apiVersion: v1
   kind: Service
   metadata:
     name: python-app-service
   spec:
     type: LoadBalancer
     ports:
       - port: 80
         targetPort: 80
     selector:
       app: python-app
   ```

### Step 7: Write the `.gitlab-ci.yml` File
1. **Create a `.gitlab-ci.yml` file in the root directory:**
   ```yaml
   image: docker:latest

   services:
     - docker:dind

   stages:
     - build
     - push
     - deploy

   variables:
     DOCKER_DRIVER: overlay2

   before_script:
     - docker login -u "$DOCKER_USERNAME" -p "$DOCKER_PASSWORD"

   build:
     stage: build
     script:
       - docker build -t $DOCKER_USERNAME/my-python-app:latest .

   push:
     stage: push
     script:
       - docker push $DOCKER_USERNAME/my-python-app:latest

   deploy:
     stage: deploy
     image: google/cloud-sdk
     script:
       - echo "$KUBECONFIG_DATA" | base64 --decode > /tmp/kubeconfig
       - export KUBECONFIG=/tmp/kubeconfig
       - kubectl apply -f deployment.yaml
   ```

### Step 8: Push the Code to GitLab and Trigger CI/CD Pipeline
1. **Initialize a Git repository:**
   ```bash
   git init
   git remote add origin <your-gitlab-repo-url>
   ```

2. **Add and commit the files:**
   ```bash
   git add .
   git commit -m "Initial commit"
   ```

3. **Push the code to GitLab:**
   ```bash
   git push -u origin master
   ```

4. **Monitor the CI/CD pipeline:**
   - Navigate to your GitLab project and check the pipeline's status in the CI/CD section.

Once the pipeline completes, your Python application should be deployed on the AWS EKS cluster, accessible via the LoadBalancer service created in the Kubernetes deployment YAML.

This setup automates the process of building the Docker image, pushing it to Docker Hub, and deploying the application to an EKS cluster using GitLab CI/CD.

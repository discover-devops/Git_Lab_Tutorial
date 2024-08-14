## Step-by-Step Guide to Setting Up a CI/CD Pipeline with Local Runners in GitLab

This guide will walk you through the process of setting up a CI/CD pipeline using GitLab's local runners to build and deploy a simple Flask application in a Docker container. We'll be working with the provided `.gitlab-ci.yml` file, `app.py`, and `Dockerfile`.

### Prerequisites

1. **GitLab Account**: Ensure you have a GitLab account and a project repository set up.
2. **Local Runner**: Install and configure a GitLab runner on your local Ubuntu machine. Ensure it is tagged with `localshell` and `localrunner`.
3. **Docker**: Docker should be installed and running on the local Ubuntu machine where the runner is configured.

### Step 1: Set Up Your GitLab CI/CD Pipeline

Create a `.gitlab-ci.yml` file in the root of your project repository. This file defines the stages and jobs of your pipeline.

```yaml
stages:
    - build_stage
    - deploy_stage

build:
    stage: build_stage
    script:
        - docker --version
        - docker build -t pyapp .
    tags:
        - localshell
        - localrunner

deploy:
    stage: deploy_stage
    script:
        - docker stop pyappcontainer1 || true && docker rm pyappcontainer1 || true
        - docker run -d --name pyappcontainer1 -p 8080:8080 pyapp
    tags:
        - localshell
        - localrunner
```

#### Explanation:
- **Stages**: The pipeline has two stages: `build_stage` and `deploy_stage`.
- **Build Job**: This job builds a Docker image named `pyapp` from the Dockerfile.
- **Deploy Job**: This job stops any running container named `pyappcontainer1`, removes it, and runs a new container from the `pyapp` image.
- **Tags**: The jobs are tagged with `localshell` and `localrunner` to ensure they are picked up by the local runner.

### Step 2: Create the Flask Application

Create the `app.py` file, which contains the code for your Flask web application.

```python
from flask import Flask
import os

app = Flask(__name__)

@app.route("/")
def wish():
    message = "Happy birthday {name}"
    return message.format(name=os.getenv("NAME", "John"))
    
if __name__ == "__main__":
    app.run(host='0.0.0.0', port=8080)
```

#### Explanation:
- **Flask App**: The application displays a simple message with a name that can be customized using an environment variable `NAME`.
- **Default Port**: The app runs on port 8080, making it accessible from `http://localhost:8080`.

### Step 3: Create the Dockerfile

Create a `Dockerfile` in the root of your project. This file will be used to build the Docker image for the Flask application.

```dockerfile
FROM python:3.8.0-slim
WORKDIR /app
ADD . /app
RUN pip install --trusted-host pypi.python.org Flask
ENV NAME Mark
CMD ["python", "app.py"]
```

#### Explanation:
- **Base Image**: The Docker image is built from `python:3.8.0-slim`, a lightweight version of Python.
- **Work Directory**: The application is copied into the `/app` directory inside the container.
- **Dependencies**: Flask is installed using pip.
- **Environment Variable**: The `NAME` environment variable is set to "Mark" by default.
- **Command**: The container runs the Flask app using `python app.py`.

### Step 4: Push the Code to GitLab

Commit your changes to the `.gitlab-ci.yml`, `app.py`, and `Dockerfile` files, and push the code to your GitLab repository.

```bash
git add .
git commit -m "Add Flask app with CI/CD pipeline"
git push origin main
```

### Step 5: Run the Pipeline

Once the code is pushed, GitLab will automatically trigger the pipeline. You can monitor the pipeline’s progress in the GitLab UI.

1. **Navigate to Your Project**: Go to your GitLab project.
2. **CI/CD Pipeline**: Click on `CI/CD` in the left sidebar, then select `Pipelines`.
3. **View Pipeline**: You'll see the pipeline running with two stages: `build_stage` and `deploy_stage`.

### Step 6: Verify the Deployment

After the pipeline completes successfully:

1. **Check Running Containers**: Verify that the Docker container `pyappcontainer1` is running.

   ```bash
   docker ps
   ```

2. **Access the Flask App**: Open your browser and navigate to `http://localhost:8080`. You should see the message "Happy birthday Mark".

### Step 7: Clean Up

To stop and remove the container:

```bash
docker stop pyappcontainer1
docker rm pyappcontainer1
```

### Conclusion

In this guide, we set up a CI/CD pipeline on GitLab using local runners. We built and deployed a simple Flask application inside a Docker container. The pipeline was configured to run on a local Ubuntu machine using GitLab runners tagged with `localshell` and `localrunner`.

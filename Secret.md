### What Are GitLab Secrets?

GitLab Secrets are secure variables that store sensitive information like passwords, API keys, tokens, and other confidential data. These secrets are encrypted and can be used in CI/CD pipelines to avoid hardcoding sensitive information directly into your code or configuration files.

### How to Define GitLab Secrets?

GitLab Secrets are defined as CI/CD variables in the GitLab project. These variables can be set at different levels:
- **Project Level**: Accessible only within the specific project.
- **Group Level**: Accessible within all projects in a group.
- **Instance Level**: Accessible across all projects in a GitLab instance.

#### Steps to Define Secrets at the Project Level

1. **Navigate to Your GitLab Project**:
   - Go to your GitLab instance and navigate to the project where you want to define secrets.

2. **Go to CI/CD Settings**:
   - Click on `Settings` in the left sidebar, then select `CI/CD`.

3. **Expand the Variables Section**:
   - Scroll down to the `Variables` section and expand it.

4. **Add a New Variable**:
   - Click the `Add variable` button.
   - Enter the **key** (name) of the variable, for example, `SECRET_API_KEY`.
   - Enter the **value** of the variable, for example, `your-secret-api-key`.
   - Set the **Type** to `Variable`.
   - Optionally, check the boxes to:
     - **Protect the variable**: This restricts the variable to protected branches and tags.
     - **Mask the variable**: This prevents the value from being displayed in job logs.
   - Click `Add variable` to save.

### How to Use GitLab Secrets in CI/CD Pipelines?

Once you've defined secrets in GitLab, you can use them in your `.gitlab-ci.yml` file to inject sensitive data into your CI/CD pipeline without exposing it in your source code.

#### Example 1: Using a Simple Secret

Let's define a secret API key and use it in a pipeline.

1. **Define the Secret**:
   - Navigate to `Settings` > `CI/CD` > `Variables`.
   - Add a variable named `SECRET_API_KEY` with the value `your-secret-api-key`.

2. **Use the Secret in `.gitlab-ci.yml`**:

```yaml
stages:
  - test

test_api:
  stage: test
  script:
    - echo "Using Secret API Key: $SECRET_API_KEY"
```

In this example, the pipeline job `test_api` will print the value of `SECRET_API_KEY`. The value will be securely retrieved from the GitLab variables.

#### Example 2: Using Secrets in a Python Application

Let’s assume you have a Python application that needs an API key to interact with a third-party service.

1. **Define the Secret**:
   - Navigate to `Settings` > `CI/CD` > `Variables`.
   - Add a variable named `API_KEY` with the value `your-secret-api-key`.

2. **Create the Python Application**:

```python
# app.py
import os

def main():
    api_key = os.getenv("API_KEY")
    print(f"Using API Key: {api_key}")

if __name__ == "__main__":
    main()
```

3. **Use the Secret in `.gitlab-ci.yml`**:

```yaml
stages:
  - build
  - deploy

build:
  stage: build
  script:
    - python3 -m venv venv
    - source venv/bin/activate
    - pip install -r requirements.txt
    - python3 app.py
  tags:
    - myec2runner

deploy:
  stage: deploy
  script:
    - echo "Deploying application..."
  tags:
    - myec2runner
```

In this example, the Python script `app.py` retrieves the `API_KEY` from the environment variable, which is securely provided by GitLab during the pipeline execution.

### Summary

- **GitLab Secrets**: These are secure CI/CD variables used to store sensitive data like API keys, passwords, etc.
- **Defining Secrets**: Secrets are defined in the `Settings` > `CI/CD` > `Variables` section of your GitLab project.
- **Using Secrets**: Once defined, secrets can be accessed in your `.gitlab-ci.yml` file using `$VARIABLE_NAME`.

This method ensures that sensitive data remains secure while still being accessible during the CI/CD process.

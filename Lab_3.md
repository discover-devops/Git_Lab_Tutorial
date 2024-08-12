###  step-by-step tutorial for creating and understanding GitLab CI with `stages`, `stage`, and `artifacts`. We'll go through three `.gitlab-ci.yml` files:

### 1. **First `.gitlab-ci.yml` File: Without `stages` and `artifacts`**

In this first step, we'll create a `.gitlab-ci.yml` file with two jobs: `build` and `deploy`. Since we haven't defined any `stages` or `artifacts`, the jobs will run in parallel, and the `deploy` job will likely fail due to missing build artifacts.

```yaml
# .gitlab-ci.yml (First File)
build-job:
  script:
    - echo "Building the project..."
    - mkdir build
    - echo "Build content" > build/index.html

deploy-job:
  script:
    - echo "Deploying the project..."
    - cat build/index.html
```

#### What Happens:
- Both `build-job` and `deploy-job` will run in parallel by default.
- The `deploy-job` will fail because it tries to access the `build/index.html` file, which may not exist if the `build-job` hasn’t completed before it runs.

### 2. **Second `.gitlab-ci.yml` File: Introducing `stages`**

Now, let's introduce `stages` to ensure that the `build-job` runs before the `deploy-job`.

```yaml
# .gitlab-ci.yml (Second File)
stages:
  - build
  - deploy

build-job:
  stage: build
  script:
    - echo "Building the project..."
    - mkdir build
    - echo "Build content" > build/index.html

deploy-job:
  stage: deploy
  script:
    - echo "Deploying the project..."
    - cat build/index.html
```

#### What Happens:
- The `build-job` will run first, and only after it completes successfully, the `deploy-job` will start.
- However, the `deploy-job` might still fail because the `build` directory and its contents are not passed between stages.

### 3. **Third `.gitlab-ci.yml` File: Introducing `artifacts`**

To fix the issue with the `deploy-job`, we'll introduce `artifacts` to pass the build output to the deploy stage.

```yaml
# .gitlab-ci.yml (Third File)
stages:
  - build
  - deploy

build-job:
  stage: build
  script:
    - echo "Building the project..."
    - mkdir build
    - echo "Build content" > build/index.html
  artifacts:
    paths:
      - build/

deploy-job:
  stage: deploy
  script:
    - echo "Deploying the project..."
    - cat build/index.html
```

#### What Happens:
- The `build-job` will generate the `build` directory and store it as an artifact.
- The `artifacts` keyword ensures that the `build` directory is available for the `deploy-job` in the next stage.
- The `deploy-job` will successfully access the `build/index.html` file and complete the deployment process.

### Summary:

- **First File**: Jobs run in parallel. `deploy-job` fails because the build output isn’t available.
- **Second File**: Introduced `stages` to control job execution order, but `deploy-job` still fails due to missing artifacts.
- **Third File**: Introduced `artifacts` to pass build output to the next stage, resulting in a successful pipeline execution.

By going through these steps, you’ll understand the importance of `stages`, `stage`, and `artifacts` in GitLab CI/CD pipelines!

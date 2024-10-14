In GitLab, CI/CD pipelines are defined using a YAML file called `.gitlab-ci.yml`. This file contains various commands, keywords, and structures to define the stages and jobs of the pipeline. Below is a list of important commands and keywords used in GitLab CI/CD YAML, along with explanations and examples for each.

### **1. `stages`**

Defines the different stages of the pipeline. Stages are executed sequentially.

**Example:**
```yaml
stages:
  - build
  - test
  - deploy
```

### **2. `jobs`**

Each job is a set of instructions that GitLab Runner executes. Jobs are defined under stages.

**Example:**
```yaml
build_job:
  stage: build
  script:
    - echo "Building the project"
    - ./build.sh
```

### **3. `script`**

The list of shell commands to execute by the job.

**Example:**
```yaml
test_job:
  stage: test
  script:
    - echo "Running tests"
    - ./test.sh
```

### **4. `before_script` and `after_script`**

Commands to run before or after each job.

**Example:**
```yaml
before_script:
  - echo "Setting up the environment"

after_script:
  - echo "Cleaning up"
```

### **5. `variables`**

Define custom environment variables for jobs.

**Example:**
```yaml
variables:
  DATABASE_URL: "postgres://user:password@localhost:5432/mydb"
```

### **6. `environment`**

Specifies the environment to which the job deploys. This is used for managing deployments.

**Example:**
```yaml
deploy_job:
  stage: deploy
  script:
    - echo "Deploying to production"
    - ./deploy.sh
  environment:
    name: production
    url: http://example.com
```

### **7. `only` and `except`**

Control when jobs are created. Use these keywords to restrict jobs to certain branches, tags, or Git refs.

**Example:**
```yaml
deploy_job:
  stage: deploy
  script:
    - ./deploy.sh
  only:
    - master
  except:
    - develop
```

### **8. `rules`**

More flexible than `only` and `except`, allowing for complex job conditions.

**Example:**
```yaml
test_job:
  stage: test
  script:
    - ./test.sh
  rules:
    - if: '$CI_COMMIT_REF_NAME == "main"'
    - if: '$CI_COMMIT_REF_NAME =~ /^feature\//'
```

### **9. `artifacts`**

Define artifacts to be stored after the job finishes. These can be used by subsequent jobs.

**Example:**
```yaml
build_job:
  stage: build
  script:
    - ./build.sh
  artifacts:
    paths:
      - build/
```

### **10. `cache`**

Cache dependencies to speed up the pipeline.

**Example:**
```yaml
cache:
  paths:
    - node_modules/
```

### **11. `services`**

Use Docker services for jobs.

**Example:**
```yaml
test_job:
  stage: test
  services:
    - postgres:latest
  script:
    - ./test.sh
```

### **12. `image`**

Specify the Docker image to use for the job.

**Example:**
```yaml
build_job:
  stage: build
  image: node:14
  script:
    - npm install
    - npm run build
```

### **13. `dependencies`**

Control which jobs need to be completed before running the current job.

**Example:**
```yaml
deploy_job:
  stage: deploy
  script:
    - ./deploy.sh
  dependencies:
    - build_job
```

### **14. `retry`**

Define how many times a job should be retried in case of failure.

**Example:**
```yaml
build_job:
  stage: build
  script:
    - ./build.sh
  retry: 2
```

### **15. `timeout`**

Set a time limit for the job to run.

**Example:**
```yaml
test_job:
  stage: test
  script:
    - ./test.sh
  timeout: 30 minutes
```

### **16. `tags`**

Specify which runners can pick up the job based on tags.

**Example:**
```yaml
build_job:
  stage: build
  script:
    - ./build.sh
  tags:
    - docker
```

### **17. `allow_failure`**

Allow the job to fail without failing the pipeline.

**Example:**
```yaml
lint_job:
  stage: lint
  script:
    - ./lint.sh
  allow_failure: true
```

### **18. `when`**

Control when to run the job (e.g., on_success, on_failure, always).

**Example:**
```yaml
deploy_job:
  stage: deploy
  script:
    - ./deploy.sh
  when: manual
```

### **19. `extends`**

Use to reuse configuration from another job.

**Example:**
```yaml
.default_job:
  script:
    - echo "Default script"

test_job:
  extends: .default_job
  stage: test
  script:
    - echo "Running tests"
    - ./test.sh
```

### **20. `include`**

Include external YAML files.

**Example:**
```yaml
include:
  - local: 'templates/.gitlab-ci-template.yml'
```

### **21. `parallel`**

Run multiple instances of the job in parallel.

**Example:**
```yaml
test_job:
  stage: test
  script:
    - ./test.sh
  parallel:
    matrix:
      - VERSION: [1.0, 1.1]
```

### **Example `.gitlab-ci.yml` Combining Multiple Concepts**

```yaml
stages:
  - build
  - test
  - deploy

variables:
  DATABASE_URL: "postgres://user:password@localhost:5432/mydb"

before_script:
  - echo "Setting up the environment"

after_script:
  - echo "Cleaning up"

build_job:
  stage: build
  image: node:14
  script:
    - npm install
    - npm run build
  artifacts:
    paths:
      - build/
  tags:
    - docker

test_job:
  stage: test
  services:
    - postgres:latest
  script:
    - npm test
  dependencies:
    - build_job
  cache:
    paths:
      - node_modules/
  rules:
    - if: '$CI_COMMIT_REF_NAME == "main"'
    - if: '$CI_COMMIT_REF_NAME =~ /^feature\//'

deploy_job:
  stage: deploy
  script:
    - ./deploy.sh
  environment:
    name: production
    url: http://example.com
  only:
    - master
  when: manual

include:
  - local: 'templates/.gitlab-ci-template.yml'
```

This tutorial covers the essential commands and keywords used in GitLab CI/CD YAML files, providing explanations and examples for each concept.

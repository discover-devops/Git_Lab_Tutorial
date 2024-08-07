### What is a Pipeline?

- **Definition**: Pipelines are the top-level component of continuous integration, delivery, and deployment.
- **Structure**: A pipeline is a set of elements connected in series, where each set of elements performs specific tasks.
- **Functionality**: Pipelines automate the process of running tasks sequentially, from start to end, across different stages.

### Components of a Pipeline

1. **Jobs**:
   - **Basic Unit**: The most fundamental element of a pipeline.
   - **Function**: Defines what to do, such as testing or compiling code.
   - **Execution**: Jobs perform individual tasks within the pipeline.

2. **Stages**:
   - **Collection of Jobs**: Combines multiple jobs into a single unit.
   - **Order**: Defines the order of job execution within the stage.
   - **Progression**: If all jobs in a stage succeed, the pipeline moves to the next stage. If any job fails, the next stage is usually not executed, and the pipeline ends early.

### Characteristics of Pipelines

- **Automation**: Pipelines are typically executed automatically with no intervention once created.
- **Manual Interaction**: GitLab allows for manual interaction with a pipeline if needed.
- **Flexibility**: Pipelines can have multiple stages based on project requirements, such as test, build, staging, and production.

### Types of Pipelines

1. **Basic Pipeline**:
   - **Usage**: Suitable for straightforward projects.
   
2. **Directed Acyclic Graph (DAG) Pipeline**:
   - **Usage**: Ideal for large and complex projects.

3. **Parent-Child Pipeline**:
   - **Usage**: Organizes pipelines hierarchically for better management.
   
4. **Multi-Project Pipeline**:
   - **Usage**: Integrates pipelines across multiple projects.

### Creating a Simple Pipeline in GitLab

1. **Purpose**:
   - **Initial Setup**: Write a very simple pipeline to run the code present in the repo.
   - **Focus**: Introduction to pipeline definitions in GitLab without complicating with testing or building phases initially.

2. **Execution**:
   - **Job Definition**: Define a basic job in the `.gitlab-ci.yml` file.
   - **Pipeline Execution**: Run the pipeline to execute the defined job.

3. **Pipeline Example**:
   ```yaml
   stages:
     - stage1
   
   job1:
     stage: stage1
     script:
       - echo "This is a simple pipeline job"
   ```

- **Explanation**:
  - `stages`: Defines the stages in the pipeline.
  - `job1`: Defines a job named `job1`.
  - `stage`: Associates the job with `stage1`.
  - `script`: Contains the commands to be executed by the job.


Ref: https://docs.gitlab.com/ee/ci/pipelines/


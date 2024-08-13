# Accessing and Using GitLab Runners from the GitLab UI

## Introduction

In this section, we'll explore how to access and manage GitLab Runners through the GitLab UI, focusing on the concept of specific and shared runners. We'll also learn how to assign jobs to specific runners using tags and run a sample pipeline using a Windows-based shared runner.

## Accessing GitLab Runners

1. **Navigate to CI/CD Settings**:
   - To access the runners, start by navigating to your project on GitLab.
   - Go to `Settings` in the left-hand menu.
   - Under `Settings`, select `CI/CD`.
   - Scroll down to the `Runners` section.

2. **Understanding the Runners Section**:
   - In the `Runners` section, you'll see two categories:
     - **Specific Runners**: These runners are dedicated to your project or group.
     - **Shared Runners**: These are available for all projects within the GitLab instance. As of now, you might see a list of available shared runners, such as "15 shared runners are available."

3. **Runner Tags**:
   - Each runner may have tags associated with it. Tags are crucial for directing specific jobs to specific runners. 
   - For example, if a runner has the tag `docker`, it indicates that the runner is configured to handle Docker-related jobs.

## Using Tags to Select Runners

### Why Tags Matter

- **Job Selection**: When you define a job in your `.gitlab-ci.yml` file, you can add a `tags` directive. This tells GitLab to assign the job to a runner with a matching tag.
- **Runner Selection**: If a job has a tag `build`, GitLab will look for a runner with a `build` tag and assign the job to that runner.
- **Default Behavior**: If you don't specify any tags, GitLab will choose a runner from the pool of available runners.

### Example: Running a Pipeline on a Windows Runner

Let’s run a pipeline that utilizes a shared runner tagged with `windows`.

1. **Create a New Job in the `.gitlab-ci.yml` File**:
   - We’ll create a job named `windows-info`.
   - This job will use the `systeminfo` command to print detailed information about the Windows OS configuration on the runner.

```yaml
windows-info:
  stage: test
  script:
    - systeminfo
  tags:
    - windows
```

2. **Commit and Run the Pipeline**:
   - After adding the job to your `.gitlab-ci.yml` file, commit the changes.
   - The pipeline will start running, and the job will be assigned to a runner with the `windows` tag.

3. **Observe the Pipeline Execution**:
   - During execution, you’ll notice that the job is being run on a Windows-based shared runner.
   - The job output will include detailed system information about the runner.

4. **Identify the Runner**:
   - The pipeline UI will display the runner's ID and name, such as `on windows-shared-runners-manager`.
   - You can search for this runner in the shared runners list to view its details.

5. **View the Output**:
   - The `systeminfo` command will return the system configuration of the Windows host where the job was executed.
   - This information confirms that the job ran on the expected Windows runner.

## Disabling Shared Runners

- **Custom Runners**: If you prefer not to use shared runners, you can disable them from the CI/CD settings page. This forces GitLab to use only specific runners assigned to your project.

## Conclusion

In this tutorial, we explored how to access and manage GitLab Runners through the GitLab UI. We learned about the importance of runner tags in directing jobs to specific runners and ran a sample pipeline using a Windows-based shared runner. In the next lectures, we'll delve deeper into creating and configuring your own GitLab Runner on a local machine and running pipelines on it. Stay tuned!

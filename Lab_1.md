### step-by-step lab to create a Node.js project, run it locally, push it to a GitLab repository, and set up CI/CD with GitLab using a `.gitlab-ci.yml` file.

## Step-by-Step Lab

### Step 1: Create a Sample Node.js Application

1. **Install Node.js and npm**:
   - Ensure you have Node.js and npm installed on your Ubuntu machine. You can install them using the following commands:
     ```bash
     sudo apt update
     sudo apt install nodejs npm
     ```

2. **Create the Project Directory**:
   - Create a directory for your Node.js project and navigate into it:
     ```bash
     mkdir my-node-app
     cd my-node-app
     ```

3. **Initialize the Node.js Project**:
   - Initialize a new Node.js project using npm:
     ```bash
     npm init -y
     ```

4. **Install Express.js**:
   - Install the Express.js framework:
     ```bash
     npm install express
     ```

5. **Create the Application Files**:
   - Create a file named `app.js` and add the following sample code:
     ```javascript
     const express = require('express');
     const app = express();
     const port = 3000;

     app.get('/', (req, res) => {
       res.send('Hello World!');
     });

     app.listen(port, () => {
       console.log(`Example app listening at http://localhost:${port}`);
     });
     ```

6. **Update `package.json`**:
   - Update the `scripts` section of your `package.json` to include a start script:
     ```json
     "scripts": {
       "start": "node app.js"
     }
     ```

### Step 2: Run the Code Locally

1. **Start the Application**:
   - Start your Node.js application:
     ```bash
     npm start
     ```

2. **Access the Application**:
   - Open a web browser and navigate to `http://localhost:3000`. You should see "Hello World!" displayed.

### Step 3: Push the Code to GitLab

1. **Create a GitLab Repository**:
   - Log in to your GitLab account and create a new repository named `my-node-app`.

2. **Initialize Git and Add Remote**:
   - Initialize git in your project directory and add the remote repository:
     ```bash
     git init
     git remote add origin <your-gitlab-repo-url>
     ```

3. **Create and Checkout `projectA` Branch**:
   - Create and switch to a new branch named `projectA`:
     ```bash
     git checkout -b projectA
     ```

4. **Add, Commit, and Push Code**:
   - Add your project files, commit them, and push to the `projectA` branch:
     ```bash
     git add .
     git commit -m "Initial commit"
     git push -u origin projectA
     ```

### Step 4: Write `.gitlab-ci.yml` File

1. **Create `.gitlab-ci.yml`**:
   - In the root of your project directory, create a file named `.gitlab-ci.yml` and add the following content:

     ```yaml
     stages:
       - build
       - test
       - deploy

     variables:
       NODE_ENV: 'production'

     cache:
       paths:
         - node_modules/

     before_script:
       - apt-get update -y
       - apt-get install -y curl
       - curl -sL https://deb.nodesource.com/setup_14.x | bash
       - apt-get install -y nodejs
       - npm install

     build:
       stage: build
       script:
         - echo "Skipping build step for simplicity"

     test:
       stage: test
       script:
         - npm test

     deploy:
       stage: deploy
       script:
         - echo "Deploying to production environment"
     ```

2. **Configure Your GitLab Project**:
   - Ensure your GitLab project is configured to run CI/CD pipelines. This can usually be done from the project settings.

### Step 5: Explanation of Each Step in `.gitlab-ci.yml`

1. **Stages**:
   - Defines the stages of the pipeline: `build`, `test`, and `deploy`.

2. **Variables**:
   - Sets environment variables. Here, `NODE_ENV` is set to `production`.

3. **Cache**:
   - Caches the `node_modules` directory to speed up subsequent builds.

4. **Before Script**:
   - Runs before each job. Updates package lists, installs curl and Node.js, and installs project dependencies.

5. **Build Stage**:
   - Contains the `build` job, which currently skips the build step for simplicity.

6. **Test Stage**:
   - Contains the `test` job, which runs `npm test`. Make sure you have a test script defined in your `package.json`.

7. **Deploy Stage**:
   - Contains the `deploy` job, which currently only echoes a deployment message. You can expand this to deploy to your production environment.

### Step 6: Run the CI/CD Pipeline

1. **Push Changes to GitLab**:
   - Commit your `.gitlab-ci.yml` file and push it to the `projectA` branch:
     ```bash
     git add .gitlab-ci.yml
     git commit -m "Add .gitlab-ci.yml"
     git push origin projectA
     ```

2. **Monitor the Pipeline**:
   - Go to your GitLab project, navigate to CI/CD > Pipelines, and monitor the pipeline execution.

### Final Step: Enhance the Pipeline

1. **Expand Deployment Job**:
   - Update the `deploy` job to include actual deployment steps. This might involve SSHing into a server, copying files, or deploying to a cloud service.

2. **Add More Tests**:
   - Expand the `test` job to include unit tests, integration tests, and other necessary validations.

### Conclusion

You have now created a sample Node.js application, run it locally, pushed it to GitLab, and set up a CI/CD pipeline using `.gitlab-ci.yml`. 

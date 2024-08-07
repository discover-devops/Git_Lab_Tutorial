### Step-by-step lab to create a Python project for GitLab, run it on Ubuntu machine, and set up CI/CD with a `.gitlab-ci.yml` file.

### Step 1: Set Up Your Azure Ubuntu Machine
1. **Create an Azure Ubuntu VM:**
   - Follow the Azure portal to create an Ubuntu VM.
   - Ensure you have a public IP address assigned to the VM.

2. **Connect to Your Azure Ubuntu VM:**
   - Use SSH to connect to your VM:
     ```bash
     ssh <your-username>@<your-public-ip>
     ```

### Step 2: Install Necessary Software
1. **Update the Package List:**
   ```bash
   sudo apt update
   ```

2. **Install Python and pip:**
   ```bash
   sudo apt install python3 python3-pip -y
   ```

3. **Install Git:**
   ```bash
   sudo apt install git -y
   ```

### Step 3: Create a Sample Python Application
1. **Create a New Directory for Your Project:**
   ```bash
   mkdir my-python-app
   cd my-python-app
   ```

2. **Create a Simple Python Application:**
   - Create `app.py`:
     ```python
     from flask import Flask

     app = Flask(__name__)

     @app.route('/')
     def hello_world():
         return 'Hello, World!'

     if __name__ == '__main__':
         app.run(host='0.0.0.0', port=5000)
     ```

3. **Create a `requirements.txt` File:**
   ```bash
   echo "Flask" > requirements.txt
   ```

### Step 4: Run Your Python Application Locally
1. **Install Dependencies:**
   ```bash
   pip3 install -r requirements.txt
   ```

2. **Run the Application:**
   ```bash
   python3 app.py
   ```

3. **Access the Application:**
   - Open a web browser and go to `http://<your-public-ip>:5000`.

### Step 5: Push Code to GitLab
1. **Initialize a Git Repository:**
   ```bash
   git init
   ```

2. **Add Your Files:**
   ```bash
   git add .
   ```

3. **Commit Your Changes:**
   ```bash
   git commit -m "Initial commit"
   ```

4. **Create a New Repository on GitLab:**
   - Go to GitLab and create a new project named `my-python-app`.

5. **Add the Remote Repository and Push Your Code:**
   ```bash
   git remote add origin <your-gitlab-repo-url>
   git push -u origin master
   ```

6. **Create and Push to the `projectB` Branch:**
   ```bash
   git checkout -b projectB
   git push -u origin projectB
   ```

### Step 6: Write the `.gitlab-ci.yml` File
1. **Create the `.gitlab-ci.yml` File:**
   ```bash
   nano .gitlab-ci.yml
   ```

2. **Add the Following CI/CD Configuration:**
   ```yaml
   stages:
     - build
     - test
     - deploy

   build:
     stage: build
     script:
       - echo "Building the application..."
       - pip install -r requirements.txt
     artifacts:
       paths:
         - ./

   test:
     stage: test
     script:
       - echo "Running tests..."
       - python -m unittest discover

   deploy:
     stage: deploy
     script:
       - echo "Deploying the application..."
       - ssh <your-username>@<your-public-ip> "cd /path/to/your/project && git pull origin projectB && pip install -r requirements.txt && sudo systemctl restart my-python-app"
   ```

3. **Commit and Push the `.gitlab-ci.yml` File:**
   ```bash
   git add .gitlab-ci.yml
   git commit -m "Add CI/CD configuration"
   git push
   ```

### Step 7: Verify Your CI/CD Pipeline
1. **Go to Your GitLab Project:**
   - Navigate to the **CI/CD > Pipelines** section.

2. **Check the Pipeline Status:**
   - Ensure that your pipeline runs through the build, test, and deploy stages successfully.

### Step 8: Configure Systemd Service for Deployment
1. **Create a Systemd Service File on Your VM:**
   ```bash
   sudo nano /etc/systemd/system/my-python-app.service
   ```

2. **Add the Following Service Configuration:**
   ```ini
   [Unit]
   Description=My Python App

   [Service]
   User=<your-username>
   WorkingDirectory=/path/to/your/project
   ExecStart=/usr/bin/python3 /path/to/your/project/app.py
   Restart=always

   [Install]
   WantedBy=multi-user.target
   ```

3. **Reload Systemd and Start the Service:**
   ```bash
   sudo systemctl daemon-reload
   sudo systemctl start my-python-app
   sudo systemctl enable my-python-app
   ```

### Conclusion
You have successfully set up a Python project on your Azure Ubuntu machine, pushed the code to GitLab, configured a CI/CD pipeline, and automated the deployment process. This setup will help you manage and deploy your Python applications efficiently using GitLab CI/CD.

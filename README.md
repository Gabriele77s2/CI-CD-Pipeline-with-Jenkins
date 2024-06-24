# CI-CD-Pipeline-with-Jenkins
Set up a simple Continuous Integration/Continuous Deployment (CI/CD) pipeline using Jenkins. 
Use the pipeline to build, test, and deploy a sample application.

### Step 1: Install Jenkins

**Install Jenkins on your local machine or a server.** You can follow the official Jenkins installation guide [here](https://www.jenkins.io/doc/book/installing/).

### Step 2: Start Jenkins and Set Up

**Start Jenkins:**
```bash
# On a local machine
sudo systemctl start jenkins
```

**Access Jenkins:**
Open your browser and go to `http://localhost:8080` (or your server's IP if installed on a remote server).

**Unlock Jenkins:**
Follow the instructions on the screen to get the initial admin password, and then install the suggested plugins.

### Step 3: Create a Jenkins Pipeline

**Create a New Job:**
1. Click on "New Item" on the Jenkins dashboard.
2. Enter a name for your job, select "Pipeline", and click "OK".

**Configure the Pipeline:**
1. Scroll down to the "Pipeline" section.
2. Select "Pipeline script" and enter the following script:

```groovy
pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/your-username/your-repo.git'
            }
        }
        stage('Build') {
            steps {
                sh 'npm install'
            }
        }
        stage('Test') {
            steps {
                sh 'npm test'
            }
        }
        stage('Deploy') {
            steps {
                sh 'docker build -t your-dockerhub-username/your-repo:latest .'
                sh 'docker push your-dockerhub-username/your-repo:latest'
                sh 'kubectl apply -f deployment.yaml'
                sh 'kubectl apply -f service.yaml'
            }
        }
    }
}
```

**Note:**
- Replace `https://github.com/your-username/your-repo.git` with your repository URL.
- Ensure you have `deployment.yaml` and `service.yaml` files in your repository for Kubernetes deployment.

### Step 4: Configure Credentials

**Add DockerHub and Kubernetes Credentials:**
1. Go to "Manage Jenkins" -> "Manage Credentials".
2. Add credentials for DockerHub (username and password).
3. Add credentials for Kubernetes (if required).

### Step 5: Trigger the Pipeline

**Manual Trigger:**
1. Go to your job dashboard.
2. Click "Build Now" to trigger the pipeline manually.

**Automated Trigger:**
1. Go to your job configuration.
2. Scroll to the "Build Triggers" section.
3. Check "GitHub hook trigger for GITScm polling" to enable builds on code commits.

### Step 6: Monitor and Review

**Monitor Build Progress:**
1. Go to your job dashboard.
2. Click on the build number to view logs and monitor the progress.

**Review Test Results and Deployment:**
1. Ensure that each stage completes successfully.
2. Check the deployed application on your Kubernetes cluster.

### Step 7: Clean Up

**Stop Jenkins:**
```bash
# On a local machine
sudo systemctl stop jenkins
```

**Remove Docker Images and Containers:**
```bash
docker system prune -a
```

**Remove Kubernetes Deployments:**
```bash
kubectl delete -f deployment.yaml
kubectl delete -f service.yaml
```

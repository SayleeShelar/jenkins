# Jenkins Complete Guide for Beginners 🚀

A comprehensive guide to learning Jenkins from scratch using Docker. This README covers every concept a beginner needs to understand and implement Jenkins CI/CD pipelines.

---

## Table of Contents

1. [What is Jenkins?](#what-is-jenkins)
2. [Why Use Jenkins?](#why-use-jenkins)
3. [Prerequisites](#prerequisites)
4. [Installing Docker](#installing-docker)
5. [Running Jenkins from Docker](#running-jenkins-from-docker)
6. [Jenkins Dashboard Overview](#jenkins-dashboard-overview)
7. [Creating Your First Jenkins Job](#creating-your-first-jenkins-job)
8. [Understanding Jenkins Pipelines](#understanding-jenkins-pipelines)
9. [Creating Your First Jenkinsfile](#creating-your-first-jenkinsfile)
10. [Building a Real-World Pipeline](#building-a-real-world-pipeline)
11. [Common Issues and Troubleshooting](#common-issues-and-troubleshooting)
12. [Next Steps](#next-steps)

---

## What is Jenkins?

**Jenkins** is an open-source automation server that helps automate the building, testing, and deployment of software projects. It's the most popular CI/CD (Continuous Integration/Continuous Deployment) tool used by developers worldwide.

### Key Terms You Need to Know:

| Term | Meaning |
|------|---------|
| **CI (Continuous Integration)** | Automatically building and testing code every time changes are pushed to the repository |
| **CD (Continuous Deployment/Delivery)** | Automatically deploying tested code to production or staging environments |
| **Pipeline** | A collection of steps that define the CI/CD process |
| **Job/Project** | A configured task that Jenkins executes |
| **Build** | The process of compiling source code into a usable application |

---

## Why Use Jenkins?

- ✅ **Free and Open Source** - No licensing costs
- ✅ **Large Plugin Ecosystem** - 1800+ plugins available
- ✅ **Easy to Set Up** - Simple installation process
- ✅ **Cross-Platform** - Works on Windows, Linux, macOS
- ✅ **Strong Community** - Extensive documentation and support
- ✅ **Docker Support** - Native integration with Docker
- ✅ **Flexible** - Highly customizable workflows

---

## Prerequisites

Before starting with Jenkins, make sure you have:

### 1. Basic Requirements:
- A computer with at least 4GB RAM (8GB recommended)
- 50GB+ free disk space
- Administrative access to install software

### 2. Required Software:
- **Docker Desktop** - For running Jenkins in a container
- **Git** - For version control
- **Code Editor** - VS Code, Sublime Text, or any editor

### 3. Knowledge Prerequisites (Basic):
- Understanding of command line/terminal
- Basic programming knowledge
- Familiarity with version control (Git)

---

## Installing Docker

### Windows Installation:

1. **Download Docker Desktop:**
   - Go to [Docker Desktop for Windows](https://www.docker.com/products/docker-desktop)
   - Download the installer

2. **Install Docker Desktop:**
   - Run the downloaded `.exe` file
   - Follow the installation wizard
   - Enable WSL 2 if prompted (recommended)

3. **Verify Installation:**
   
```
cmd
   docker --version
   docker-compose --version
   
```

4. **Start Docker:**
   - Launch Docker Desktop from the Start menu
   - Wait for it to show "Docker is running"

### macOS Installation:

1. **Download Docker Desktop:**
   - Go to [Docker Desktop for Mac](https://www.docker.com/products/docker-desktop)
   - Download the Apple Silicon or Intel version

2. **Install Docker:**
   - Open the `.dmg` file
   - Drag Docker to Applications folder
   - Launch Docker from Applications

3. **Verify Installation:**
   
```
bash
   docker --version
   docker-compose --version
   
```

### Linux Installation (Ubuntu):

```
bash
# Update package index
sudo apt-get update

# Install prerequisites
sudo apt-get install -y apt-transport-https ca-certificates curl software-properties-common

# Add Docker's official GPG key
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

# Add Docker repository
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Install Docker
sudo apt-get update
sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-compose-plugin

# Start Docker
sudo systemctl start docker
sudo systemctl enable docker

# Verify installation
docker --version
```

---

## Running Jenkins from Docker

### Step 1: Create Docker Network (Optional but Recommended)

```
bash
docker network create jenkins_network
```

### Step 2: Run Jenkins Container

#### Basic Method (All-in-one command):

```
bash
# Run Jenkins in Docker (first time setup)
docker run -d \
  --name jenkins \
  -p 8080:8080 \
  -p 50000:50000 \
  -v jenkins_home:/var/jenkins_home \
  jenkins/jenkins:lts
```

#### Advanced Method (With Docker socket - for building Docker images inside Jenkins):

```
bash
# Run Jenkins with Docker socket mounting
docker run -d \
  --name jenkins \
  -p 8080:8080 \
  -p 50000:50000 \
  -v jenkins_home:/var/jenkins_home \
  -v /var/run/docker.sock:/var/run/docker.sock \
  jenkins/jenkins:lts
```

### Parameter Explanation:

| Parameter | Meaning |
|-----------|---------|
| `-d` | Run container in detached mode (background) |
| `--name jenkins` | Name the container "jenkins" |
| `-p 8080:8080` | Map port 8080 (host) to 8080 (container) - Web UI |
| `-p 50000:50000` | Map port 50000 for Jenkins agent communication |
| `-v jenkins_home:/var/jenkins_home` | Persist Jenkins data in a Docker volume |
| `-v /var/run/docker.sock:/var/run/docker.sock` | Allow Jenkins to build Docker images |

### Step 3: Get Initial Admin Password

After starting Jenkins, wait about 1-2 minutes for it to initialize, then:

```
bash
# Method 1: View logs to get password
docker logs jenkins

# Method 2: Get password from container
docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword
```

### Step 4: Access Jenkins

1. Open your browser
2. Go to: `http://localhost:8080`
3. You'll see the "Unlock Jenkins" screen
4. Enter the initial admin password you obtained

### Step 5: Complete Setup Wizard

1. **Install Suggested Plugins** - Click "Install suggested plugins"
2. **Create First Admin User** - Fill in your details
3. **Instance Configuration** - Keep default (http://localhost:8080)
4. **Click "Save and Finish"**
5. **Click "Start using Jenkins"**

🎉 **Congratulations! Jenkins is now running!**

---

## Jenkins Dashboard Overview

When you first log in, you'll see the Jenkins dashboard. Here's what each section means:

### Main Components:

```
┌─────────────────────────────────────────────────────────────┐
│  JENKINS                                                      │
│  ┌──────────────┬──────────────────────────────────────────┐│
│  │ New Item     │  Welcome to Jenkins!                     ││
│  │ People       │                                          ││
│  │ Build History│  To get started:                         ││
│  │ Credentials  │  • Create a new job                      ││
│  │ My Views     │  • Configure Jenkins                     ││
│  │ Manage Jenkins│ • Manage Plugins                        ││
│  └──────────────┴──────────────────────────────────────────┘│
└─────────────────────────────────────────────────────────────┘
```

### Sidebar Menu:

| Menu Item | Purpose |
|-----------|---------|
| **New Item** | Create a new job/project |
| **People** | View users and their builds |
| **Build History** | View past build logs and status |
| **Credentials** | Manage passwords, SSH keys, etc. |
| **My Views** | Create custom views |
| **Manage Jenkins** | Configure Jenkins settings |
| **Manage Plugins** | Install/update plugins |

---

## Creating Your First Jenkins Job

Let's create a simple "Hello World" job to understand how Jenkins works.

### Step 1: Create New Item

1. Click **"New Item"** in the sidebar
2. Enter job name: `My-First-Job`
3. Select **"Freestyle project"**
4. Click **OK**

### Step 2: Configure the Job

**General Section:**
- Description: "My first Jenkins job"

**Build Steps Section:**
1. Click **"Add build step"**
2. Select **"Execute Windows batch command"** (Windows) or **"Execute shell"** (Linux/Mac)
3. Enter the following commands:

```
bash
echo "Hello from Jenkins!"
echo "Today's date is: $(date)"
echo "Building project: My-First-Job"
```

### Step 3: Save and Build

1. Click **"Save"**
2. On the job page, click **"Build Now"**
3. Watch the build execute in the "Build History" section
4. Click on the build number (e.g., #1) to see the console output

### Step 4: View Build Output

1. Click on the completed build
2. Click **"Console Output"**
3. You should see your echo commands executed successfully!

🎉 **You've created and run your first Jenkins job!**

---

## Understanding Jenkins Pipelines

### What is a Pipeline?

A pipeline is a way to define your entire CI/CD process as code. Instead of clicking through a GUI, you write a Jenkinsfile that describes your build process.

### Pipeline Types:

1. **Declarative Pipeline** - Newer, easier to write and understand
2. **Scripted Pipeline** - More flexible, uses Groovy syntax

### Why Use Pipelines?

- ✅ **Version Controlled** - Store pipeline code in Git
- ✅ **Reproducible** - Same pipeline runs the same every time
- ✅ **Visualization** - See pipeline stages visually
- ✅ **Resumable** - Pipelines can be resumed after failures
- ✅ **Parallel Execution** - Run stages in parallel

---

## Creating Your First Jenkinsfile

A Jenkinsfile is a text file that defines your pipeline. Let's create a simple one:

### Example 1: Basic Declarative Pipeline

```
groovy
pipeline {
    agent any
    
    stages {
        stage('Build') {
            steps {
                echo 'Building the application...'
                sh 'echo "Build complete!"'
            }
        }
        
        stage('Test') {
            steps {
                echo 'Running tests...'
                sh 'echo "Tests passed!"'
            }
        }
        
        stage('Deploy') {
            steps {
                echo 'Deploying application...'
                sh 'echo "Deployment complete!"'
            }
        }
    }
}
```

### Example 2: Pipeline with Git and Variables

```
groovy
pipeline {
    agent any
    
    environment {
        APP_NAME = 'my-application'
        BUILD_VERSION = '1.0.0'
    }
    
    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out source code...'
                git branch: 'main', url: 'https://github.com/your-repo/your-project.git'
            }
        }
        
        stage('Build') {
            steps {
                echo "Building ${APP_NAME} version ${BUILD_VERSION}"
                sh 'npm install'
                sh 'npm run build'
            }
        }
        
        stage('Test') {
            steps {
                echo 'Running tests...'
                sh 'npm test'
            }
        }
        
        stage('Deploy') {
            steps {
                echo 'Deploying to production...'
            }
        }
    }
    
    post {
        always {
            echo 'Cleaning up...'
        }
        success {
            echo 'Build and deployment successful!'
        }
        failure {
            echo 'Build failed!'
        }
    }
}
```

### Key Jenkinsfile Concepts:

| Section | Purpose |
|---------|---------|
| `agent` | Where to run the pipeline (any, docker, specific node) |
| `environment` | Define environment variables |
| `stages` | Collection of stage definitions |
| `stage` | A logical grouping of steps |
| `steps` | Actual commands to execute |
| `post` | Actions to run after pipeline completes |

---

## Building a Real-World Pipeline

Let's build a complete CI/CD pipeline for a Node.js application.

### Prerequisites:

1. Create a sample Node.js project:
   
```
bash
   mkdir my-app && cd my-app
   npm init -y
   npm install express
   
```

2. Create an `index.js` file:
   
```
javascript
   const express = require('express');
   const app = express();
   
   app.get('/', (req, res) => {
     res.send('Hello from Jenkins CI/CD Pipeline!');
   });
   
   app.listen(3000, () => {
     console.log('App running on port 3000');
   });
   
```

3. Create a `package.json`:
   
```
json
   {
     "name": "my-app",
     "version": "1.0.0",
     "main": "index.js",
     "scripts": {
       "start": "node index.js",
       "test": "echo \"Tests passed\" && exit 0"
     },
     "dependencies": {
       "express": "^4.18.2"
     }
   }
   
```

### Complete Jenkinsfile:

```
groovy
pipeline {
    agent {
        docker {
            image 'node:18-alpine'
            args '-p 3000:3000'
        }
    }
    
    environment {
        APP_NAME = 'my-application'
        BUILD_VERSION = "${env.BUILD_NUMBER}"
    }
    
    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out source code...'
                checkout scm
            }
        }
        
        stage('Install Dependencies') {
            steps {
                echo 'Installing dependencies...'
                sh 'npm install'
            }
        }
        
        stage('Lint') {
            steps {
                echo 'Running linter...'
                sh 'echo "Linting complete"'
            }
        }
        
        stage('Test') {
            steps {
                echo 'Running tests...'
                sh 'npm test'
            }
        }
        
        stage('Build') {
            steps {
                echo 'Building application...'
                sh 'echo "Build complete"'
            }
        }
        
        stage('Security Scan') {
            steps {
                echo 'Running security scan...'
                sh 'echo "Security scan complete"'
            }
        }
        
        stage('Deploy to Staging') {
            when {
                branch 'develop'
            }
            steps {
                echo 'Deploying to staging environment...'
            }
        }
        
        stage('Deploy to Production') {
            when {
                branch 'main'
            }
            steps {
                echo 'Deploying to production environment...'
            }
        }
    }
    
    post {
        always {
            echo 'Cleaning workspace...'
            cleanWs()
        }
        success {
            echo '🎉 Pipeline completed successfully!'
            emailext (
                subject: "SUCCESS: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: "Build #${env.BUILD_NUMBER} completed successfully!",
                to: 'developer@example.com'
            )
        }
        failure {
            echo '❌ Pipeline failed!'
            emailext (
                subject: "FAILED: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: "Build #${env.BUILD_NUMBER} failed. Check console output.",
                to: 'developer@example.com'
            )
        }
    }
}
```

---

## Common Issues and Troubleshooting

### Issue 1: Jenkins Container Won't Start

**Problem:** Container exits immediately after starting

**Solution:**
```
bash
# Check container logs
docker logs jenkins

# Remove old container and try again
docker rm -f jenkins
docker run -d --name jenkins -p 8080:8080 -v jenkins_home:/var/jenkins_home jenkins/jenkins:lts
```

### Issue 2: Initial Admin Password Not Showing

**Problem:** Getting blank password or file not found

**Solution:**
```
bash
# Wait 2-3 minutes for Jenkins to fully initialize
# Then try:
docker exec -it jenkins cat /var/jenkins_home/secrets/initialAdminPassword
```

### Issue 3: Port 8080 Already in Use

**Problem:** Another application is using port 8080

**Solution:**
```
bash
# Use a different port
docker run -d --name jenkins -p 9090:8080 -v jenkins_home:/var/jenkins_home jenkins/jenkins:lts

# Access Jenkins at http://localhost:9090
```

### Issue 4: Docker in Docker Not Working

**Problem:** Can't build Docker images inside Jenkins

**Solution:**
```
bash
# Use Docker socket approach
docker rm -f jenkins

docker run -d \
  --name jenkins \
  -p 8080:8080 \
  -p 50000:50000 \
  -v jenkins_home:/var/jenkins_home \
  -v /var/run/docker.sock:/var/run/docker.sock \
  --group-add $(getent group docker | cut -d: -f3) \
  jenkins/jenkins:lts

# Add jenkins user to docker group
docker exec -it jenkins usermod -aG docker jenkins
```

### Issue 5: Plugins Failing to Install

**Problem:** Plugin installation timeout or failure

**Solution:**
1. Go to **Manage Jenkins** → **Manage Plugins**
2. Click **Advanced** tab
3. Update URL: `https://updates.jenkins.io/update-center.json`
4. Try installing again

### Issue 6: Build Failing Due to Memory

**Problem:** Out of memory errors during build

**Solution:**
```
groovy
// Add to Jenkinsfile
pipeline {
    agent {
        docker {
            image 'node:18-alpine'
            args '-e NODE_OPTIONS="--max-old-space-size=4096"'
        }
    }
    // ...
}
```

---

## Docker Commands Reference

Here are essential Docker commands for managing Jenkins:

```
bash
# Start Jenkins
docker start jenkins

# Stop Jenkins
docker stop jenkins

# Restart Jenkins
docker restart jenkins

# View Jenkins logs
docker logs -f jenkins

# Access Jenkins container shell
docker exec -it jenkins /bin/bash

# Get initial admin password
docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword

# Check Jenkins status
docker ps

# Remove Jenkins container (data preserved)
docker rm -f jenkins

# Remove Jenkins data (permanent deletion)
docker volume rm jenkins_home

# List all Docker containers
docker ps -a

# List all Docker volumes
docker volume ls
```

---

## Jenkins Commands Reference (Jenkins CLI)

```
bash
# Build a job
curl -X POST http://localhost:8080/job/YOUR_JOB/build

# Get job information
curl http://localhost:8080/job/YOUR_JOB/api/json

# Trigger parameterized build
curl -X POST http://localhost:8080/job/YOUR_JOB/buildWithParameters \
  -d param1=value1 -d param2=value2
```

---

## Next Steps

Now that you've learned the basics, here are topics to explore next:

### Advanced Topics:

1. **Jenkins Shared Libraries** - Reusable pipeline code
2. **Blue Ocean** - Modern Jenkins UI
3. **Jenkins X** - Cloud-native Jenkins
4. **Kubernetes Integration** - Running Jenkins on K8s
5. **Pipeline as Code** - Advanced Jenkinsfile patterns
6. **Security** - Securing your Jenkins installation
7. **Distributed Builds** - Master-Slave architecture

### Useful Plugins to Explore:

| Plugin | Purpose |
|--------|---------|
| Docker Pipeline | Build and use Docker in pipelines |
| GitHub Integration | Trigger builds on GitHub events |
| Pipeline Stage View | Visual pipeline representation |
| Blue Ocean | Modern UI for pipelines |
| Credentials Binding | Manage secure credentials |
| Email Extension | Advanced email notifications |

---

## Quick Reference Card

```
┌────────────────────────────────────────────────────────────┐
│                  JENKINS QUICK REFERENCE                   │
├────────────────────────────────────────────────────────────┤
│  URL:              http://localhost:8080                  │
│  Default Port:     8080 (Web), 50000 (Agent)               │
│  Config Dir:       /var/jenkins_home                       │
│  Initial Password: /var/jenkins_home/secrets/              │
│                    initialAdminPassword                   │
│  Log Location:     /var/jenkins_home/logs/                │
├────────────────────────────────────────────────────────────┤
│  Common Commands:                                          │
│  • docker run -d -p 8080:8080 jenkins/jenkins:lts         │
│  • docker logs jenkins                                    │
│  • docker exec -it jenkins /bin/bash                     │
└────────────────────────────────────────────────────────────┘
```

---

## Contributing

Feel free to contribute to this guide! Fork the repository and submit a pull request.

---

## License

This project is licensed under the MIT License.

---

## Resources

- [Jenkins Official Documentation](https://www.jenkins.io/doc/)
- [Jenkins Wiki](https://wiki.jenkins.io/)
- [Jenkins CLI](https://www.jenkins.io/doc/book/managing/cli/)
- [Pipeline Syntax](https://www.jenkins.io/doc/book/pipeline/syntax/)
- [Docker Documentation](https://docs.docker.com/)

---

**Happy Learning! 🎉**

If you found this guide helpful, please ⭐ star this repository and share it with others!

---

*Last Updated: 2024*
*Author: Jenkins Beginner Guide*

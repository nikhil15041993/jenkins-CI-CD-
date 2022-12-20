## Automate Docker builds using Jenkins Pipelines | Dockerize Python App | Upload Images into AWS ECR

We will learn how to automate Docker builds using Jenkins. We will use Python based application. I have already created a repo with source code + Dockerfile. We will see how to create Docker image and upload into AWS ECR successfully. We will not be using AWS access keys to upload image into ECR, we will be using IAM role and attach to Jenkins instance to access ECR.

Pre-requistes:

1. Jenkins is up and running
2. Docker installed on Jenkins instance.
3. Docker and Docker pipelines plug-in are installed
4. Repo created in ECR
5. Make sure port 8096 is opened up in firewall rules. 
6. Create an IAM role with AmazonEC2ContainerRegistryFullAccess policy, attach to Jenkins EC2 instance
7. Make sure AWS cli is installed in Jenkins instance.


### Step 1 - Create a pipeline in Jenkins, name can be anything

### Step 2 - Copy the pipeline code from below

Your account_d should be updated and repo should be updated.


```

pipeline {
    agent any
    environment {
        registry = "acct_id.dkr.ecr.us-east-2.amazonaws.com/your_ecr_repo"
    }
   
    stages {
        stage('Cloning Git') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '', url: 'https://github.com/akannan1087/myPythonDockerRepo']]])     
            }
        }
  
    // Building Docker images
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build registry
        }
      }
    }
   
    // Uploading Docker images into AWS ECR
    stage('Pushing to ECR') {
     steps{  
         script {
                sh 'aws ecr get-login-password --region us-east-2 | docker login --username AWS --password-stdin acct_id.dkr.ecr.us-east-2.amazonaws.com'
                sh 'docker push acct_id.dkr.ecr.us-east-2.amazonaws.com/your_ecr_repo:latest'
         }
        }
      }
   
         // Stopping Docker containers for cleaner Docker run
     stage('stop previous containers') {
         steps {
            sh 'docker ps -f name=mypythonContainer -q | xargs --no-run-if-empty docker container stop'
            sh 'docker container ls -a -fname=mypythonContainer -q | xargs -r docker container rm'
         }
       }
      
    stage('Docker Run') {
     steps{
         script {
                sh 'docker run -d -p 8096:5000 --rm --name mypythonContainer acct_id.dkr.ecr.us-east-2.amazonaws.com/your_ecr_repo:latest'
            }
      }
    }
    }
}

```

### Step 3 - Click on Build - Build the pipeline

Once you create the pipeline and changes values per your ECR account ID, click on Build now.


### Steps 5 - Access PythonApp in the browser which is running inside docker container

Once build is successful, go to browser and enter http://public_dns_name:8096

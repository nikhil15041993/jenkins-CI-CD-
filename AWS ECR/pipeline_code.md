```

pipeline{
    
    agent any
    
    
     environment {
        registry = "442357423419.dkr.ecr.ap-south-1.amazonaws.com/myrepo "
        def myVariable = "442357423419.dkr.ecr.ap-south-1.amazonaws.com/myrepo"
    }
    
    stages{
        
        
        
        stage("git")
        {
            steps
            {
            git 'https://github.com/akannan1087/myPythonDockerRepo.git'    
            }
        }
        
        
        stage("sens artifate to server")
        {
            steps
            {
                sshPublisher(publishers: [sshPublisherDesc(configName: 'ec2', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: '', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: 'workspace/aws-ecr', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '**/*')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
            }
        }
        
        
        
        
        stage('Building image')
        {
            agent { 
                label 'ec2'
            }
            
           steps
           {
                script
                {
                    sh 'docker build -t ${myVariable}:$BUILD_ID .'
                }
            }
        }
            
            
        stage('Pushing to ECR') 
           {
                 agent { 
                  label 'ec2'
                  }
                steps
                {  
                    script
                    {
                        sh 'aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin 442357423419.dkr.ecr.ap-south-1.amazonaws.com'
                        sh 'docker push ${myVariable}:$BUILD_ID'
                    }
                }
            }
            
            
        stage('stop previous containers') {
        agent { label 'ec2' }
        steps {
            sh 'docker ps -f name=mypythonContainer -q | xargs --no-run-if-empty docker container stop'
            sh 'docker container ls -a -fname=mypythonContainer -q | xargs -r docker container rm'
         }
       }
       


   
       
       stage('Docker Run') {
            agent { 
                  label 'ec2'
                  }
     steps{
         script {
                sh 'docker run -d -p 8096:5000 --rm --name mypythonContainer ${myVariable}:$BUILD_ID'
            }
      }
    }
            
        }
    
    
    
    }
    
    ```
    
    
    

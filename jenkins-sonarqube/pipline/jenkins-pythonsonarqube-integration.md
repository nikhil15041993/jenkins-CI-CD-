## using this script we can scan our code using sonarqube scanner on jenkins pipeline.


```
pipeline{
    agent any
    stages{
        stage("scm"){
            steps{
                git 'https://github.com/nikhil15041993/terminalnet-123.git'
            }
        }
               
        
    stage('SonarQube Analysis') {
    steps{
        script{
        def scannerHome = tool 'sonarqube';
        }
        
      withSonarQubeEnv('sonarqube_token') {
      sh """/var/lib/jenkins/tools/hudson.plugins.sonar.SonarRunnerInstallation/sonarqube/bin/sonar-scanner \
        -D sonar.projectVersion=1.0-SNAPSHOT \
        -D sonar.login=admin \
        -D sonar.password=admin \
        -D sonar.projectBaseDir=/var/lib/jenkins/workspace/sonarqube-scaner/ \
        -D sonar.projectKey=project \
        -D sonar.sourceEncoding=UTF-8 \
        -D sonar.host.url=http://128.199.21.88:9000"""
        }
        
}
}
}
}

```

In this script the **scannerHome** is geting from SonarQube Scanner on global configuration tool on jenkins.

**sonarqube_token** is getting from SonarQube servers part on Configure System part on jenkins.


also we can disabel the
```
   script{
        def scannerHome = tool 'sonarqube';
        }
```
part if we install it automatically via jenkins ( same as maven we did )


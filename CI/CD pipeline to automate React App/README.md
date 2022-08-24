## Setup a CI/CD pipeline to automate React App deployment on AWS ec2

### Prerequisites
* AWS ec2 instance running an Ubuntu Server AMI
* Java 8 and Jenkins server installed
* React application created using create-react-app and pushed to a Github repository.


## Setting up the environment

### install Nginx webserver

```
$ sudo apt install -y nginx
```
After installing the web server, we have to create the root folder to serve our files.
```
$ sudo mkdir /var/www/jenkins-react-app
```
After completing the above steps, we need to create the configuration file for the Nginx server.

create the file /etc/nginx/sites-available/jenkins-react-app and paste the following code
```
server {

  listen 3000;
  server_name <ip address>;
  root /var/www/jenkins-react-app;
  index index.html;
  
  access_log /var/log/nginx/jenkins-react-app.log;
  error_log /var/log/nginx/jenkins-react-app.error.log;
  location / {
    try_files $uri /index.html =404;
  }
}
```

The above configuration will setup the Nginx server to listen on port 3000 and serve the files from the directory which we created earlier. You have to change the server_name attribute to the public IP address of your ec2 instance. You can also change the port 3000 to your required port. Make sure that you allow the specified port in the security group of your instance.


```
Note : If you change the port, make sure you add a file named .env to the root folder of the react app and insert the line PORT=YOUR_NEW_PORT_HERE because the default port of create-react-app is 3000
```

Now we have to create a Symlink to the above configuration in /etc/nginx/sites-enabled

```
$ sudo ln -s /etc/nginx/sites-available/jenkins-react-app /etc/nginx/sites-enabled/jenkins-react-app
```
Alright, all configurations are done, let’s spin up the webserver.
```
$ sudo service nginx start
```

## Setting up the Jenkins CI/CD pipeline

Open your jenkins server and Click on New Item, then enter a job name and select Pipeline from the options.

After creating the Pipeline, under Build Triggers tick the GitHub hook trigger for GITScm polling option.

Under the Pipeline section, select and fill out the fields as shown below.





### Now let’s create the Jenkinsfile.

```
pipeline {
     agent any
     stages {
        stage("Build") {
            steps {
                sh "sudo npm install"
                sh "sudo npm run build"
            }
        }
        stage("Deploy") {
            steps {
                sh "sudo rm -rf /var/www/jenkins-react-app"
                sh "sudo cp -r ${WORKSPACE}/build/ /var/www/jenkins-react-app/"
            }
        }
    }
}
```
As you can see we are executing shell command with superuser privileges, we cannot do that with the Jenkins user or ther users. So we need to grant the privileges to execute commands as superuser. To do that, open /etc/sudoers file and insert the below lines at the end.

```
jenkins ALL=(ALL) NOPASSWD: /usr/bin/npm install
jenkins ALL=(ALL) NOPASSWD: /usr/bin/npm run build
jenkins ALL=(ALL) NOPASSWD: /bin/rm -rf /var/www/jenkins-react-app
jenkins ALL=(ALL) NOPASSWD: /bin/cp -r /var/lib/jenkins/workspace/jenkins-react-app/build/ /var/www/jenkins-react-app/
```

Adding the above lines will grant permission to the jenkins user to execute the commands which are after the keyword NOPASSWD: without a password. Any other command not listed here will fail to execute as superuser unless the password is provided. Please make sure that the directory in the rm and cp command is the same as the ones we wrote in the Jenkinsfile above.


To automatically trigger our Jenkins job, we have to add a Github webhook to our repository, let’s go ahead and add it.

Go to the settings of your Github repository and go to the Webhooks section. Enter the payload URL, i.e. by default http://{external_dns_hostname}:{jenkins port}/github-webhook/

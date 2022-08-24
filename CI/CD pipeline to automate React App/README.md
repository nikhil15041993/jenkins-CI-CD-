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


 

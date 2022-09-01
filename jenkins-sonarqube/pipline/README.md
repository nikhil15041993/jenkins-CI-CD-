
### In this example we’ll:

* get Jenkins and SonarQube up and running
* install the SonarQube Scanner Jenkins plugin and configure it to point to our SonarQube instance
* configure SonarQube to call the Jenkins webhook when project analysis is finished
* create  Jenkins pipelines
* run the pipelines and see it all working


### Step 1 . Run Jenkins and SonarQube Servers

Once Run jenkins server head over to Manage Jenkins > Manage Plugins > Available and search for sonar. Select the SonarQube Scanner and Quality Gate plugin and click Install without restart.

Once the plugin is installed, let’s configure it!

Go to Manage Jenkins > Configure System and scroll down to the SonarQube servers section. This is where we’ll add details of our SonarQube server so Jenkins can pass its details to our project’s build when we run it.

Click the Add SonarQube button. Now add a Name for the server, such as ```SonarQube```. The Server URL will be ```http://sonarqubeserver-ip:9000``` Remember to click Save.
Set autentication token. (token from sonarqube server project)

### Configuring SonarQube

Let’s jump over to SonarQube. Click Log in at the top-right of the page, and log in with the default credentials of admin/admin. You’ll then have to set a new password.

Now go to Administration > Configuration > Webhooks. This is where we can add webhooks that get called when project analysis is completed. In our case we need to configure SonarQube to call ```Jenkins``` to let it know the results of the analysis.

Click Create, and in the popup that appears give the webhook a name of Jenkins, set the URL to ```http://jenkins-ip:8080/sonarqube-webhook``` and click Create.



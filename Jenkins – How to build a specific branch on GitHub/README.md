# Jenkins – How to build a specific branch on GitHub


## Step 1: Install Generic Webhooks Trigger Plugin

Login to your Jenkins installation setup and navigate to Manage Jenkins >> Manage Plugins and click on the Available tab and install Generic Webhook Trigger Plugin.

Restart Jenkins to use the plugin.

## Step 2: Setup GitHub Personal Access Token
Login to your GitHub account and go to your account Settings >> Developer Settings >> Personal access tokens.

Click Generate new token.

Enter a note in the Note field.

Select repo.

Click Generate token.

Now a new token will be visible for this one time.

Copy the generated token.


## Step 3: Setup Webhooks in Github

* Login to your GitHub account and go to the repository that is configured in Jenkins. Click on Settings >> Webhooks.

* In the Payload URL enter the the url in the following format.

* Replace JENKINS_URL with your own URL.

* Replace YOUR_TOKEN with the personal access token you created in the previous step.
```
https://JENKINS_URL/generic-webhook-trigger/invoke?token=YOUR_TOKEN
```
* In the Content type select application/json.

* In the Which events choose Just the push event.

* Click Add webhook.


## Step 4: Configure Jenkins to build only specific branch

* Once you have your Webhook created you can configure Jenkins to trigger build only if a push is made to the specific repository.

* This configuration is for Freestyle Project in Jenkins.

* Login to your Jenkins installation and go you your job and click configure.

* In the Source Code Management section under Build Triggers check the Generic Webhook Trigger.

* Click Add next to the Post content parameters.

* Once a push is made, GitHub passes the branch name in the JSON format with the ref key.

The example payload will look something as shown below.

```
{
   "ref": "refs/heads/branch_name",
   "before": "5b6f5bfbb31d9d28f5655cs8753gae308ef392c5",
   "after": "0000000000000000000000000000000000000000",
   "repository": { 

```

* In the Variable enter the name of the variable as ref.

* In the Expression enter $.ref to match the key and choose the JSONPath format of payload.

* In the Token field enter the personal access token you created before.

Next we can configure Optional Filter.

* In the Expression field enter the branch name to match the key ref as shown below.

```
refs/heads/branch_name\srepository
```

* In the Text field enter the variable name you assigned before as $ref $repository .

Save the configuration.


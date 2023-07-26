## Automated Daily Backups of Jenkins Master to Amazon S3 Bucket

### Prerequisites
Amazon EC2 Linux Instance with Jenkins installed

Steps
* Download AWS CLI on jenkins server
* Create S3 Bucket and IAM User
* Configure Jenkins Environment Variables and Job

### 1 Download AWS CLI to EC2 Instance

```
sudo apt install awscli
```
If successful the aws --version command should output the aws-cli version you have installed.
```
$:aws --version
aws-cli/1.11.13
```


### 2. Create S3 Bucket and IAM User

Next we need to create an S3 bucket to push our Jenkins backups to. In your AWS account navigate to S3 and create a new bucket.

### 3. Creating an IAM user

For us to be able to upload to S3 we will need an IAM user. This can be found under Security, Identity and Compliance in your AWS console. Give it a username and check ‘Programmatic access’.

In step 2 add the user to the default ‘admin’ group.

### 4. Configure Jenkins Environment Variables and Job

Lets add our environment variables to our Global properties so that we can upload to our S3 bucket. In your Jenkins Master, navigate to;
```
Manage Jenkins > Configure System > Global Properties > Environment Variables.
```
```
eg: 
NAME=AWS_ACCESS_KEY_ID
VALUE=<YOUR_ACCESS_KEY_ID>

Now that we have our IAM user added to our environment we will be able to successfully upload to S3, lets create a new Jenkins job. Click ‘New item’ from the left nav panel of Jenkins.

Lets call this job ‘jenkins-backup’ and choose ‘Freestyle project’ for project type.


We only have 2 configurations we need to add for the ‘jenkins-backup’ job;
```
Build Triggers > Build Periodically
Build > Execute Shell
```
First we want to setup our CRON job. Go to ‘Build Triggers’ tab, for this job I want it to run every day at midnight, so we can set it to ‘0 0 * * *’.

Next we want to add a script which will do the following;

* tar gzip the ‘jenkins_home’ directory
* push our tar file to S3 bucket.
* remove all files after successful upload.

```
echo 'tar $JENKINS_HOME directory'
set +e 
tar -cvf jenkins_backup.tar -C $JENKINS_HOME .
exitcode=$?
if [ "$exitcode" != "1" ] && [ "$exitcode" != "0" ]; then
exit $exitcode
fi
set -e
echo 'Upload jenkins_backup.tar to S3 bucket'
aws s3 cp jenkins_backup.tar s3://<YOUR_BUCKET_NAME>/
echo 'Remove files after succesful upload to S3'
rm -rf *
```

Check the ‘Console Output’ to get a better understanding of how the script ran. Next we want to visit our S3 bucket and make sure it has uploaded a ’jenkins_backup.tar’ as expected.

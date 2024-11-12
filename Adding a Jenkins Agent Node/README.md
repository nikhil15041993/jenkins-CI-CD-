## Step : Create the Jenkins Agent on the Jenkins Master
Go to Jenkins Manage Jenkins:

From the Jenkins dashboard, click on Manage Jenkins.
Add New Node (Agent):

In Manage Jenkins, click Manage Nodes and Clouds.

Click New Node.

Enter a name for your agent (e.g., Ubuntu-Agent) and select Permanent Agent, then click OK.
Configure the Agent:

Description: Optional, you can leave it empty or describe your agent.
of executors: Set the number of concurrent jobs this agent can run (e.g., 1 for a basic setup).

Remote root directory: Specify the home directory on the agent where Jenkins will store files (e.g., /home/jenkins).

Labels: Optional, you can use labels to assign specific jobs to this agent.

Usage: Set to Use this node as much as possible to let Jenkins schedule jobs on this agent.

Launch method: Select Launch agent via SSH.

Enter Agent Details:

Host: Enter the IP address or hostname of your Ubuntu server where the agent will run.

Credentials: Click Add next to Credentials and create a new SSH credential using the SSH Username with private key option. You can use a private key or password for authentication.

Username: Typically jenkins or your server username.

Private key: Provide the private key for SSH access (or use a password if necessary).

Save the Configuration: Once you’ve filled in the necessary details, click Save.

# Step 2: Set Up the Jenkins Agent on the Ubuntu Server
Ensure SSH is installed on your Ubuntu server:

```
sudo apt install openssh-server -y
```
Verify SSH is running:

```
sudo systemctl status ssh
```
Create a Jenkins user (if not already created):

```
sudo adduser jenkins
sudo usermod -aG sudo jenkins
```
### Configure SSH Keys (optional but recommended):

### On the Jenkins master server, generate SSH keys:

```
ssh-keygen -t rsa -b 2048 -f ~/.ssh/id_rsa
```
Copy the public key to the Ubuntu server where the agent will run:
```
ssh-copy-id jenkins@<agent-server-ip>
```
# Step 3: Launch the Agent on Ubuntu
Connect the Agent:

After setting everything up in Jenkins, the agent should automatically attempt to connect to the Jenkins master.
Check the Manage Nodes page in Jenkins, and the new agent should appear as online.
Verify Connection:

If the agent is not coming online, verify the SSH connectivity and the configuration on both ends.
You can also check the agent logs in Jenkins by clicking on the agent name in Manage Nodes.
# Step 4: Use the Agent in Jenkins Jobs
Now that the agent is set up, you can use it in Jenkins jobs:

Assign Jobs to the Agent:
In the job configuration, go to Restrict where this project can be run and enter the label you assigned to the agent. This ensures that specific jobs will run on the new agent.

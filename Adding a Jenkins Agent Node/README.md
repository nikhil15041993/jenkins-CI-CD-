
## Step 1 Configure Jenkins Master Credentials

Now choose the authentication method.

```
Kind: SSH Username with password
Scope: Global
Username: ubuntu
Password: ubuntu
```

## Step 2 Add New Slave Nodes

On the Jenkins dashboard, click the ‘Manage Jenkins’ menu, and click ‘Manage Nodes’ and Click the ‘New Node’.

Type the node name ‘test-jenkins-slave’, choose the ‘permanent agent’, and click ‘OK’.


## Step 3  Edit Node Information Details.

Now type node information details.

```
Description: test-jenkins-slave node agent server
Remote root directory: /var/jenkins
Labels: test-jenkins-slave
Launch method: Launch slave agent via SSH,
Host: ‘10.0.0.5’ (it’s my test-jenkins-slave external IP)
Authentication: using ‘Jenkins’ credential. ( credential which we create earler)
Host key verification : manualy trust
```

Now click ‘Save’ button and wait for the master server to connect to our agent nodes and launch the agent services.


## On Agent Machine

## Step 1 install java

```
sudo apt install default-jdk
```

## Step 2 create a Directory  /var/jenkins

```
sudo mkdir /var/jenkins
```

## Step 3 change permision to /var/jenkins

```
sudo chmod 777 /var/jenkins
```











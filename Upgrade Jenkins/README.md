# Upgrade Jenkins

### In this short tutorial we will apply minor update to existing Jenkins installation 

Common location of jenkins war file on linux server is:

```
/usr/share/jenkins
```
## Update the installation

Jump to jenkins home directory
```
cd /usr/share/jenkins
```
## Stop the jenkins server

```
sudo service jenkins stop
```

## Move existing jenkins war file

```
sudo mv jenkins.war jenkins.war.old
```

## Download latest jenkins war file

```
sudo wget https://updates.jenkins-ci.org/latest/jenkins.war
```

## Start the Jenkins server

```
sudo service jenkins start
```

Everything should be good now.

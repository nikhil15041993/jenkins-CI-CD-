# How to install Jenkins on Ubuntu 20.04
The steps followed and the commands used to install Jenkins on Ubuntu 20 are as follows:

```
sudo apt-get update

sudo apt-get install openjdk-8-jdk

wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -

sudo sh -c 'echo deb https://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'

sudo apt-get update

sudo apt-get install jenkins

sudo apt install git

```

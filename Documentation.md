# Documentation for Second Deployment 

## Set Up

1. Login to aws console and start an ec2 ubuntu instance with security groups 22, 80 and 8080. 

2. Connect to the ec2 and use the commands below to install jenkins.
```
sudo apt update && upgrade
sudo apt install default-jre
wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -
sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
sudo apt update
sudo apt install jenkins
sudo systemctl start jenkins
sudo systemctl status jenkins

```
3. Activate jenkins user on ec2 with the commands below
```
sudo passwd jenkins
usermod -aG sudo username
sudo su - jenkins -s /bin/bash
```
4. create jenkins user in aws account using the steps below.

- Search for IAM in aws console search bar, click on it. Next click on user in Access management 
- Select add user and enter the name of user you want, then select programmic access and select next.
- Felect "attach existing policies directly" and select administrator access. Now select Next for this page and the other as well.
- Finally, create the user, keep that window open because you will come return to this window for the access key and secret access keys thats generated.



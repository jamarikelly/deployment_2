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

5. install AWS CLI on the jenkins EC2 and configure using commands below.
```
sudo su jenkins 
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
sudo apt install unzip
unzip awscliv2.zip
sudo ./aws/install
aws --version (checking if aws is installed)
aws configure (go back to the IAM window, copy and paste the keys as promped); region set to us-east-1 ; format: json
```
6. install EB cli in the jenkins ec2 using commands below:
```
sudo apt install python3-pip
pip install awsebcli --upgrade --user
export PATH=$PATH:~/.local/bin
eb --version
```
7. Connecting Github to Jenkins Server:

- Fork the deployment repo firstly. https://github.com/kura-labs-org/kuralabs_deployment_2.git
- Create access token from github by navigating to github settings, select developer settings.
- select personal access token and create a new token; select "Repo" and "admin:repo_hook" for the token's permissions 

- Log back into the jenkins and select "New Item"
- 


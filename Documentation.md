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
- Select multibranch pipeline
- Enter a display name and brief description of what youre doing 
- Add a branch source by selecting add source and select github
- Select the add button and select Github
- Click on add and then select jenkins 
- Under the username, enter your github username 
- Under password, enter the token copied from github
- Enter your url to the repository and you can validate by selecting validate.
- After its validated, click apply then save.
- You should see the build happening, if not, scan the repository.

8. Time to deploy application from elastic beanstalk CLI.
```
sudo su - jenkins -s /bin/bash
cd /var/workspace/deployment.main/
eb init 
```
- after the "eb init" command, you'll be prompted, the reply to the prompts are:
* select: us-east-1
* press enter
* select: python
* select the latest version of python
* select N for the (codecommit)
* do command below then follow prompts again
```
eb create
```
* Press enter to take the default settings for the first three prompts
* spot fleet: N
* environment being made, go to elastics beanstalk in aws console  to look at the environment being created. 

9. Add deployment stage and also another tes to the pipeline in jenkinsfile, shown below:
```
 stage ('Deploy') {
       steps {
         sh '/var/lib/jenkins/.local/bin/eb deploy Deploy2-main-dev'
       }
     }
      stage('Test2'){
      steps{
          echo "Testing"
      }
      }
 }
```
10. Finally, scan repository again, now both the "Deploy" and "test2" stages will be shown.
11. Go to this address to see images of the deployment 👉 https://github.com/jamarikelly/kuralabs_deployment_2/blob/main/images_d2.pdf


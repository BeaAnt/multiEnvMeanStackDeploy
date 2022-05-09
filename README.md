# multiEnvMeanStackDeploy   
Deploy a MEAN stack webapplication to multiple environment. Use Nginx on AWS Linux instances and configure Github Actions for the website code to get automatically build and deploy. Quality check with SonarQube (test environment)

## Prerequisites    
create 2 deployment environments so that you can push in both development and production environments   

**development environment:**    
1.Create a AWS EC2 instance with an Ubuntu machine image and download the ssh keys    
2.Install and configure Nginx web server    
3.Configure UFW   
4.Install MongoDB follow link https://www.mongodb.com/docs/manual/tutorial/install-mongodb-on-ubuntu/       
5.Install Node.js and PM2   

![image](https://user-images.githubusercontent.com/57292753/166952834-e8c43522-1777-4e05-86a8-ff509f4260ae.png)       

**production environment:**     
1.Create a AWS EC2 instance with an Ubuntu machine image and download the ssh keys    
2.Install and configure Nginx web server    
3.Configure UFW   
4.Install MongoDB follow link https://www.mongodb.com/docs/manual/tutorial/install-mongodb-on-ubuntu/   
5.Install Node.js and PM2        

## Deploy MEAN Stack Application to Multiple Environments with GitHub Actions        

**Create a repo on Github**   
1.Create the content/application files for the Mean Stack Application:      
  (A MEAN Stack application is made up of a front-end app built with Angular that connects to a back-end api built with Node.js + Express + MongoDB)    
  The backend folder : contains  Node.js + MongoDB API that supports user registration, login with JWT authentication and user management.    
  The frontend folder : contains an example of how to build a simple user registration and login system using Angular 8   
2.Create a workflow file: The action must be created inside a .github/workflow/ folder in a root directory for it to be accessible by Github    

**Create a GitHub repository secrets**    
Click on tab *Settings* of the Github repo, go to *Secrets* section. Click *New repository secret* to create 7 differently named secrets for each environment:    
SSH_PRIVATE_KEY_DEV  -> the contents of a PRIVATE part of the SSH key:.pem file (development environment)    
SSH_PRIVATE_KEY_UAT  -> the contents of a PRIVATE part of the SSH key: .pem file(production environment)   
HOST_DNS_DEV  -> Public DNS record of the EC2 instance(development environment)  
HOST_DNS_UAT  -> Public DNS record of the EC2 instance(production environment)  
TARGET_PATH_F -> remote path where you want to deploy your Frontend code (development environment)       
TARGET_PATH_B -> remote path where you want to deploy your Backend code (development environment)    
TARGET_PATH_PROD_F -> remote path where you want to deploy your Frontend code (production environment)    
TARGET_PATH_PROD_B -> remote path where you want to deploy your Backend code (production environment)   
USERNAME  -> the username of the EC2 instance   

**Create a Github Environments**    
Click on tab *Settings* of the Github repo, go to *Environments* section. Click *New environment* to create 2 environment:    
DEV -> to deploy the code in a development environment    
PROD -> to deploy the code in a production environment. Configure a protection (approval) rule in the environment:The distribution process will be suspended until approval is granted before the PROD run       


**Quality check with sonarQube**    
1. Create a Sonarqube project
2. Create a sonar-project.properties file in project’s root folder (branch dev): this file contains the project key that identifies the project in the SonarQube service 
3. Configure a workflow YAML file with https://github.com/marketplace/actions/sonarqube-scanner-action  
4. Create a Github secret:    
SONAR_HOST_URL – Required this tells the scanner where SonarQube is hosted    
SONAR_TOKEN – Required this is the token used to authenticate access to SonarQube   

Commit to the dev branch and the GitHub Actions deployments will start automatically. You can view the distributions, including details about each run, on the Actions tab.

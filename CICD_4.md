
## Diagram of CI and CD
![Alt text](<CICD pipeline.png>)

```
The local host store your application code where manual changed can be made to the application.
This the then pushed to your GITHUB repository via git which is a type of version control.
This is then passed on to Jenkins using a webhook, which is automates the process of pushing code.
The master node within jenkins distributed the workload to the agent node. this is where it gets tested completing the pipeline of Continious intergration. 
If the test is successfull it is pushed for production using a AWS EC2 instance, this is Continuous deployment.
```


## GIT work flow 
![Alt text](<git workflow.png>)

```
1. This diagram represents the GIT workflow pathway when making changes to your code or updating files. 

2. changes are made in your working directory which is pushed to your local staging area using the command git add.

3. you then commit the changes to your local repository using the command git commit.

4. you are then able to push the changes to your remote GITHUB repository using the command git push. 


```

## What is CI - CD CDE?

```
CI (Continuous Integration) is a development practice that involves merging code  into a shared repository, enabling automated builds and tests to ensure early detection of integration issues.

CD (Continuous Delivery) is practices that focuses on automating the release and deployment processes, allowing software changes to be delivered reliably and frequently to production environments.

CDE (Continuous Deployment Environment) refers to the automated environment where software changes are deployed directly to production without manual intervention, ensuring a streamlined and rapid release process.
```

## The difference between CD and CDE?
```
CD (Continuous Delivery) focuses on automating the release and deployment processes, while CDE (Continuous Deployment Environment) refers specifically to the automated environment where software changes are deployed directly to production, without manual intervention.

think about use cases. CD you can hold the release 
```


### what is jenkins - how does it work?
```
Jenkins is an open-source automation server that helps automate software development processes by providing a framework for building, testing, and deploying applications through configurable pipelines.
```

## why should we used jenkins? benifits? what other automation servers are available 
```
Jenkins is widely used due to its flexibility and large community support.

enabling organizations to automate software delivery, achieve faster time to market, and integrate with a variety of tools and technologies; 

other notable automation servers include GitLab CI/CD, TeamCity, and Bamboo.
```
## Jenkins work flow 

Jenkins follows a workflow where it first pulls code from a version control system in our case GIT, then builds and tests the code, and finally deploys the code to the desired environment based on predefined configurations and triggers.

## SDLC work flow stages 

`Software Development Life Cycle (SDLC) is a framework that defines the steps involved in the development of software at each phase. It covers the detailed plan for building, deploying and maintaining the software.`

```
Requirements Gathering: Gather and document  requirements of the software.

Design: Create a detailed design specification based on the requirements, including architecture.

Development: Write and implement the code based on the design specifications.

Testing: Conduct various types of testing to ensure the software meets the requirements.

Deployment: Release the software to the production environment or distribute it to end users.

Operations & Maintenance: Monitor and support the software in the live environment, user issues, patches, and perform regular maintenance tasks.

Retirement: Eventually phase out or replace the software as it becomes outdated or no longer serves its purpose.

```


# Jenkins on Amazon Web Services (AWS)
## Building a server on AWS

`1. Launch an EC2 Instance`
```
AMI - Ubuntu 18.04 LTS 1e9
```
`2. Configure Security Groups:`
```
adding port 8080 (jenkins), 80 (HTTP), 22 (SSH), 3000 (app)
```
`3. SSH into your EC2 instance` 
```
sudo apt-get update
```
`install java`
```
sudo apt-get install openjdk-11-jdk
```
`installation path for jenkins`
```
curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null

```
```
sudo apt-get update
```
`install jenkins`
```
sudo apt-get install jenkins
```
```
sudo systemctl status jenkins 
```
![Alt text](<jenkins-server-running-Linux .png>)

`Linux should show status: active running`

```
jenkins URL
http://54.229.22.121:8080/
```
```
Enter password and signup to jenkins user interface
```
```
set recommended pluggins - plugins can always be set at a later date
```

```
Manage jenkins tab -> Plugins -> Available plugins: install office 365 and node.js, SSH Agent (.pem file)

manage jenkins tab ->tools ->nodejs installation version 12.0.0

```
```
manage jenkins tab -> Security -> Git Host Key Verification Confiuration: Accept first connection
```

![Alt text](webhook.png)
```
change webhook IP address for new jenkins IP 
tick send me everything  
tick active box

```
`Build jobs CI, CI merge and CD`

# CI build configuration
```

tick Discard old builds ->
Max # of builds to keep = 3

GitHub Project -> 
Paste HTTP: Github project URL

Source Code Management 
Git ->
Repo URL: Github SSH URL
Credentials Add private key path of GITHUB to Jenkins 
Branches to build -> Branch specifier (*/dev)

Build triggers - GITHUB hook trigger fir GITcm polling 

Build Env ->
Name of Nodejs installed (kevin-NodeJS)

Build Steps -> Execute shell 

cd app
npm install
npm test
```
# CI for Merge
```
tick Discard old builds ->
Max # of builds to keep = 3

GitHub Project -> 
Paste HTTP: Github project URL

Source Code Management 
Git ->
Repo URL: Github SSH URL
Credentials Add private key path of GITHUB to Jenkins 
Branches to build -> Branch specifier (*/dev)
```
`additional behaviours ->`
![Alt text](<additional behaviours .png>)

```

Build Env ->
Name of Nodejs installed (kevin-NodeJS)
```
 ![Alt text](<ci MERGE POST BUILD.png>)
 `Post build Actions`
 
 ```
build other porjects ->
projects to build: CD

```
# CD

```

GitHub Project -> 
Paste HTTP: Github project URL

Source Code Management 
Git ->
Repo URL: Github SSH URL
Credentials Add private key path of GITHUB to Jenkins 
Branches to build -> Branch specifier (*/main)

Build triggers - GITHUB hook trigger fir GITcm polling 

Build Env ->
SSH Agent -> credentials: <.pem file key>
```
`Build Steps -> Execute shell` 

```
rsync -avz -e "ssh -o StrictHostKeyChecking=no" app ubuntu@ec2-3-250-229-209.eu-west-1.compute.amazonaws.com:/home/ubuntu/app
ssh -o StrictHostKeyChecking=no ubuntu@ec2-3-250-229-209.eu-west-1.compute.amazonaws.com <<EOF
    unset DB_HOST
    cd app/app
    npm install
    pm2 kill
    pm2 start app.js
    ps aux
    
EOF

```

`Ensure Instance is running with associated AMI of sparta app script`
`open the Public IP of that instance`



```
# Creating AMI of my Jenkins
`tech241-kevin-jenkins-ami`
`10-07-23 - Week 6` 

# Topic of the week CI/CD pipeline

## what it is? 
```
continous intergration and continous deployment pipeline is a set of practices and tools that automate the process of building, testing and delpoying software applications.
```
## why is it important for businesses/ business value  ? 
```
faster time to market 
early bug detection 
collaboration and efficiency 
quality assurance 
risk reduction
```

# Whats is Jenkins?
```
  A tool that automates repatative tasks and facilitating CI and CD practices.
  open source automation server providng wide range of plugins and intergration making it highly customizable and flexible for building pipelines.

```

## other tools used for CI/CD
```
GitLab
Travis CI
Circle CI
Azure DevOps
AWS CodePipeline
GitHub Actions
TeamCity
```
## why build a pipeline ?

```
Essential to streamline and automate the software development and delivery processes. 

It enables consistent and efficient execution of tasks such as code integration, building, testing, and deployment. 

By implementing a pipeline, organizations can ensure faster and more reliable delivery of software updates, improved quality control, and efficient collaboration among development teams.
```

**Software Development Life Cycle (SDLC)**

`plan - design - develop - test - deploy`

```
planning is important to create a strong foundation. 
building a pipeline to add new features to the app and release new features using CI/CD pipeline to allow faster time to market from 3 days automation to 3 minutes.
```

![Alt text](<CICD Architecture.png>)
```
1. Code is on local host 

2. created a SSH key pair and pushed to GITHUB (SCP/gitclone) 
(key for account of github syncing account)

3. webhook - automated the process of pushing code to next stage without manual push. everytime we make a local change.

4. generate SSH from GITHUB to Jenkins (key for repo sync for app code)
```
**This is continuous Intergration**
```
5. test the code: add a payment gateway 

6. master node
(automatated test before production) 
7. agent node 

8. test passed - push to production (if it fails it goes back to CI)

9. pushed to aws on a EC2 instance using SSH (port 22 and add additional port to allow push from jekins)
```



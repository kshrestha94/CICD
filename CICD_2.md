`11-07-2023`
```
testing with tech241
```
# connecting jenkins with github

`create an item - freestyle project`

`Discription:`
```
ci for node.js app new feature 
testing the code using a test agent node called sparta-node-app
```
`discard old builds` 

```
max number of builds 3 
```
`github project URL`

source code management 
- GIT
- Enter Repo URL :SSH
- add: Kind SSH - username with private key 
- Enter username 
- -paste private key 

`build environment`
- provide Node and NPM

`Build`

- execute shell

```
cp add (this command was not needed in my case)
npm install
npm test 
```

`office 365 connector: Restrict where the project can be run spartaubuntu-node`

# adding a webhook

on application repo

click on settings 

click on the webook tag 

payload URL: http://18.133.222.223:8080/github-webhook/

content type -  application/jason 

which events to trigger: - pull requests and push

# CICD Tasks 

# job 1
create a job for ci for node.js app new feature 
testing the code using a test agent node called sparta-node-app

discard old builds and select 3 max build to keep
select GITHub project - insert project URL HTTPS for repo

restrict where the project can be run option select -label expression "sparta-ubuntu-node"

sourece code management 
GIT select 
Repo URL - paste SSH repo URL
credentials - kevin-jenkins 

branch to build 
- branch specifier -*/main

build trigger
-GITHUB hook trigger for GITScm polling 

Buid environment 
- provide Node and NMP bin/folder to path 

build - execute shell 
```
npm install
npm test
```

CI pipeline we created 

# job 2

## create a dev branch on local host - github 
```
git branch 
git checkout -b dev
make adjustment to test file 

on orginal item adjust branch specify to */dev

discard old builds and select 3 max build to keep
select GITHub project - insert project URL HTTPS for repo

restrict where the project can be run option select -label expression "sparta-ubuntu-node"

sourece code management 
GIT select 
Repo URL - paste SSH repo URL
credentials - kevin-jenkins 

branch to build 
- branch specifier -*/dev

build trigger
-GITHUB hook trigger for GITScm polling 

Buid environment 
- provide Node and NMP bin/folder to path 

build - execute shell 

npm install
npm test

git init
git status
git add .
git status
git commit -m "edit dev"
git push -u https://github.com/kshrestha94/tech241_sparta_app dev

```

## make change to dev branch and push code to github- the webhook should trigger

create kevin-ci-merge

add post build action and build other projects to build kevin-ci-merge.

trigger only if the build is stable

## if the test passes- the code should then be merged to from dev to main branch 

configure kevin-ci-merge

```
git branch 
git checkout -b dev
make adjustment to test file 

on orginal item adjust branch specify to */dev

discard old builds and select 3 max build to keep
select GITHub project - insert project URL HTTPS for repo

restrict where the project can be run option select -label expression "sparta-ubuntu-node"

sourece code management 
GIT select 
Repo URL - paste SSH repo URL
credentials - kevin-jenkins 

branch to build 
- branch specifier -*/main

build trigger
-GITHUB hook trigger for GITScm polling 

Buid environment 
- provide Node and NMP bin/folder to path 

build - execute shell 

git checkout main
git merge origin/dev

Post Buid Actions 
git publisher and select 

push only if build suceeds
merge results 

on local terminal dev branch adjust file

git init
git status
git add .
git commit -m "merge"
git push -u https://github.com/kshrestha94
tech241_sparta_app dev

```

# job 3

## clone from the main branch and push to production - AWS EC2

# End goal is to push once from local host, goes through the pipline   






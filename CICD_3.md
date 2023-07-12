# job 3 clone from the main branch and push to production - AWS EC2

## General

`description`
```
CD to rsync and ssh

```
`tick github project`

`project URL - ENTER HTTPS for github repo`

`office 365 connector -tick restrict where this project can be run`

```
label expression - sparta-ubuntu-node
```
`source code management - tick GIT`

```
Repositories 
- Repo URL - enter SSH github repo
- credentials - kevin-jenkins key 
Branches to build 
-branch specifier - */main

```
Build environment 
- tick SSH Agent and specify credentials `tech<>.pem`


Build - execute shell
```

rsync -avz -e "ssh -o StrictHostKeyChecking=no" app ubuntu@ec2-<Public IP>.eu-west-1.compute.amazonaws.com:/home/ubuntu/app
ssh -o StrictHostKeyChecking=no ubuntu@ec2-<public IP>>.eu-west-1.compute.amazonaws.com <<EOF
    unset DB_HOST
    cd app/app
    npm install
    pm2 kill
    pm2 start app.js
    ps aux
    
```

# blockers

```
Sparta app public IP showing 502 error.
could not rsync to correct file path.
```

# troubleshoot method 

```
created app folder in the app folder with all discrepencies and pushed back to git hub. Make adjustments to the execute command and ran build. refreshed sparta application page and is actively working.


```
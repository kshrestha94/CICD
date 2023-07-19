# Infrastructure as Code (Iac)
```
IAC stands for Infrastructure as Code, which is the practice of managing and provisioning computer infrastructure through machine-readable definition files, enabling automated and consistent deployment and management of resources.



codify (user stories) everything in particular langauge (YAML) 
immutable 
on prem, hybrid, and cloud (AWS)

```
`configureation management with ansible`   
```
cheff
set node (VM) and agent node 
- playbooks used to automate scripts to multiple vms simutaneously in YAML 

owned by redhat:
python based, agent less, SSH, free and open source, push model

ansible tool - config tool in devops and devsec

HOST: iventory control file 
```
`orchestraction - terraform` - cloud indpendent; scripts are 
```
reuseable saving time  

aws cloudformmation 
```
![Alt text](<ansible architecture.png>)

# creating ansible controller and agent nodes

```
create 3 EC2 instances (controller, webb, db)
SSH into your controller VM
```

# commands to run of controller node

### 1. `update package list using root access`
```
sudo apt update -y
```

### 2. `install software properties common utilities for managing software repositories and add personal package archive repositories`
```
sudo apt-get install software-properties-common
```
### 3. `add ansible repository (open source IT automation tool) and install onto your system`
```
sudo apt-add-repository ppa:ansible/ansible
```
### 4. `update packages`

```
sudo apt update -y
```
### 5. `install ansible`
```
sudo apt install ansible -y
```
### 6. `check for the correct version`
```
sudo ansible --version
```
### 7. `cd into your ansible folder to make configerations`
```
cd /etc/ansible
```
### 8. `cd into your ssh folder`

```
cd ~/.ssh
```
### 9. `ls to check what you have inside your ssh folder`

```
ls
```
### 10. `nano allows edit the file and add private key for tech241.pem`  
```
sudo nano tech241.pem
```
### 11. `give read access only to your tech241.pem file`
```
sudo chmod 400 tech241.pem
```
### 12. `check inside your folder`
```
ls
```
### 13. `SSH into your web app VM from the controller add sudo`
```
sudo ssh -i "****" ubuntu@ec2-63-33-68-63.eu-west-1.compute.amazonaws.com
```

### 14. `SSH into your db VM from the controller add sudo`
```
sudo ssh -i "***" ubuntu@ec2-3-253-195-197.eu-west-1.compute.amazonaws.com

```
### 15. `cd into ansible` 
```
cd /etc/ansible
```
### 16. `install tree package`
```
sudo apt install tree -y
```
### 17. `edit host file`
```
sudo nano hosts 
```
`add for both webb app and db`

```
[web]
ec2-instance ansible_host=<web pubic IP> ansible_user=ubuntu ansible_ssh_private_key_file=~/.ssh/tech241.pem

[db]
ec2-instance ansible_host=<db pubic IP> ansible_user=ubuntu ansible_ssh_private_key_file=~/.ssh/tech241.pem

```
### 18. `ping command to connect host in the web and db group to send test ping. If the hosts respond successfully, it indicates that the hosts are reachable and the connectivity is working fine. The command will display the output for each host, showing whether the ping was successful or not`
``` 
sudo ansible web -m ping
sudo ansible db -m ping
```

## this concludes this documentation

playbook and ad hoc commands

why use adhoc commands? 
check os of all nodes without habing to SSH into each manually 

cd /etc/ansible
sudo apt install tree -y
sudo nano hosts (change ip address of webb and db) 



### check version 
```
sudo ansible web -a "uname -a"
```

### check region
```
sudo ansible web -a "date"
```

### how much space available and free
```
sudo ansible web -a "free"
```

### folders
```
sudo ansible web -a "ls -a"
```
### create and edit file
```
sudo nano test.text
```

### show whats in the file
```
cat test.text
```
### check host file 
```
ls -a
```

### copy file from host to web
```
sudo ansible web -m copy -a "src=/etc/ansible/test.text dest=/home/ubuntu/test.text"
```

### check folder
```
sudo ansible web -a "ls -a"
```

### copy test.text file from home to web
```
sudo ansible web -m copy -a "src=/etc/ansible/test.text dest=/home/ubuntu/test.text"
```

### check files in web home directory  to see if test.text file has copied
```
sudo ansible web -a "ls -a"
```
### present working directory 
```
pwd
```

### create nginx playbook and script in YML
```
sudo nano nginx-playbook.yml
```

```
# Why YAML? human readable
# YAML file with --- three dashes
# Why playbook - apply repetitive tasks
# create a playbook to install nginx in web node

---

# which host to perform the task
- hosts: web

# see the logs by gathering facts
  gather_facts: yes
# admin access (sudo)
  become: true
# add the instructions - install nginx - to web node
  tasks:
  - name: Installing Nginx
    apt: pkg=nginx state=present
# ensure status of nginx is actively running (systemctl)

# adhoc command to check the status

```

### display what is in playbook file
```
cat nginx-playbook.yml
```

### run playbook with root
```
sudo ansible-playbook nginx-playbook.yml
```

### run nginx status
```
sudo ansible web -a "systemctl status nginx"
```

`copy and paste public IP of web vm and nginx should load`


# creating playbook and install node.js

### create new playbook
```
sudo nano nodejs-playbook.yml
```

```
---

# Which host to perform the task
- hosts: web

# See the logs by gathering facts
  gather_facts: yes

# admin access is required.
  become: true

# add the instructions - install Nodejs - web
  tasks:

  - name: Update apt cache
    apt:
      update_cache: yes

  - name: Ugrade packages
    apt:
      upgrade: dist

  - name: Clone repository
    git:
      repo: https://github.com/kshrestha94/tech241_sparta_app.git
      dest: app


  - name: Installing Node.js
    apt:
      state=present


  - name: Installing npm
    shell: npm install
    args:
       chdir: /home/ubuntu/app/app
       warn: false

  - name: Npm start
    command: sudo npm start
    args:
       chdir: /home/ubuntu/app/app
```

`npm start will hang but if you go to your web vm public IP:3000 sparta app will load.`

# pending tasks

automate using pm2 
add reverse proxy 




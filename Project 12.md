# PROJECT-12-Ansible-refactoring-Assignments-and-imports


# Jenkins Job Enhancement
#### Before we begin, let us make some changes to our Jenkins job – now every new change in the codes creates a separate directory which is not very convenient when we want to run some commands from one place. Besides, it consumes space on Jenkins serves with each subsequent change. Let us enhance it by introducing a new Jenkins project/job – we will require Copy Artifact plugin.
#### Go to your Jenkins-Ansible server and create a new directory called ansible-config-artifact – we will store there all artifacts after each build.
```
sudo mkdir /home/ubuntu/ansible-config-artifact
```
Change permissions to this directory, so Jenkins could save files there – 
```
chmod -R 0777 /home/ubuntu/ansible-config-artifact
```
#### Go to Jenkins web console -> Manage Jenkins -> Manage Plugins -> on Available tab search for Copy Artifact and install this plugin without restarting Jenkins
#### Create a new Freestyle project and name it save_artifacts.

#### This project will be triggered by completion of the existing ansible project. Configure it accordingly:
![image](https://user-images.githubusercontent.com/41236641/153746828-386a080f-cb03-4933-b496-e8a978559ce9.png)
![project12a](https://user-images.githubusercontent.com/41236641/153747404-6855057e-cdfc-4e1e-88f5-e228dc14d680.PNG)

#### The main idea of save_artifacts project is to save artifacts into /home/ubuntu/ansible-config-artifact directory. To achieve this, create a Build step and choose Copy artifacts from other project, specify ansible as a source project and /home/ubuntu/ansible-config-artifact as a target directory.

#### Test your set up by making some change in README.MD file inside your ansible-config-mgt repository (right inside master branch).
#### If both Jenkins jobs have completed one after another – you shall see your files inside /home/ec2-user/ansible-config-artifact directory and it will be updated with every commit to your master branch.

#### Within playbooks folder, create a new file and name it site.yml – This file will now be considered as an entry point into the entire infrastructure configuration. Other playbooks will be included here as a reference. In other words, site.yml will become a parent to all other playbooks that will be developed. Including common.yml that you created previously. Dont worry, you will understand more what this means shortly.

#### Create a new folder in root of the repository and name it static-assignments. The static-assignments folder is where all other children playbooks will be stored. This is merely for easy organization of your work. It is not an Ansible specific concept, therefore you can choose how you want to organize your work. You will see why the folder name has a prefix of static very soon. For now, just follow along.

#### Move common.yml file into the newly created static-assignments folder.

#### Inside site.yml file, import common.yml playbook.
```
---
- hosts: all
- import_playbook: ../static-assignments/common.yml
The code above uses built in import_playbook Ansible module.
```

#### Run ansible-playbook command against the dev environment
#### Since you need to apply some tasks to your dev servers and wireshark is already installed – you can go ahead and create another playbook under static-assignments and name it common-del.yml. In this playbook, configure deletion of wireshark utility.
```
---
- name: update web, nfs and db servers
  hosts: webservers, nfs, db
  remote_user: ec2-user
  become: yes
  become_user: root
  tasks:
  - name: delete wireshark
    yum:
      name: wireshark
      state: removed

- name: update LB server
  hosts: lb
  remote_user: ubuntu
  become: yes
  become_user: root
  tasks:
  - name: delete wireshark
    apt:
      name: wireshark-qt
      state: absent
      autoremove: yes
      purge: yes
      autoclean: yes
update site.yml with - import_playbook: ../static-assignments/common-del.yml instead of common.yml and run it against dev servers:
```
```
ansible-playbook -i /home/ec2-user/ansible/ansible-config/inventory/dev /home/ec2-user/ansible/ansible-config/playbooks/site.yaml
```

![project12ansible](https://user-images.githubusercontent.com/41236641/153747549-19c60499-d987-4a62-a33c-f1a7ca9304b2.PNG)

#### Configure UAT Webservers with a role ‘Webserver’

#### Update your inventory ansible-config-mgt/inventory/uat.yml file with IP addresses of your 2 UAT Web servers
```
[uat-webservers]
<Web1-UAT-Server-Private-IP-Address> ansible_ssh_user='ec2-user' ansible_ssh_private_key_file=<path-to-.pem-private-key>
<Web2-UAT-Server-Private-IP-Address> ansible_ssh_user='ec2-user' ansible_ssh_private_key_file=<path-to-.pem-private-key>
```

#### In /etc/ansible/ansible.cfg file uncomment roles_path string and provide a full path to your roles directory roles_path    = /home/ubuntu/ansible-config-mgt/roles, so Ansible could know where to find configured roles.

#### It is time to start adding some logic to the webserver role. Go into tasks directory, and within the main.yml file, start writing configuration tasks to do the following:

#### Install and configure Apache (httpd service)
#### Clone Tooling website from GitHub https://github.com/<your-name>/tooling.git.
#### Ensure the tooling website code is deployed to /var/www/html on each of 2 UAT Web servers.
#### Make sure httpd service is started
#### Your main.yml may consist of following tasks:

```
---
- name: install apache
  become: true
  ansible.builtin.yum:
    name: "httpd"
    state: present

- name: install git
  become: true
  ansible.builtin.yum:
    name: "git"
    state: present

- name: clone a repo
  become: true
  ansible.builtin.git:
    repo: https://github.com/<your-name>/tooling.git
    dest: /var/www/html
    force: yes

- name: copy html content to one level up
  become: true
  command: cp -r /var/www/html/html/ /var/www/

- name: Start service httpd, if not started
  become: true
  ansible.builtin.service:
    name: httpd
    state: started

- name: recursively remove /var/www/html/html/ directory
  become: true
  ansible.builtin.file:
    path: /var/www/html/html
    state: absent
  ```
  
 #### Reference ‘Webserver’ role
Within the static-assignments folder, create a new assignment for uat-webservers uat-webservers.yml. This is where you will reference the role.
```
---
- hosts: uat-webservers
  roles:
     - webserver
  ```
 #### Remember that the entry point to our ansible configuration is the site.yml file. Therefore, you need to refer your uat-webservers.yml role inside site.yml.

#### So, we should have this in site.yml
```
---
- hosts: all
- import_playbook: ../static-assignments/common.yml

- hosts: uat-webservers
- import_playbook: ../static-assignments/uat-webservers.yml
```
```
ansible-playbook -i /home/ec2-user/ansible/ansible-config/inventory/uat /home/ec2-user/ansible/ansible-config/playbooks/site.yaml
```
![image](https://user-images.githubusercontent.com/41236641/153750509-1341f08b-2726-4b6e-983f-80fc78630311.png)


### Blocker

#### Payload Url 

![project12dpayloadunabletoconnecttohostsecuritygroup](https://user-images.githubusercontent.com/41236641/153747369-4c647209-77f9-4012-8e03-2aaf6fd0d639.JPG)

![project12payloadsolution](https://user-images.githubusercontent.com/41236641/153747377-0424bc20-e00f-439f-bd27-a698e7933142.JPG)

### Blocker

#### Open Authentication Agent error and identity file errors
#### Issue was resolved by:
Installing  ssh agent 

##### By default the ssh-agent service is disabled. Allow it to be manually started for the next step to work.
##### Make sure you're running as an Administrator.
Get-Service ssh-agent | Set-Service -StartupType Automatic

##### Start the service
Start-Service ssh-agent

##### This should return a status of Running
Get-Service ssh-agent

##### Now load your key files into ssh-agent
ssh-add ~\.ssh\id_ed25519

##### Download windows terminal

#### Run on powershell
ssh-add -k C:\Users\ANN\Desktop\ANN-ec2.pem

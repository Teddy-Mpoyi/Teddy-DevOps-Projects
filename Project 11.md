# Project-11---ANSIBLE-CONFIGURATION-MANAGEMENT-AUTOMATE-PROJECT-7-TO-10


<h1> IN THIS PROJECT WE WOULD WORK ON ANSIBLE CONFIGURATION MANAGEMENT WHERE WE WOULD AUTOMATE PROJECT 7 TO 10</h1>

<p>This Project will make you appreciate DevOps tools even more by making most of the routine tasks automated with Ansible Configuration Management, at the same time you will become confident at writing code using declarative language such as YAML.</p>

<h2>Task</h2>

<p>1. Install and configure Ansible client to act as a Jump Server/Bastion Host</p>

<p>2. Create a simple Ansible playbook to automate servers configuration</p>

<h2>Step 1 - INSTALL AND CONFIGURE ANSIBLE ON EC2 INSTANCE</h2>

<p>Update Name tag on your Jenkins EC2 Instance to Jenkins-Ansible. We will use this server to run playbooks.</p>

![1 a](https://user-images.githubusercontent.com/10243139/129177492-35ccbd24-f6df-4faa-b3a5-a4a4927c0ba5.jpg)

<p>In your GitHub account create a new repository and name it ansible-config-mgt.</p>


<p>Instal Ansible</p>

    sudo apt update
    sudo apt install ansible
    
<p>Check your Ansible version by running:</p>
	
    ansible --version
    
![1 d](https://user-images.githubusercontent.com/10243139/129177777-b0c6233d-38a6-45ef-a1d9-a3351a44202e.jpg)

<p>Configure Jenkins build job to save your repository content every time you change it:</p>

<p>Create a new Freestyle project ansible in Jenkins and point it to your ‘ansible-config-mgt’ repository.</p>

![1 e 1](https://user-images.githubusercontent.com/10243139/129178445-c94b8122-3161-44e3-952e-b852d2f978af.jpg)

<p>Configure Webhook in GitHub and set webhook to trigger ansible build.</p>

![1 e 2](https://user-images.githubusercontent.com/10243139/129178503-e4fad420-bade-4194-a27e-895dc2388b36.jpg)

<p>Configure a Post-build job to save all (**) files, like you did it in Project 9.</p>

![1 e 3](https://user-images.githubusercontent.com/10243139/129178576-42a85909-cebb-4f06-a8e2-dad558a2a8f9.jpg)

<p>Test your setup by making some change in README.MD file in master branch and make sure that builds starts automatically and Jenkins saves the files</p>

![1 f](https://user-images.githubusercontent.com/10243139/129178633-26f859c3-af4e-4f53-86e4-3dc3cd38f412.jpg)

<p>Confirm if Jenkins saves the files (build artifacts) in following folder:</p>

	ls /var/lib/jenkins/jobs/ansible/builds/<build_number>/archive/
  
![1 g](https://user-images.githubusercontent.com/10243139/129178725-8bcf12bf-7930-49db-86d5-32fbe31e9c05.jpg)

<h3>Now your setup will look like this:</h3>

![jenkins_ansible](https://user-images.githubusercontent.com/10243139/129178933-eb1f3ac2-e704-41c7-9731-6d0d95cbdfaa.png)

<h2>Step 2 – Prepare your development environment using Visual Studio Code</h2>

<h2>Step 3 - BEGIN ANSIBLE DEVELOPMENT</h2>

- [ ] <p>In your ansible-config-mgt GitHub repository, create a new branch that will be used for development of a new feature.</p>

- [ ] <p>Checkout the newly created feature branch to your local machine and start building your code and directory structure</p>

![3 b](https://user-images.githubusercontent.com/10243139/129180304-7e78178c-bfa1-4c65-8802-a48e9beb3b8d.jpg)

- [ ] <p>Create a directory and name it playbooks – it will be used to store all your playbook files.</p>

- [ ] <p>Create a directory and name it inventory – it will be used to keep your hosts organised.</p>

- [ ] <p>Within the playbooks folder, create your first playbook, and name it common.yml<p>
  
- [ ] <p>Within the inventory folder, create an inventory file (.yml) for each environment (Development, Staging Testing and Production) dev, staging, uat, and prod respectively.</p>

![3 f](https://user-images.githubusercontent.com/10243139/129180519-8fedea0c-de53-4d10-850e-a75d914c88d3.jpg)

<h2>Step 4 – Set up an Ansible Inventory</h2>

<p>Ansible uses TCP port 22 by default, which means it needs to ssh into target servers from Jenkins-Ansible host – for this you need to copy your private (.pem) key to your server.</p>

	eval `ssh-agent -s`
	ssh-add <path-to-private-key>

![4 a](https://user-images.githubusercontent.com/10243139/129182304-4ab2c8ba-3238-4e64-9379-9782550fe426.jpg)

<p>Update your inventory/dev.yml file with this snippet of code:</p>

	[nfs]
	<NFS-Server-Private-IP-Address> ansible_ssh_user='ec2-user'

	[webservers]
	<Web-Server1-Private-IP-Address> ansible_ssh_user='ec2-user'
	<Web-Server2-Private-IP-Address> ansible_ssh_user='ec2-user'

	[db]
	<Database-Private-IP-Address> ansible_ssh_user='ec2-user' 

	[lb]
	<Load-Balancer-Private-IP-Address> ansible_ssh_user='ubuntu'

![4 b](https://user-images.githubusercontent.com/10243139/129182392-27ecbf0e-ce6f-4710-a22e-e46ce9b6e18e.jpg)

<h2>Step 5 – Create a Common Playbook</h2>

<p>It is time to start giving Ansible the instructions on what you needs to be performed on all servers listed in inventory/dev.</p>

<p>In common.yml playbook you will write configuration for repeatable, re-usable, and multi-machine tasks that is common to systems within the infrastructure.</p>

<p>Update your playbooks/common.yml file with following code:</p>

	---
	- name: update web, nfs and db servers
	  hosts: webservers, nfs, db
	  remote_user: ec2-user
	  become: yes
	  become_user: root
	  tasks:
	  - name: ensure wireshark is at the latest version
	    yum:
	      name: wireshark
	      state: latest

	- name: update LB server
	  hosts: lb
	  remote_user: ubuntu
	  become: yes
	  become_user: root
	  tasks:
	  - name: ensure wireshark is at the latest version
	    apt:
	      name: wireshark
	      state: latest
	      
![5 a](https://user-images.githubusercontent.com/10243139/129182974-47ccef20-ee03-49bb-ad9d-df8a989f5c25.jpg)

<h2>Step 6 – Update GIT with the latest code</h2>

<p>Commit your code into GitHub:</p>

	Commit the Code using VS Code
	
![6 a](https://user-images.githubusercontent.com/10243139/129183419-7b9fff24-af6f-4d8d-bca8-c60ee22dbc2a.jpg)


<p>Once your code changes appear in master branch – Jenkins will do its job and save all the files (build artifacts) to /var/lib/jenkins/jobs/ansible/builds/<build_number>/archive/ directory on Jenkins-Ansible server.</p>
	
![6 d](https://user-images.githubusercontent.com/10243139/129184027-ad133d33-1ba1-47bb-8dca-c69cb976346c.jpg)

<h2>Step 7 – Run first Ansible test</h2>

<p>Now, it is time to execute ansible-playbook command and verify if your playbook actually works:</p>

	ansible-playbook -i /var/lib/jenkins/jobs/ansible/builds/<build-number>/archive/inventory/dev.yml /var/lib/jenkins/jobs/ansible/builds/<build-number>/archive/playbooks/common.yml

![7 a](https://user-images.githubusercontent.com/10243139/129184142-9d05f5a7-f014-429e-8d71-29b2a818f430.jpg)

<p>You can go to each of the servers and check if wireshark has been installed by running which wireshark or wireshark --version</p>

![7 b](https://user-images.githubusercontent.com/10243139/129184235-7aa056be-b681-45eb-b09a-6ac090acffa9.jpg)

<p>Your updated with Ansible architecture now looks like this:</p>

![ansible_architecture](https://user-images.githubusercontent.com/10243139/129184324-9ba05d71-784c-4679-9364-e84bac931f80.png)

<h3>NB: There was some modification that had to be done for everything to run successfully</h3>

<p>The dev.yml was modified as follows:</p>

	[nfs]
	172.31.3.128 ansible_connection=ssh ansible_ssh_user=ec2-user ansible_ssh_private_key_file=/home/ubuntu/key.pem

	[webservers]
	172.31.12.46 ansible_connection=ssh ansible_ssh_user=ec2-user ansible_ssh_private_key_file=/home/ubuntu/key.pem
	172.31.11.202 ansible_connection=ssh ansible_ssh_user=ec2-user ansible_ssh_private_key_file=/home/ubuntu/key.pem

	[db]
	172.31.22.39 ansible_connection=ssh ansible_ssh_user=ubuntu ansible_ssh_private_key_file=/home/ubuntu/key.pem

	[lb]
	172.31.43.144 ansible_connection=ssh ansible_ssh_user=ubuntu ansible_ssh_private_key_file=/home/ubuntu/key.pem

<p>The common.yml was modified as follows:</p>

	---
	- name: update web, nfs and db servers
	  hosts: webservers, nfs
	  remote_user: ec2-user
	  become: yes
	  become_user: root
	  tasks:
	  - name: ensure wireshark is at the latest version
	    yum:
	      name: wireshark
	      state: latest

	- name: update LB server
	  hosts: lb, db
	  remote_user: ubuntu
	  become: yes
	  become_user: root
	  tasks:
	  - name: ensure wireshark is at the latest version
	    apt:
	      name: wireshark
	      state: latest

<p>Major Changes was that Database Server is Ubuntu and not RedHat and ansible ssh details had to be added as the former code was giving permission denied.</p>

![NB](https://user-images.githubusercontent.com/10243139/129184607-89d8759f-b4c1-4a13-a84c-06fa23e1f510.jpg)


<h3>Congratulations</h3>

<p>You have just automated your routine tasks by implementing your first Ansible project with Jenkins</p>

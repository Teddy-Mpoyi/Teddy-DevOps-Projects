# EXPERIENCE CONTINUOUS INTEGRATION WITH JENKINS | ANSIBLE | ARTIFACTORY | SONARQUBE | PHP


Part 1 – Configuring Ansible For Jenkins Deployment

In previous projects, you have been launching Ansible commands manually from a CLI. Now, with Jenkins, we will start running Ansible from Jenkins UI.

Launch an AWS EC2 server based on Ubuntu Server 20.04 LTS and name it "Proj14-Jenkins"

Install JDK (since Jenkins is a Java-based application)
    
    sudo apt update -y
    sudo apt install default-jdk-headless -y

Install Jenkins

    wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -
    sudo sh -c 'echo deb https://pkg.jenkins.io/debian-stable binary/ > \
        /etc/apt/sources.list.d/jenkins.list'
    sudo apt update -y
    sudo apt-get install jenkins -y

Make sure Jenkins is up and running

    sudo systemctl status jenkins

![1 d](https://user-images.githubusercontent.com/10243139/137590173-c640787a-088d-47e6-a3f3-0e4ef9abe94f.png)

Perform initial Jenkins setup.

From your browser access, navigate to:

    http://<Jenkins-Server-Public-IP-Address-or-Public-DNS-Name>:8080

You will be prompted to provide a default admin password

Retrieve the default admin password from your server:

    sudo cat /var/lib/jenkins/secrets/initialAdminPassword

Navigate to Jenkins URL

    http://<Jenkins-Server-Public-IP-Address-or-Public-DNS-Name>:8080

![1 g](https://user-images.githubusercontent.com/10243139/137590326-f4d9afc5-a050-4e71-aea9-52ebe4d27f3b.png)

- Install & Open Blue Ocean Jenkins Plugin

![1 h](https://user-images.githubusercontent.com/10243139/137590374-f85d79c7-d6a1-4cc5-8fbb-103425f49213.png)

- Create a new pipeline

- Select GitHub

<img width="1235" alt="Jenkins-Select-Github" src="https://user-images.githubusercontent.com/10243139/137590465-243ba7a0-d2e1-4218-92fb-e0c4aefcade7.png">

- Connect Jenkins with GitHub

<img width="1261" alt="Jenkins-Create-Access-Token-To-Github" src="https://user-images.githubusercontent.com/10243139/137590482-6bb60d31-d792-4873-ab8c-cf3b5a19823b.png">

- Login to GitHub & Generate an Access Token

![Jenkins-Github-Access-Token](https://user-images.githubusercontent.com/10243139/137590510-14f762c9-4a58-4551-a8cb-441a7cc05fe1.png)
<img width="1019" alt="Jenkins-Github-Generate-Token" src="https://user-images.githubusercontent.com/10243139/137590551-e43b78ed-e54c-4e1e-a794-f1368a143677.png">

- Copy Access Token

<img width="1185" alt="Jenkins-Copy-Token" src="https://user-images.githubusercontent.com/10243139/137590581-5ea60eed-04f7-4358-963c-af1fe30af15f.png">

- Paste the token and connect

<img width="1260" alt="JEnkins-Paste-Token-And-Connect" src="https://user-images.githubusercontent.com/10243139/137590598-e30f3f6b-9ab0-4eea-82ee-5e56d3335458.png">

- Create a new Pipeline

![1 o](https://user-images.githubusercontent.com/10243139/137590387-67732d4f-ea2e-496c-8c01-bf7c8f6eac82.png)

- Here is our newly created pipeline. It takes the name of your GitHub repository.

![1 p](https://user-images.githubusercontent.com/10243139/137590405-d32ae185-4912-49d2-8086-4a34031277a1.png)

Let us create our Jenkinsfile

Inside the Ansible project, create a new directory deploy and start a new file Jenkinsfile inside the directory.

Add the code snippet below to start building the Jenkinsfile gradually. This pipeline currently has just one stage called Build and the only thing we are doing is using the shell script module to echo Building Stage

    pipeline {
        agent any

      stages {
        stage('Build') {
          steps {
            script {
              sh 'echo "Building Stage"'
            }
          }
        }
        }
    }

![1 r](https://user-images.githubusercontent.com/10243139/137590809-1c242ba4-d5b9-4d79-9d0e-304de99fc580.png)

Now go back into the Ansible pipeline in Jenkins, and select configure

![Jenkins-Select-Configure](https://user-images.githubusercontent.com/10243139/137590894-c221e266-82d2-4ff1-b5fa-10bf5a675040.png)

Scroll down to Build Configuration section and specify the location of the Jenkinsfile at deploy/Jenkinsfile

<img width="1434" alt="Jenkinsfile-Location" src="https://user-images.githubusercontent.com/10243139/137590935-93c44d00-088f-4d9e-978b-dbea5f087e6e.png">

Back to the pipeline again, this time click "Build now"

<img width="810" alt="Jenkins-Build-Now" src="https://user-images.githubusercontent.com/10243139/137591069-02b380a0-26f3-4715-a624-8d3e0e7630c4.png">

This will trigger a build and you will be able to see the effect of our basic Jenkinsfile configuration by going through the console output of the build.


To really appreciate and feel the difference of Cloud Blue UI, it is recommended to try triggering the build again from Blue Ocean interface.

- Click on Blue Ocean
- Select your project
- Click on the play button against the branch

![1 w](https://user-images.githubusercontent.com/10243139/137591155-0d462f72-17bd-454d-8a12-ac70e79c931a.png)

### Add-On Bonus: Executing Multi-Branch Pipeline 

Notice that this pipeline is a multibranch one. This means, if there were more than one branch in GitHub, Jenkins would have scanned the repository to discover them all and we would have been able to trigger a build for each branch.

Let us see this in action.

Create a new git branch and name it feature/jenkinspipeline-stages

    Git checkout -b feature/jenkinspipeline-stages

Currently we only have the Build stage. Let us add another stage called Test. Paste the code snippet below and push the new changes to GitHub.

       pipeline {
        agent any

      stages {
        stage('Build') {
          steps {
            script {
              sh 'echo "Building Stage"'
            }
          }
        }

        stage('Test') {
          steps {
            script {
              sh 'echo "Testing Stage"'
            }
          }
        }
        }
    }

![1 x b](https://user-images.githubusercontent.com/10243139/137591317-5859dcd0-095a-4aec-a7ea-e7e0aa248a69.png)

To make your new branch show up in Jenkins, we need to tell Jenkins to scan the repository.

- Click on the "Administration" button
- Navigate to the Ansible project and click on "Scan repository now"

![Jenkins-Scan-Repository-Now](https://user-images.githubusercontent.com/10243139/137591377-ab4434dc-b5c1-4066-afc2-c4f9cfe9706f.png)

Refresh the page and both branches will start building automatically. You can go into Blue Ocean and see both branches there too.

![1 x d](https://user-images.githubusercontent.com/10243139/137591394-620ff58d-9106-49bf-8a77-063782f24f59.png)

In Blue Ocean, you can now see how the Jenkinsfile has caused a new step in the pipeline launch build for the new branch.

![1 x e](https://user-images.githubusercontent.com/10243139/137591408-a6cca3b7-f46f-4bac-89ca-8e1d71601be8.png)


## A QUICK TASK FOR YOU!

- Create a pull request to merge the latest code into the main branch
- After merging the PR, go back into your terminal and switch into the main branch.
- Pull the latest change.
- Create a new branch, add more stages into the Jenkins file to simulate below phases. (Just add an echo command like we have in build and test stages)
    - Package 
    - Deploy 
    - Clean up
- Verify in Blue Ocean that all the stages are working, then merge your feature branch to the main branch
- Eventually, your main branch should have a successful pipeline like this in blue ocean

## SOLUTION

Create a pull request to merge the latest code into the main branch

    git checkout main
    git merge feature/jenkinspipeline-stages
    
![1 y a](https://user-images.githubusercontent.com/10243139/137591523-1dc81cc6-6f6f-4651-bef9-1f19eb34a072.png)

Create a new branch, add more stages into the Jenkins file to simulate below phases. (Just add an echo command like we have in build and test stages)
   
- Package 
- Deploy 
- Clean up

        Git checkout -b feature/jenkinspipeline-morestages

Add the code below into the Jenkinsfile

       pipeline {
        agent any

      stages {
        stage('Build') {
          steps {
            script {
              sh 'echo "Building Stage"'
            }
          }
        }

        stage('Test') {
          steps {
            script {
              sh 'echo "Testing Stage"'
            }
          }
        }


        stage('Package') {
          steps {
            script {
              sh 'echo "Packaging Stage"'
            }
          }
        }

        stage('Deploy') {
          steps {
            script {
              sh 'echo "Deploying Stage"'
            }
          }
        }

        stage('Clean up') {
          steps {
            script {
              sh 'echo "Cleaning up Stage"'
            }
          }
        }
        }

    }

![1 y b](https://user-images.githubusercontent.com/10243139/137591587-f5989669-c122-4b1f-965d-066d08fab069.png)

Verify in Blue Ocean that all the stages are working, then merge your feature branch to the main branch

![1 y c](https://user-images.githubusercontent.com/10243139/137591669-66bef0b2-85ea-4718-8179-d38105b19a66.png)

Eventually, your main branch should have a successful pipeline like this in blue ocean

![1 y d](https://user-images.githubusercontent.com/10243139/137591678-524745c8-3b93-4ad0-bfff-941251e22404.png)


## Part 2 – Running Ansible Playbook from Jenkins

Now that you have a broad overview of a typical Jenkins pipeline. Let us get the actual Ansible deployment to work by:

Installing Ansible on Jenkins

    sudo apt install Ansible -y

![2 a](https://user-images.githubusercontent.com/10243139/137591976-81bbc93d-6b23-4f40-b6ad-2a8b6b8cd711.png)

Installing Ansible plugin in Jenkins UI

Creating Jenkinsfile from scratch. (Delete all you currently have in there and start all over to get Ansible to run successfully)

# DevOps CICD

## Project 

Create DevOps CI/CD pipelines using Git, Jenkins, Ansible, Docker and Kubernetes on AWS

![image](https://user-images.githubusercontent.com/104793540/194310307-efd8ff91-56c3-4c75-8226-f6eb71407a20.png)

![image](https://user-images.githubusercontent.com/104793540/194337354-72e003b0-5d73-4621-b2d1-a701718dc6ed.png)


![image](https://user-images.githubusercontent.com/104793540/194337681-35f4c182-8aef-4f04-bfa7-9baf2010c0bf.png)

Deploy to:
- VM 
- Docker container 
- K8 cluster 

### Deploy Artifacts on Tomcat Server 

![image](https://user-images.githubusercontent.com/104793540/194319019-9de43938-0d6a-4ced-89fa-6c5e7ba7c763.png)

#### Setting up CI/CD pipline with Github, Jenkins, Maven and Tomcat
setup jenkins 
- launch ec2 with custom tcp rule for port 8080 from anywhere 
- ssh into ec2 and `sudo su -` to be root user 
- install java 
- install jenkins from https://www.jenkins.io/download/ and download stable version LTS
- Jenkins Redhat Packages 
- `sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo`
- `sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key`
- `yum install fontconfig java-11-openjdk`
- `sudo amazon-linux-extras install java-openjdk11`
- `yum install jenkins`
- `service jenkins status`
- `service jenkins start`
- publicip:8080
- in ec2: cat location to get password > copy and paste into browser > start using jenkins (sign with admin and password)

![image](https://user-images.githubusercontent.com/104793540/194350154-74031d57-2552-46b8-b01c-feca4c0d7055.png)

- New item, configure, build now, console output 

integrate Github with jenkins 

- install git on jenkins instance 
- install github plugin on jenkins GUI 
- configure Git on Jenkins GUI

setup & configure Maven and Git 

setup Tomcat Server 

Integrating GitHub, Maven, Tomcat with Jenkins 

create a CI and CD job 

Test the deployment 

### Deploy Artifacts on a Docker Container 

![image](https://user-images.githubusercontent.com/104793540/194319652-b6bbaa43-c4b6-43f1-9e37-4cfafadd6e00.png)

#### Setting up CI/CD pipline with Github, Jenkins, Maven and Docker
- setting up docker environment 
- write Dockerfile 
- create an image and container on docker host 
- integrate docker host with jenkins 
- create CI/CD job on jenkins to build and deploy container 

### Deploy Artifacts on a Docker Container with help of Ansible 

![image](https://user-images.githubusercontent.com/104793540/194320867-21e66ab0-d606-4b98-9640-8f6c7b3926f2.png)

#### CI/CD pipline with Github, Jenkins, Maven,Ansible and Docker
- setup Ansible server
- integrate Docker host with Ansible 
- Ansible playbook to create image 
- Ansible playbook to create container 
- integrate Ansible with Jenkins 
- CI/CD job to build code on ansible and deploy it on docker container 

### Deploy Artifacts on Kubernetes  

![image](https://user-images.githubusercontent.com/104793540/194322662-e11d6a42-6863-48c0-a89e-130e0657b9e8.png)

#### CI/CD pipline with Github, Jenkins, Maven, Ansible and Kubernetes 
- **setup Kubertnetes (EKS) on aws**
- write pod, service, and deployment manifest files
- integrate Kubernetes with Ansible 
- Ansible playbooks to create deployment and service 
- CI/CD job to build code on ansible and deploy it on Kubernetes 

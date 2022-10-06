# DevOps CICD

## Project 

Create DevOps CI/CD pipelines using Git, Jenkins, Ansible, Docker and Kubernetes on AWS

![image](https://user-images.githubusercontent.com/104793540/194310307-efd8ff91-56c3-4c75-8226-f6eb71407a20.png)

Deploy to:
- VM 
- Docker container 
- K8 cluster 

### Deploy Artifacts on Tomcat Server 

![image](https://user-images.githubusercontent.com/104793540/194319019-9de43938-0d6a-4ced-89fa-6c5e7ba7c763.png)

#### Setting up CI/CD pipline with Github, Jenkins, Maven and Tomcat
- setup jenkins 
- setup & configure Maven and Git 
- setup Tomcat Server 
- Integrating GitHub, Maven, Tomcat with Jenkins 
- create a CI and CD job 
- Test the deployment 

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

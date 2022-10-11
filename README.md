# DevOps CICD

## Project 

Create DevOps CI/CD pipelines using Git, Jenkins, Ansible, Docker and Kubernetes on AWS

![image](https://user-images.githubusercontent.com/104793540/194765673-d9251e6c-3812-4b70-85fb-0c5f325d6175.png)

![image](https://user-images.githubusercontent.com/104793540/194337354-72e003b0-5d73-4621-b2d1-a701718dc6ed.png)


![image](https://user-images.githubusercontent.com/104793540/194765213-d5a59f45-aa4f-4d38-b22f-d50ed4ba0693.png)

Deploy to:
- VM 
- Docker container 
- K8 cluster 

### Deploy Artifacts on Tomcat Server 

![image](https://user-images.githubusercontent.com/104793540/194764180-92c29e84-def8-4cbe-a03a-b25d9e3c1fb3.png)

#### Setting up CI/CD pipline with Github, Jenkins, Maven and Tomcat
##### setup jenkins 
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
- `cat /var/lib/jenkins/secrets/initialAdminPassword`
- profile > configure > change password 

![image](https://user-images.githubusercontent.com/104793540/194350154-74031d57-2552-46b8-b01c-feca4c0d7055.png)

- New item, configure, build now, console output 

##### integrate Github with jenkins 

- install git on jenkins instance `yum install git` check with `git --version`
- install github plugin on jenkins GUI 
- configure Git on Jenkins GUI under Global Tool Config
- pullcodefromgithub job > check workspace console or jenkins ec2 server (`cd /var/lib/jenkins/workspace/pullcodefromgithub` > `ls`)

##### setup & configure Maven and Git  

 Tool that can now be used for building and managing any Java-based project. Benefits include:
- Making the build process easy
- Providing a uniform build system
- Providing quality project information
- Encouraging better development practices

https://maven.apache.org/install 

- setup Maven on Jenkins server

https://maven.apache.org/install
https://maven.apache.org/download.cgi

- `wget https://dlcdn.apache.org/maven/maven-3/3.8.6/binaries/apache-maven-3.8.6-bin.tar.gz`
- `tar -xvzf apache-maven-3.8.6-bin.tar.gz`
- `mv apache-maven-3.8.6 maven`
- maven > bin > `./mvn -v ` - must be done in that location 
- setup environment variable (JAVA_HOME, M2, M2_HOME): `cd ~` `vi .bash_profile`

![image](https://user-images.githubusercontent.com/104793540/194382832-7b19e841-3af0-4f4e-a400-390c9dbe6c17.png)
![image](https://user-images.githubusercontent.com/104793540/194382901-ea05757d-7b1e-4810-a85c-11d7b6a0e213.png)

- `wq!` > `source .bash_profile` > `mvn -v` (check to see if Maven 3.8.3 and Java 11 installed) 
- install maven plugin > under plugin manager > maven integration 
- configure Maven and Java > under global tool config

![image](https://user-images.githubusercontent.com/104793540/194387154-5b897a90-1747-4fb2-b720-c2ffe690a8ea.png)
![image](https://user-images.githubusercontent.com/104793540/194387190-25bc2389-1ac8-4159-8360-1198a7a18b19.png)
- new item > maven project > git:http link > goals: clean install > run now > check console 
- `cd /var/lib/jenkins/` > `ll` > `cd workspace`
- generated artficat using Maven: webapp.war is the artificat inside target folder 

##### setup Tomcat Server 
Apache Tomcat, also known as Tomcat Server, proves to be a popular choice for web developers building and maintaining dynamic websites and applications based on the Java software platform. 

https://github.com/ASalad42/Simple-DevOps-Project/blob/master/Tomcat/tomcat_installation.MD

https://tomcat.apache.org/download-10.cgi
- setup a linux ec2 instance > `sudo su -`
- install java `amazon-linux-extras install java-openjdk11`

configure Tomcat:
- `cd /opt`
- `wget https://dlcdn.apache.org/tomcat/tomcat-10/v10.0.26/bin/apache-tomcat-10.0.26.tar.gz`
- `tar -xvzf apache-tomcat-10.0.26.tar.gz`
- `mv apache-tomcat-10.0.26 tomcat`
- in bin directory `./startup.sh` > then use public ip:8080 to see server 
- Start Tomcat Server 

![image](https://user-images.githubusercontent.com/104793540/194566293-1ad1585d-d83d-41cf-b694-1e42d1296ab3.png)


Access web UI on port 8080:
- getting access to manager app on web browser
- `find / -name context.xml` in tomcat dir
- `vi /opt/tomcat/webapps/host-manager/META-INF/context.xml`
- `vi /opt/tomcat/webapps/manager/META-INF/context.xml`
- `./shutdown.sh ` in bin
- `./startup.sh`
- commenting out example at top in context.xml file 
- go to conf dir and `vi tomcat-users.xml`
- `ln -s /opt/tomcat/bin/startup.sh /usr/local/bin/tomcatup`
- `ln -s /opt/tomcat/bin/shutdown.sh /usr/local/bin/tomcatdown`
- can now use tomcatup and tomcatdown 

##### Integrating GitHub, Maven, Tomcat with Jenkins 
For tomcat install "Deploy to Container"
- go back to jenkins server
- plugin manager > deploy to container 
- manage credentials > deployer creds
- new job > clean install > post build action: deploy war/ear to a container > `/var/lib/jenkins/workspace/MavenProject/webapp/target`
- `webapp/target/webapp.war` or easier is **/*.war 
- tomcat 8 container > http://ip:8080 of our tomcat


configure tomcat server with credentials 


##### create a CI and CD job 

##### Test the deployment 

### Deploy Artifacts on a Docker Container 

![image](https://user-images.githubusercontent.com/104793540/194764324-4b8d310c-81d6-4ae6-addb-b42d33086e4a.png)

#### Setting up CI/CD pipline with Github, Jenkins, Maven and Docker
- setting up docker environment 
- write Dockerfile 
- create an image and container on docker host 
- integrate docker host with jenkins 
- create CI/CD job on jenkins to build and deploy container 

### Deploy Artifacts on a Docker Container with help of Ansible 

![image](https://user-images.githubusercontent.com/104793540/194764817-58f9dcb2-154b-44b8-bffc-f61e0483bddf.png)

#### CI/CD pipline with Github, Jenkins, Maven,Ansible and Docker
- setup Ansible server
- integrate Docker host with Ansible 
- Ansible playbook to create image 
- Ansible playbook to create container 
- integrate Ansible with Jenkins 
- CI/CD job to build code on ansible and deploy it on docker container 

### Deploy Artifacts on Kubernetes  

![image](https://user-images.githubusercontent.com/104793540/194764912-1bf74280-a68b-4ed2-9ec9-cec35adcbc02.png)

#### CI/CD pipline with Github, Jenkins, Maven, Ansible and Kubernetes 
- **setup Kubertnetes (EKS) on aws**
- write pod, service, and deployment manifest files
- integrate Kubernetes with Ansible 
- Ansible playbooks to create deployment and service 
- CI/CD job to build code on ansible and deploy it on Kubernetes 

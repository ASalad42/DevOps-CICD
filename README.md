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
- `vi /etc/hostname` (replace existing with desired) 
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
- tomcat 8 container > http://ip:8080 of our tomcat > apply and build

##### Deploy Artifacts on Tomcat server 
- change code locally , push to github, build new job > changes seen on server - MANUAL

##### Automated build and deploy  
using build triggers
- build periodically > build even if no change at decided condition 
- github hook trigger
- Poll SCM - build only if changes detected > **used this one** > job started  by SCM change > check tomcat server 

![image](https://user-images.githubusercontent.com/104793540/195090883-ca10a8c8-da9d-45ba-bc75-54bb48b0a7c5.png)


### Deploy Artifacts on a Docker Container 

![image](https://user-images.githubusercontent.com/104793540/194764324-4b8d310c-81d6-4ae6-addb-b42d33086e4a.png)

#### Setting up CI/CD pipline with Github, Jenkins, Maven and Docker
setting up docker environment (ensure sg for external port in docker is open)
- `yum install docker -y` `docker --version`
- `service docker start`  `service docker status`  `docker images`  `doccker ps`  `docker ps -a`

##### How Docker Works
![image](https://user-images.githubusercontent.com/104793540/195129991-08da5e69-32fe-409f-b99a-850da445467e.png)

tomcat container on dockerhub 

![image](https://user-images.githubusercontent.com/104793540/195131089-63c6a065-a9f1-4f81-8283-ba5ae9cc4dc5.png)

- dockerpull tomcat on ec2 server 
- docker images to check 
- docker run -d (detached) --name (name i want) tomcat-container -p 8081 (external network):8080 (exposed port to internal network) tomcat
- docker ps -a 
- docker rm id 
- login to container via `docker exec -it 67d8d46c82ba /bin/bash`

write Dockerfile & create an image and container on docker host 

https://docs.docker.com/engine/reference/builder/

```
FROM
to pull base image 

RUN
to execute commands 

CMD
to provide defaults for an executing container 

EXPOSE
informs docker that the container listens on the speficied network ports at runtime 

ENTRYPOINT
to configure a container that will run as an executable

WORKDIR
sets the working diretory 

COPY
copy a directory from your local machine to docker container 

ADD
copy files and folders from you local machine to docker containers 

ENV 
to set environmental variables 
```

- `docker build -t dirofdockerfile`


integrate docker host with jenkins 

- create dockeradmin user `cat /etc/passwd`  `cat /etc/group`  `useradd dockeradmin`  `passwd dockeradmin`  `usermod -aG docker dockeradmin`
- install SCP Publisher Plugin (host, use private ip)
- add Dockerhost to jenkins "configure systems"


create CI/CD job on jenkins to build and deploy container 
- copy from buildanddeployjob
- send artifacts over ssh/scp
- home (webapp/target) - remote 

### Deploy Artifacts on a Docker Container with help of Ansible 

![image](https://user-images.githubusercontent.com/104793540/194764817-58f9dcb2-154b-44b8-bffc-f61e0483bddf.png)

#### CI/CD pipline with Github, Jenkins, Maven,Ansible and Docker

setup Ansible server
- setup ec2 
- setup hostname and create ansadmin user
- `useradd ansadmin`  `passwd ansadmin`
- add user to sudoers file > `visudo` (shift g for end of file)

generate ssh keys and enbale password based login 
- `vi /etc/ssh/sshd_config` (passowrd authentication YES)
- `service sshd reload`
- `sudo su - ansadmin`  `ssh-keygen`

install ansible 
- become root user > `amazon-linux-extras install ansible2`

integrate Docker host with Ansible
- on docker host: create ansadmin, add ansadmin to sudoers file, enable password based login 
- on ansible node: add docker ip to hosts file `vi /etc/ansible/hosts`, copy ssh keys (`ssh-keygen in .ssh dir`  & `ssh-copy-id 172.31.84.22`)
- test the connection 


Ansible playbook to create image 

Ansible playbook to create container 

integrate Ansible with Jenkins 

CI/CD job to build code on ansible and deploy it on docker container 

### Deploy Artifacts on Kubernetes  

![image](https://user-images.githubusercontent.com/104793540/194764912-1bf74280-a68b-4ed2-9ec9-cec35adcbc02.png)

#### CI/CD pipline with Github, Jenkins, Maven, Ansible and Kubernetes 

https://kubernetes.io/docs/setup/production-environment/
https://kubernetes.io/docs/setup/production-environment/tools/

##### How K8 works
Many DevOps tools participate in the workflow of CI/CD pipelines. Kubernetes is often employed as the container orchestrator of choice, together with the Jenkins automation server, Docker, and other tools. Kubernetes keeps track of your container applications that are deployed into the cloud. It restarts orphaned containers, shuts down containers when they’re not being used, and automatically provisions resources like memory, storage, and CPU when necessary.

![image](https://user-images.githubusercontent.com/104793540/196531543-c7dd78c0-217d-4c7c-8583-5bfe8494ff1e.png)

- node: manages and runs pods, it’s the machine (whether virtualized or physical) that performs the given work. 
- pods: A Kubernetes pod is a group of container (smallest unit that Kubernetes administers) (have single IP address that is applied to every container within the pod)
- cluster: of the components put together as a single unit.
- control plane: main entry point for administrators and users to manage the various nodes. 

Both the control plane and individual worker nodes have three main components each.

###### Deployments in Kubernetes

![image](https://user-images.githubusercontent.com/104793540/196551269-481b896d-745c-4ee2-acab-a585e43b0320.png)
![image](https://user-images.githubusercontent.com/104793540/196551136-50684f5c-7cee-495f-b6c4-dbeb6eb23dfd.png)
![image](https://user-images.githubusercontent.com/104793540/196551196-dc2866f9-9b69-40d2-83eb-84e670a3a16a.png)

**setup Kubertnetes (EKS) on aws**
- refer tor README2.md

check ec2 IAM role permissions 

```
eksctl create cluster --name ayanle-cluster \
--region us-east-1 \
--node-type t2.small \
```

![image](https://user-images.githubusercontent.com/104793540/196545357-b59edb09-1c32-4e1c-a5bf-b01a9c9b0bf4.png)
![image](https://user-images.githubusercontent.com/104793540/196545403-b5e44dea-ba0a-4256-bf10-9a084fafcae4.png)
![image](https://user-images.githubusercontent.com/104793540/196545984-a5107664-09eb-476e-a6e0-dd1bfd50a108.png)

- `kubectl run webapp --image=httpd`  (for commands refer to https://kubernetes.io/docs/reference/kubectl/cheatsheet/)
- `kubectl get nodes`  `kubectl get all`   `kubectl get pod`  `eksctl delete cluster ayanle --region us-east-1`
- `kubectl create deployment  demo-nginx --image=nginx --replicas=2 --port=80`
- `kubectl get replicaset`
- `kubectl expose deployment demo-nginx --port=80 --type=LoadBalancer`  `kubectl get services -o wide`
-  `kubectl delete deployment demo-nginx`  `kubectl delete service/demo-nginx`fo LB

write pod, service, and deployment manifest files:

Pod
```
apiVersion: v1
kind: Pod
metadata:
  name: demo-pod
  labels:
    app: demo-app
spec:
  containers:
    - name: demon-nginx
      image: nginx
      ports:
        - name: demo-nginx
          containerPort: 80
```

- `kubectl apply -f pod.yml`

Service:
```
apiVersion: v1
kind: Service
metadata:
  name: ayanle-service
  labels:
    app: regapp 
spec:
  selector:
    app: regapp 

  ports:
    - port: 8080
      targetPort: 8080

  type: LoadBalancer
```

- `kubectl apply -f service.yml`
- `kubectl describe service/demo-service` 
- `kubectl get pod -o wide` > check pod ip and node its running on 

Deployment:
```
# deployment name and deployment lable 
apiVersion: apps/v1 
kind: Deployment
metadata:
  name: ayanle-regapp
  labels: 
     app: regapp

# create 2 pods from the pod template 
spec:
  replicas: 2 
  selector:
    matchLabels:
      app: regapp

# template to create a pod 
  template:
  # pod definition 
    metadata:
      labels:
        app: regapp
  # container definition 
    spec:
      containers:
      - name: regapp
        image: ayanle/regapp  # image name
        imagePullPolicy: Always  # always pull latest image 
        ports:
        - containerPort: 8080  # port that container is listening on 
  
  # make sure only one pod update at a time 
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 1
```

make highly avaiblable and scalable 

- `kubectl apply -f regapp-deploy.yml` and `kubectl apply -f regapp-service.yml`  
- `kubectl get pod -o wide` > check pod ip and node its running on 
- `kubectl describe service/ayanle-service`

![image](https://user-images.githubusercontent.com/104793540/196946721-f03c62dc-6c50-444c-9d36-51940b1bebdb.png)
![image](https://user-images.githubusercontent.com/104793540/196946641-de43e71e-7f6f-49aa-b26b-798129418d39.png)


integrate Kubernetes with Ansible 
- on bootsrap server: create ansadmin, add ansadmin to sudoers files and enbale password based login 
Ansible playbooks to create deployment and service 
- on Ansible Node: add to hosts file, copy ssh keys, test the connection
- start writing ansibe playbooks 

CI/CD job to build code on ansible and deploy it on Kubernetes 

---
description: DevOps interview questions and exercises.
---

# Interview Questions

## Questions covers the below tools/topics

GIT Jenkins Ansible Dockers & Containers Kubernetes OpenShift AWS CI/CD Scripting(Shell/Python) Infra as a code (Terraform or Cloud Formation) Linux (RHEL) Monitoring (Application Monitoring, Log monitoring)

***

* What is GIT ?
* What is the difference between GIT & Github ?
* Why do we use GIT ?
* What is SCM & VCS ?
* What is the process of pushing code to a Github repository ?
* Why do we commit ?
* What are the commands of GIT to push the code ?
* How you can merge a git repository with another ?
* What is branching in git ?
* Different types of branching in GIT ?
* What is merge conflict in git ?
* How you can resolve merge conflict if you are merging same project and in the same branch ?
* Latency vs throughput?

## &#x20;Jenkins

* What is Jenkins ?
* Why we use Jenkins ?
* What are the other tools/technologies present in market other than Jenkins for CI/CD ?
* How to move Jenkins from one server to another ?
* How to create Jenkins backup ?
* What are plugins in Jenkins ?
* What are the default plugins installed in Jenkins ?
* How to schedule builds in Jenkins ?
* Difference between Ant, Maven, Gradle ?
* Difference between Jenkins, Teamcity and Bamboo ?
* How to configure a cloud access in Jenkins ?
* What is Jenkins slaves ?
* How to run a groovy script in Jenkins ?
* What is Jenkins Pipeline ?
* What are different types of Jenkins Pipeline ?
* What is Declarative pipeline in Jenkins ?
* Is Jenkins a CI tool or both CI/CD ?
* How to install Jenkins with non root access in Linux ?
* If you have 200 employees in your company, how you can assign Jenkins access to these employee how you can give permission in Jenkins ?

### Jenkins Task

Task 1 Write the Jenkins pipeline code for Java & Php application Task 2 Write the Jenkinsfile code to build a Java application with Maven with error handling Task 3 Complete the following tasks:

1. Jenkins setup on linux
2. Setup app server with apache to deploy an app.
3. create three jobs on jenkins
4. Pull the code from git repo
5. Build the application
6. deploy an app on apache using ansible.
7. app deploy should work with single trigger hit(git pull job -> build app -> deploy on apache server)
8. job should get triggered on git push on git repo

## Ansible

* What is Ansible ?
* What is Configuration Management ?
* Is Ansible only a tool for Configuration Management ?
* What are the components of Ansible ?
* How Ansible works ?
* What are the other tools in market other than Ansible ?
* How Ansible is different from Chef & Puppet ?
* What is Inventory in Ansible ?
* What are the types of Inventories ?
* What is play & playbook ?
* Difference between hosts & groups ?
* What is Roles ?
* How to install a Role ?
* How to install multiple roles ?
* How to create roles ?
* What is Dynamic Inventory & when we use it & for what ?
* Where is the Ansible Configuration file located ?
* What are the different ways other than SSH by which Ansible can connect to remote hosts ?
* What is variable in Ansible ?
* What are different types of variables ?
* How to assign variables in group vars & hosts vars ?
* Difference between File & Template directory in Roles ?
* Difference between default & vars directory in Roles ?
* What is Jinja 2 template ?
* What is modules in Ansible ?
* Difference between COPY & FILE modules ?
* Difference between SHELL & COMMAND modules ?
* What is Setup module ? what it does ?
* What is register & debug in Ansible ?
* What is changed\_when in Ansible ?
* Can we disable automatic facts gathering in Ansible ?
* How error handling can be done in Ansible ?
* How to ignore failed commands in Ansible ?
* What is handlers ? Why we use Handlers in Ansible ?
* What is Privilege Escalation in Ansible ?
* Task to connect(SSH) Ansible to remote host using another user & run the playbook to the remote host using with another user ?
* What is Ansible vault ?
* How to decrypt a vault file ?
* How to encrypt a string in Ansible using Ansible Vault ?
* If a string is encrypted in a file with a password then how to pass the password using parameter while decrypting ?
* If a file is encrypted using password & password is stored in a file how to pass the file to decrypt the file ?
* If a file is encrypted using password & password is also encrypted then how to provide the password while decrypting the file ?
* What is Ansible galaxy ?
* What is Tags in Ansible ? Why it is used ?
* What is lookup in Ansible playbook ?
* How to control the command failure in Ansible ?
* How to debug your playbook ?
* What is diff mode ?
* What is Dry Run in Ansible & how to do that ?
* What is pre task & post task ?
* How you can run your all tasks at once ?
* What is block in Ansible ?
* What are different variable scopes ?
* How variable precedence takes place ?
* Difference between include & import ?
* How to include custom modules in Ansible ?
* Describe the role directory structure ?

### Ansible Task

Task 1 Part 1.&#x20;

Write Ansible playbook to automate Jenkins deployment&#x20;

Part 2.&#x20;

Write Ansible role to install Docker & setup Kubernetes cluster Automate the pipeline creation in Jenkins to create docker container & deploy on Kubernetes cluster&#x20;



Task 2 Write ansible playbook for below tasks:

1. Install apache server and deploy sample html application
2. Create /var/www/example.com
3. deploy a sample application to the above directory
4. create a virtual host for deploy application and set it as default virtualhost

## Dockers & Containers

* Whats is docker ?
* Difference between container & VMs ?
* Difference between Docker & Virtualization ?
* Difference between container and image ?
* How image builds ?
* What are image layers ?
* How image layers work ?
* What is overlayfs ?
* Where the image layers can be found in which directory ?
* How can we check the content of each layer ?
* How to check the layers stacked with image ?
* What is Union Mount & AUFS ?
* Why use Union mount system for Docker ?
* What are the 3 different directories in /var/lib/docker/aufs ?
* How to run an image ?
* How to tag an image ?
* How to Link one container with another ?
* How do you sequence the containers? A first then B should execute after that ?
* How to create a volume in docker container to store data ?
* How to mount a local directory into a container ?
* How to expose a port no to access container ?
* What is entrypoint in docker ?
* What is dockerfile ?
* Difference between ADD & COPY parameters in dockerfile ?
* How to create a bridge in container ?
* How a container gets an internal IP ?
* Can we check the process of a container inside as well as outside the container ?
* Can we check the container process on docker host ?
* How kernel isolates to run the container and how resources managed by the kernel ?
* What is namespace and cgroups ?
* What is docker-compose and docker-swarm ?
* How you can give different network IP to the container ?
* What are the parameters of dockerfile ?
* Is there any windows container also available ?
* How to stop a container ?
* How to run a container in background ?
* How to go inside a container if container is running in background ?
* How to check running containers ?
* How to remove an image ?
* How to run an image which is in tar format ?
* Command to check the process of a container ?
* How to check resource utilisation of a container ?
* How to create an image ?
* How to save changes of a container ?
* What are registries ?
* Difference between docker commands: up, run & start ?
* Can we run more than one process in a container ?
* How to remove a paticular layer from a container ?
* Give me a command to remove all exited conatiners ?

### Docker Task&#x20;

Part 1. Write a Docker file to create a Docker image which should have Wordpress installed&#x20;

Part 2. Write a Docker file to create a Docker image for database Now, use Docker compose to bring up the above Docker images as containers. Database container should mount the local host's “/etc/mysql” volume into it's (containers) /etc/mysql directory.

## Kubernetes

* What is Kubernetes ?
* What are Kubernetes Components ?
* What is etcd ?
* What is master & minion ?
* How to make quorum of cluster?
* What is Replication controller & what it does ?
* What is ingress ?
* What is RBAC and how can you configure RBAC in a k8's Cluster ?
* Difference between Kubernetes & Docker Swarm ?
* How can you rollbck the previous version of application in Kuberntes?
* Scenario: There are 2 tables, emp, empsal if there schema changes, How does that deployment happens into containers/POD automatically?
* How does container know that application is getting failure ?
* Difference between nodeport, clusterIP, load balancer & ingress ?
* What is kubectl & kubelet ?
* What is the use of Kube-controller manager ?
* What is pod ?
* How many containers can run in a pod ?
* How many containers can be launched in a node ?
* What is the role of Kube-Scheduler ?
* How the 2 pods communicate with each other ?
* How 2 containers inside a pod communicate with each other ?
* What is Flannel & why we use it ?
* Difference between Flannel & Calico ?

## OpenShift

* What is Openshift ?
* Difference between Openshift & Kubernetes ?
* What is Services Layer ?
* How to expose a service in Openshift ?
* What are the 3 components of any created project ?
* What is router in default or in any project while creating project ?
* What do you mean by identity provider in Openshift?
* How do you create a New user identity ?
* Where is the user identity located ?
* What is project in Openshift ?
* What are the types of permissions/role bindings in Openshift ?
* How to check the permission of user ?
* How to describe anything in Openshift ?
* How to check no of projects ?
* How to assign a role/permission to a user ?
* What is clusterrolebinding in openshift ?
* What is the process/working of POD creation ?
* What is Builder POD ?
* What is deployer POD ?
* How to create a New application POD ?
* How to check logs of POD ?
* What is Deployment Configuration & why we need DC ?
* What is SVC & why we need SVC ?
* What is RC (Replication Controller) ?
* How to check DC of POD & how to edit DC ?
* How to create route ?
* How to expose svc ?
* How to do rollout ?
* How to increase replica ?
* What is Source to Image in Openshift ?
* What is builder image ?
* What are the process to create source to image ?
* How to give the Cluster role/permission to the user ?
* How to create secure route ?
* What is PV & PVC ?
* What are access modes in PV ?
* What is node selector ?
* What are the two regions in projects ?
* Difference between template & Deployment Configuration ?
* How to migrate whole cluster to another ?
* How to manually migrate container ?

## AWS

* What is Amazon RDS ?
* What is EC2, S3, EBS ?
* What is VPC & why we require to create VPC ?
* Is is possible to scale an Ec2 Instance vertically ?
* How is Amazon RDS, Redshift & DynamoDB different ?
* How is a spot Instance different from an On-demand Instance ?
* How Infrastructure As Code processed & executes in AWS ?
* If your Linux-build server getting slow down, what will you do to check ?
* Types of EBS storage ?
* How to backup a running instance ?
* How to secure s3 bucket ?
* What are the security available for users to access S3 ?
* How to create AMI ?
* What are the main components of CloudFormation ?
* What is mapping in cloudformation template ?
* How is YAML different from JSON ?
* Different types of ELB ?
* What is autoscaling group ?
* Which type of ELB is good for application load ?
* What is difference between application load balancer & classic load balancer ?
* What is metrics in cloudwatch ?
* Is it possible to recover your lost private key ?
* How can you connect your EC2 Instance if you lost your key ?
* While connecting to your EC2 instances, what are the possible connection issues one might face ?
* What is Subnet & how many subnets are there in a VPC ?
* Why do we make subnets ?
* What is routing table ?
* How you can connect a private subnet with a public subnet ?
* Can VPC peering possible in two different region ?

AWS Task

Task 1 Write a script which will based on “Number of requests” metric of the ALB/ELB scale up webapp EC2 instances under the Load Balancer, increase AWS Elasticsearch Nodes count, and change the instance size of a MongoDB EC2 instance from m4.large to m4.xlarge. (without using ASG). Task 2 Architecture Diagram for a PHP/JAVA/Python based application to be hosted on AWS with all mentions like VPC, AWS/any other cloud platform services, well defined network segregation.

Scripting (SHELL/Python) Shell Task 1:&#x20;

Bash script to setup a whole LAMP stack, PHP app can be Wordpress and DB can be MySQL. This script should install all components needed for a Wordpress website. We should be able to run this script on a local machine or server and after execution of the script it should have Wordpress Running via Nginx/Apache. DB user for Wordpress should also be made automatically from within the script and same should be set in Wordpress conf file.&#x20;

Task 2 Bash script to setup a whole JAVA application stack on a server. This script should install all components needed for a Java/Grails application. Once the script is run it should have the java application running and being served via Nginx on local machine or server. Sample java application can be simply a tomcat war etc

## CI/CD

* What is CI & CD ?
* What is CI/CD pipeline ?
* Difference between Continuous Delivery & Deployment ?
* List the important tools & technologies used in Devops ?

## Linux (RHEL)

* What is Linux ?
* What are Linux OS Flavors ?
* Difference between Debian & RPM based OS ?
* What is Kernel ?
* Explain the boot process of Linux OS ?
* How is RHEL different from CentOS ?
* What is the Latest version of RHEL ?
* What is Grub ?
* Difference between Grub & Grub2 ?
* What is boot loader ?
* Do you think the boot process in RHEL 7 is faster than RHEL 6 ? If yes, How ?
* What is .rpm & .deb ?
* What is RPM ?
* What is YUM ?
* Different methods to install the rpm based packages ?
* What is Bash ?
* What is SHell ?
* How many types of SHells are there ? \* Explain the daily used basic commands like cp, mv, rm ?
* What is the significance of touch command ?
* In how many ways you can create a file ?
* How to delete the content from a file ?
* Explain the process/work behind hitting the google.com ? how you access google.com ?
* How many types of permissions are there ? What is chmod ?
* What is sticky bit ?
* What is ACLs ?
* What is SetGID, SetUID & Stickybit ?
* Location where all the user information are stored ?
* File where user password are stored ?
* What is the default permission of a file ?
* What is the significance of -rvf ?
* What is PV, VG & LV ?
* What are the types of file system ?
* What is XFS ?
* Can we reduce XFS file system ?
* How can we extend LV ?
* Command to check running process ?
* Command to check RAM usage ?
* Command to check Disk usage ?
* Difference between ps -aux & top command ?
* What are the ways to check CPU usage ?
* How to check CPU details ?
* Explain the steps to create a partition & how to format with file system ?
* Explain the steps to create LV ?
* Explain steps to reduce XFS & EXT files systems ?
* Significance of .bashrc file ?
* How you check the kernel version ?
* How you check the Red hat release version ?
* Significance of resolv.conf file ?
* What is DNS ? How you resolve DNS ? Types of DNS records ?
* Difference between Nginx & HTTP Server ?
* Port no of HTTP, FTP, SSH, HTTPS ?
* What is SSH ? How you generate SSH-keys ?
* What is Private & public key ? How they authenticate ?
* Configuration file of SSH ?
* Configuration file of HTTP ?
* What is Virtual Hosting ? How you configure virtual hosting ?
* Explain ifconfig command ?
* Difference between IPv4 & IPv6 ?
* What is MAC address ? can we change the physical address ?
* How to check system uptime ?
* How to check memory information ?
* What is SWAP ?
* What is the exact memory free in your system ?
* What is cache memory ?
* What if you can do rm -rvf / ?
* Kinds of permission in Linux ?
* What is vim & vi ?
* What is pipe | ?
* What is grep command ?
* What Find command does ?
* How to redirect commands output ?
* What is systemd in Linux ?
* What does systemctl do ?
* If you run a command like nautilus in terminal, whether it will block your terminal or not ?
* If yes, whats the solution of this to not to unblock the terminal without closing the command application?
* What is rsyslog ?
* What is SSH-tunnel ?
* How to set history size ?
* How to extend VG ?
* What are logical & extended partitions ?
* Explain the steps to reset root password at boot time ?
* What are run-levels ? How many types of run levels are there ?
* How we change the run level ?
* How to check the logs ?
* Difference between Journalctl & tail command ?
* What does the subscription-manager do ?
* How to archive a file ?
* What is umask ?
* How to kill a process ?
* How to assign IP address manually ?
* How to assign static IP address to a system ?
* Explain the different types of Linux process states ?
* What is a Zombie process ?
* What is KVM ?
* What is hypervisor ?
* Difference between MBR & GPT ?
* How you can mount a file system permanently ?
* What is cron ? How to setup a cron job ?
* What is Kickstart ?
* How to create a network bridge in Linux ?
* Difference between iptables & firewalld
* What is SElinux ?
* What is ISCSI & targetcli ?
* Difference between NFS & SAMBA ?
* What is nfsnobody ?
* What is SSHFS ?
* What is Kerberos ?
* How to secure NFS with Kerberos ?
* What is the difference between telnet & SSH ?
* What is DHCP ?
* What is Kickstart file ?
* What is NTP Server ? How to configure NTP ?

Monitoring

* Why we use monitoring ?
* What are the different tools & technologies for monitoring ?
* How we monitor our applications & servers differently ?
* What is Prometheus ? How is different from other monitoring tools ?
* What is ELK stack ?
* Why we use Grafana ?
* How we query different outputs in Prometheus ?
* What type of graph can be implemented in Grafana ?

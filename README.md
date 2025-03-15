
#  MyprojectApp End-to-End DevOps CICD Pipeline with AWS, Terraform, Jenkins, Ansible & Kubernetes

MyProjectApp is an end-to-end DevOps CI/CD pipeline designed to automate the deployment of containerized applications on AWS infrastructure. This project leverages Terraform for infrastructure provisioning, Jenkins for continuous integration, Maven for build management, Ansible for configuration management, Docker for containerization, and Kubernetes (EKS) for orchestration.

The pipeline ensures seamless code integration, automated testing, and efficient deployment with robust security and scalability. It supports real-time monitoring and manages multiple environments, from development to production.

## Terraform Setup
### Terraform Init
![Image](https://github.com/user-attachments/assets/bcc6d257-241c-4fdf-a008-f6ccac49bd2c)
**Description:** The terraform init command initializes the working directory and downloads the necessary provider plugins (e.g., AWS provider) to manage the resources.
### Terraform Apply
![Image](https://github.com/user-attachments/assets/0c72cbbd-28c8-4792-a9ad-343c2b9e79ad)

![Image](https://github.com/user-attachments/assets/e0370bff-9d8d-4d1a-aa17-2eb8ecdbf289)
**Description:** This screenshot captures the successful execution of the terraform apply command, showcasing the infrastructure provisioning and resource changes.
## 2. Start a Jenkins Server on AWS
### Install Jenkins as root user
```bash
sudo yum update –y
sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key
sudo yum upgrade
amazon-linux-extras install epel
sudo amazon-linux-extras install java-openjdk11 -y
yum install java-11-amazon-corretto -y
sudo yum install jenkins -y
sudo systemctl enable jenkins
sudo systemctl start jenkins
```
![Image](https://github.com/user-attachments/assets/e40b8d3f-dc48-4cfb-95da-b7b32c7eefcc)
**Description:** enkins server is up and running to manage the CI/CD pipeline.
## 3. Setup for Jenkins
![Image](https://github.com/user-attachments/assets/3da36610-30f0-4088-914e-1a79ac92d393)

## 4. Install and Configure Maven
```bash
sudo su  & cd ~
cd /opt
wget https://dlcdn.apache.org/maven/maven-3/3.9.3/binaries/apache-maven-3.9.3-bin.tar.gz
tar -xzvf apache-maven-3.9.3-bin.tar.gz
mv apache-maven-3.9.3 maven
vim .bash_profile
M2_HOME=/opt/maven
M2=/opt/maven/bin
JAVA_HOME=/usr/lib/jvm/java-11-openjdk-11.0.19.0.7-1.amzn2.0.1.x86_64
PATH=$PATH:$HOME/bin:$JAVA_HOME:$M2_HOME:$M2
```
![Image](https://github.com/user-attachments/assets/5498d7ac-f76b-4394-a903-9035a48a854e)
**Description:** Ansible is installed to handle configuration management and automation.
## 5. Install and Configure Ansible
```bash
sudo su
amazon-linux-extras install ansible2
ansible --version
```
## 6. Install Docker on Ansible Server
```bash
sudo yum install docker
sudo usermod -aG docker ansadmin
sudo service docker start
```
![Image](https://github.com/user-attachments/assets/9580d648-0f3a-4fba-a624-dabd538f6230)
**Description:** Docker is installed and running to containerize applications
## 7. Provision EKS Cluster with eksctl
```bash
eksctl create cluster --name myprojectapp-cluster \
--region us-east-2 \
--node-type t2.small
```
![Image](https://github.com/user-attachments/assets/79f1699a-4b51-40cb-ae18-389ae8f5cbd6)
**Description:** Amazon EKS cluster is provisioned to manage container orchestration.
## 8. Install AWS CLI and Connect to Cluster
``` bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
aws eks update-kubeconfig --region us-east-2 --name myprojectapp-cluster
kubectl get nodes
```
**Description:** AWS CLI is configured to manage EKS resources and Kubernetes nodes.
## 9. Integrate Ansible with EKS
```bash
vi /etc/ansible/hosts
ssh-copy-id root@EKS-Server-IP
```
## 10. Deploy Application using Ansible Playbook
``` bash
ansible-playbook kube_deploy.yml --check
ansible-playbook kube_deploy.yml
```
![Image](https://github.com/user-attachments/assets/415d4454-1da3-4c17-8318-a1fb6fee9c27)

**Description:** The application is deployed on the Kubernetes cluster via Ansible playbook.
## 11. Runnig pods in Kubernetes cluster
![Image](https://github.com/user-attachments/assets/6596e898-0005-450c-a40e-a9f4fcce6eeb)
**Description:** The command kubectl get pods is used to list all the running pods in your Kubernetes cluster.
## 12. Access Tomcat Web Application and Registration Page
```bash
kubectl expose deployment myprojectapp-deployment --type=NodePort --port=8080
```
![Image](https://github.com/user-attachments/assets/17d9efce-e61a-403d-bfdb-1225040e952a)
![Image](https://github.com/user-attachments/assets/e5e4b3f7-504e-44ef-a3aa-6151f91aeef2)
**Description:** The Tomcat application is now accessible through the NodePort.
## Conclusion
The MyProjectApp CI/CD pipeline efficiently automates the deployment process using Terraform, Jenkins, Ansible, Docker, and Kubernetes (EKS) on AWS. It streamlines infrastructure provisioning, continuous integration, and container orchestration while ensuring high availability and secure application management. This approach enhances deployment speed, scalability, and overall productivity.










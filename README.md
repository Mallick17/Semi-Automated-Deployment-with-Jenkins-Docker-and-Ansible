# Deploy a finance.war file using Jenkins, Ansible, Docker, and Tomcat to set up a web application accessible through a public IP and port.
### Overview
This project demonstrates the deployment of a finance.war web application using Jenkins, Docker, Tomcat, and Ansible. The application is built and packaged in Jenkins, deployed using Docker, and hosted on a Tomcat server. The deployed application is accessible via a public IP and port.
# Steps for Deployment
**Install Required Tools**<br>
As the `root` user:
- Install Java 17:
  ```bash
  yum install java-17* -y
  ```
- Install Git:
  ```bash
  yum install git -y
  ```
- Setup Docker & Docker Compose
  - Follow standard Installation
  [Install Docker & Docker Compose](https://github.com/Mallick17/Docker.git)
- Set up Jenkins
  


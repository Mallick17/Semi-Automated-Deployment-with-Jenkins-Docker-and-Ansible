# Deploy a finance.war file using Jenkins, Docker, and Tomcat to set up a web application accessible through a public IP and port.
### Overview
This project demonstrates the deployment of a finance.war web application using Jenkins, Docker, & Tomcat. The application is built and packaged in Jenkins, deployed using Docker, and hosted on a Tomcat server. The deployed application is accessible via a public IP and port.
# Steps for Deployment
## 1. Install Required Tools
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
- Installing Jenkins
  - To install Jenkins on a Red Hat-based system, prefer jenkins installation guide for Red Hat [Jenkins Installation page](https://www.jenkins.io/doc/book/installing/linux/#long-term-support-release-3)
  - Enable and Start Jenkins<br>
    Enable Jenkins to start on boot, and start the Jenkins service immediately.
  ```bash
  systemctl enable jenkins
  systemctl start jenkins
  ```
  - Verify Jenkins Installation<br>
    Jenkins typically runs on port 8080. Ensure it is accessible by checking the connection at `http://<your-server-ip>:8080`.
  - Retrieve Jenkins Admin Password<br>
    The initial admin password for Jenkins is stored in the following location. 
    Retrieve it using the command below:
    ```bash
    sudo cat /var/lib/jenkins/secrets/initialAdminPassword
    ```
### 2. Install Maven Plugin in Jenkins
To build Java projects with Maven, you must install the Maven plugin in Jenkins.
- Log in to Jenkins.
- Navigate to **Manage Jenkins** → **Tools** → **Maven Installation**.
- Click **Add Maven**, provide a name (e.g., `Name_Maven`), and **save**.

### 3. Create and Configure a Jenkins Job
a. **Create a New Job**
- On the Jenkins dashboard, click **New Item**.
- Enter the name `project_deploy`, choose **Freestyle project**, and click **OK**.
b. **Configure the Job**
- In General:
  - Under **Source Code Management**, select **Git** and *provide the repository URL*.
  - Click **Apply & Save**.
- Under **Build Triggers**, you can set up triggers such as SCM polling or GitHub webhooks.
- Under **Build Steps**, click **Add build step → Invoke top-level Maven targets**.
  - Set **Maven Version** to the version installed (e.g., `Maven`).
  - Set **Goals** to `clean package`.
  - Click **Apply & Save**.
c. **Run the build**
- Navigate to the `project_deploy` project dashboard.
- Click Build Now to start the build.

### 4. Configure to Deploy the WAR File
- After the Jenkins build is complete, navigate to:
  ```bash
  cd /var/lib/jenkins/workspace/app_deploy/target/
  ls
  ```
- Copy the generated WAR file (finance.war) to the root directory:
  ```bash
  cp finance.war /root
  ```
- Create a `Dockerfile` to package the WAR file into a Tomcat container:
  ```bash
  vi Dockerfile
  ```
  Add the following content:
  ```dockerfile
  FROM tomcat:9-jre9
  MAINTAINER "gyanaranjanmallick444@gmail.com"
  COPY ./finance.war /usr/local/tomcat/webapps/
  ```
### 5. Build and Run the Docker Image
As the `root` user
- Build the Docker image:
  ```bash
  docker build -t project-finance .
  ```
- Verify the image & Run the container & Verify the container is running:
  ```bash
  docker images
  docker run -it -d -p 8081:8080 --name webapp project-finance
  docker ps
  ```
### 6. Access the Application
- Identify the public IPv4 address of your server.
- Access the application in your browser using
  ```vbnet
  http://<public-ip>:8081/finance
  ```
---
## Additional Notes
- **Permissions Issue with Docker:**<br>
   If you encounter a permissions error with the Docker socket, run:
  ```bash
  sudo chmod 777 /var/run/docker.sock
  ```
- **Clean-Up Commands**
  - Stop and remove the container
    ```bash
    docker rm -f webapp
    ```
  - **Remove the Docker image**:
    ```bash
    docker rmi project-finance
    ```
---
## Troubleshooting
- **Jenkins Build Fails**:
  - Verify Maven and Git configurations.
  - Check the repository URL and credentials.
    
- **Docker Build Issues:**
  - Ensure the Dockerfile is in the correct directory.
  - Check for typos in the Dockerfile.

- **Application Not Accessible:**
  - Verify the container is running and bound to the correct port.
  - Ensure the server firewall allows traffic on port 8081.




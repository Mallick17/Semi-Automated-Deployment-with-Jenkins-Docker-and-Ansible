# Deploy a finance.war file using Jenkins, Ansible, Docker, and Tomcat to set up a web application accessible through a public IP and port.
### Overview
This project demonstrates the deployment of a finance.war web application using Jenkins, Docker, Tomcat, and Ansible. The application is built and packaged in Jenkins, deployed using Docker, and hosted on a Tomcat server. The deployed application is accessible via a public IP and port.
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
```bash
sudo wget -O /etc/yum.repos.d/jenkins.repo \
    https://pkg.jenkins.io/redhat-stable/jenkins.repo
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key
sudo yum upgrade
# Add required dependencies for the jenkins package
sudo yum install fontconfig java-17-openjdk
sudo yum install jenkins
sudo systemctl daemon-reload
```
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


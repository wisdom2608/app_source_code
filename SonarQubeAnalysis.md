											SonarQube - Code Quality Analysis
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------

1. Launch an (01) Ubuntu ec2 Instance
 - Use ubuntu, 24.04, t2.large, 26 GB

2. Connect to the instance and install `Jenkins`, `Docaker` and `SonarQube`, on the same server

 - Update the instance 

```bash
sudo apt update
```

 - Setup Jenkins
```bash
sudo apt install openjdk-17-jre-headless -y
```
The commands above can be found in; https://www.jenkins.io/doc/book/installing/linux/#debianubuntu

 - Install Jenkins
```bash
sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc]" \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins -y
```
You can script the commands... 

Access jenkins on the browser and configure it

 
3. SonarQube Setup

 - Setup sonar server using Docker (this is the simplest way)
```bash
sudo apt install docker.io
```

 - Give permissions to run docker
```bash
$ sudo chmod 666 /var/run/docker.sock
```
 -  Run docker commands to install SonarQube
```bash
$ docker run -d --name sonarqube -p 9000:9000 sonarqube:lts-community
```
- Check the docker container
```bash
$ docker ps
```
 - You can see SonarQube container status

Access SonarQube on Port 9000
 - The default login creds of SonarQube is -u `admin`, and -p `admin`


4. Jenkins Pipeline Job for SonarQube Analysis

a). On the Jenkins Dashboard, Go to
  - `Manage Jenkins` →  `Plugins` →  `Available Plugins`
      Search the follwing:
      - `SonarQube Scanner`,: 
      - `Blue Ocean`, : To monitor each step of your pipeline
      - `Eclipse Temurin installer`: 
      - `PipelineStage View`:
    clinck on Install.

b). On the Jenkins Dashboard, Go to
  - `Tools` 
     - `Add JDK` →  Name: `jdk17`, 'Check' install automatically, Select `'Install from adoptium.net'` from dropdown, Version: `select jdk-17.0.11+9`
     - `Add SonarQube Scanner`, Name: `sonar-scanner`, 'Check' install automatically, - Select `Install from Maven Central`, Version: `SonarQube Scanner 5.0.1.3006`
     - `Add Maven` →  Name:  `maven3`,  'Check' install automatically, Select 'Install from Apache' from dropdown, Version: 3.9.7 →  Apply →  Save


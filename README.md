# CI/CD Pipeline: Jenkins, SonarQube, and Docker all in EC2 Slave Setup
## Infrastructure

![image](https://github.com/didin012/Jenkins-EC2-CICD/assets/104528282/2e33feb2-3cc1-4c5c-8a1c-7aec2e66563c)

##Launching our EC2 Instances
1. Launch an EC2 instance for our Jenkinsunch an EC2 instance for our Jenkins

![image](https://github.com/didin012/Jenkins-EC2-CICD/assets/104528282/0a20f780-d593-48dd-a200-9f46249eccb8)
![image](https://github.com/didin012/Jenkins-EC2-CICD/assets/104528282/9ad31ffe-bc4a-4ee3-9778-8c243abd8fbf)
![image](https://github.com/didin012/Jenkins-EC2-CICD/assets/104528282/8eac1113-75c7-4fbe-ba39-516b3ec20854)
![image](https://github.com/didin012/Jenkins-EC2-CICD/assets/104528282/0583c12e-e5a3-472a-8f5f-f70e8b0df9ad)

2. Launch another 2 EC2 instance for our SonarQube and Docker with same specifications on our Jenkins instance. You should have total of 3 EC2 instance running.

## Inside Jenkins EC2 Instance
1. SSH on our Jenkins EC2 instance. Make sure to have the ```pem``` key file in the local directory and ran ```chmod 400 <keypair>.pem``` to associate privileges. <br>
```$ ssh -i <keypair>.pem ubuntu@<jenkinsec2_publicIP>```
2. Run the following commands to install our Jenkins inside. <br>
```$ sudo apt update``` <br>
```$ sudo apt install openjdk-17-jre``` <br>
  ```
  $ sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
    https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
  echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
    https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
    /etc/apt/sources.list.d/jenkins.list > /dev/null
  sudo apt-get update
  sudo apt-get install jenkins
  ```
3. After installing you should be able to access your Jenkins in your EC2. Type in the IP address and add ```:8080``` at the end

![image](https://github.com/didin012/Jenkins-EC2-CICD/assets/104528282/abbe3c13-15b5-4873-979e-3d5c1b459ca6)

4.	To be able to get the Admin Password, type in your EC2 terminal to get the password, copy the code then put it for your password <br>
```$ cat /var/lib/Jenkins/secrets/initialAdminPassword```
5.	Install suggested plugins
6.	Proceed on setting up your account for this Jenkins server instance.

## Check if our Code is Running
1.  Click on New Item then Create a Freestyle project and name it.
2.  Inside the **Source Code Management (SCM)** click Git and put your repository where the files are kept.

![image](https://github.com/didin012/CICD-Pipeline-EC2-Jenkins-SonarQube-Docker/assets/104528282/0d243943-11c0-433e-916b-292d6bed3deb)

3.  Then enable the GitHub hook trigger for GITScm polling option inside Build Triggers. This will trigger the pipeline automatically whenever there are changes in the repository.
4.  Click Save
5.  Go to your GitHub repository then click ```Settings``` > ```Webhooks``` > ```Add Webhook``` then follow the details below.

![image](https://github.com/didin012/CICD-Pipeline-EC2-Jenkins-SonarQube-Docker/assets/104528282/c2212a72-0c06-4ed2-b793-24606f373b07)

6.	Make sure you enable the **Pull Requests** and **Pushes**

![image](https://github.com/didin012/CICD-Pipeline-EC2-Jenkins-SonarQube-Docker/assets/104528282/5c0275f4-27bc-4582-a9f2-c93ba43f05b1)

7.	Click **Add Webhook** then come back to Jenkins. Check if it is running, Click on **Build**

![image](https://github.com/didin012/CICD-Pipeline-EC2-Jenkins-SonarQube-Docker/assets/104528282/590a45bb-b7c0-4d1b-833f-41bc8ed214f0)

8.	Go to **Workspaces** to check if there would be changes inside

![image](https://github.com/didin012/CICD-Pipeline-EC2-Jenkins-SonarQube-Docker/assets/104528282/82befcb7-b061-4557-bf6a-474f90774f6b)

9.	Inside your repository try to add a file inside. This should trigger our pipeline

![image](https://github.com/didin012/CICD-Pipeline-EC2-Jenkins-SonarQube-Docker/assets/104528282/362e621a-6d84-4923-8e17-391e9aa7f34b)
![image](https://github.com/didin012/CICD-Pipeline-EC2-Jenkins-SonarQube-Docker/assets/104528282/9c52950b-40af-46c4-b720-acf85f24fbe0)

## Configuring our SonarQube Server

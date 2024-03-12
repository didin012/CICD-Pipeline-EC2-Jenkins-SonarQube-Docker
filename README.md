# CI/CD Pipeline: Jenkins, SonarQube, and Docker all in EC2 Slave Setups
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




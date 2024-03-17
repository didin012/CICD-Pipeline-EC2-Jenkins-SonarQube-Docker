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

1.	Access the instance using SSH
2.	Rename the hostname for this instance including Jenkins and Docker EC2 slaves <br>
```$ sudo hostnamectl set-hostname sonarqube```<br>
```$ /bin/bash```
3.	Update our instance and run following commands <br>
```$ sudo apt update```<br>
```$ sudo apt install openjdk-17-jre```
4.	Go to Downloads webpage of SonarQube then right click the Community Edition Download and select Copy Link and run the command on our SonarQube instance <br>
```$ wget <copied_link>```

![image](https://github.com/didin012/CICD-Pipeline-EC2-Jenkins-SonarQube-Docker/assets/104528282/e26691fc-3798-4bbb-8185-c60fbb71ff08)
![image](https://github.com/didin012/CICD-Pipeline-EC2-Jenkins-SonarQube-Docker/assets/104528282/3cf9070a-f623-4b84-a782-8230b5cd38e8)

5.	Run the following commands <br>
```$ sudo apt install unzip``` <br>
```$ unzip <name_of_the_zip>``` <br>
```$ ./<name_of_the_unzipped_folder>/bin/linux-x86-64/sonar.sh console```

![image](https://github.com/didin012/CICD-Pipeline-EC2-Jenkins-SonarQube-Docker/assets/104528282/f25de96c-43b2-4cce-bed3-1ac13b6e19fd)

6.	While waiting it to run, add an **Inbound Rule** on our **SonarQube** EC2 instance. It will have a ```9000``` port.

![image](https://github.com/didin012/CICD-Pipeline-EC2-Jenkins-SonarQube-Docker/assets/104528282/0291b7b9-2273-4caa-bb97-292f27e5c603)

7.	After waiting, check if it is running, copy the ```IPv4``` of the SonarQube instance then add ```:9000```
8.	The default login account is username: ```admin``` and password: ```admin```
9.	Inside the SonarQube, click on **Create a local project**

![image](https://github.com/didin012/CICD-Pipeline-EC2-Jenkins-SonarQube-Docker/assets/104528282/57c93196-200c-467e-ad37-e193833c03c2)
![image](https://github.com/didin012/CICD-Pipeline-EC2-Jenkins-SonarQube-Docker/assets/104528282/76d02700-76ff-4888-bdeb-7ce349179d68)
![image](https://github.com/didin012/CICD-Pipeline-EC2-Jenkins-SonarQube-Docker/assets/104528282/8439d9d3-82d1-4bc4-b5fb-d202b2ee8915)

10.	Click ```With Jenkins``` on Analysis Method > GitHub then check on ```Other (for JS, TS, Go, Python, PHP, …)```. Add the sonar-project.properties file inside the GitHub project repository with the indicated content below. This will enable the SonarQube scanning when we using Pipeline scripts. 

![image](https://github.com/didin012/CICD-Pipeline-EC2-Jenkins-SonarQube-Docker/assets/104528282/39f8288b-924e-4384-9b88-921f9ffb185e)

11.	Click on ```My Account``` in SonarQube and select ```Security``` then ```generate a Global Token```. **Save** the token somewhere, we will use it later

![image](https://github.com/didin012/CICD-Pipeline-EC2-Jenkins-SonarQube-Docker/assets/104528282/5e67db26-465a-42fd-9f4e-279ac7f91042)

12.	Go back to Jenkins and go to ```Manage Jenkins``` > ```Plugins``` and search for **SonarQube Scanner**, **SSH Pipeline Steps**, and **SSH2 Easy** then install.

![image](https://github.com/didin012/CICD-Pipeline-EC2-Jenkins-SonarQube-Docker/assets/104528282/b094641a-63f4-4e03-929d-78133cfd4ad2)
![image](https://github.com/didin012/CICD-Pipeline-EC2-Jenkins-SonarQube-Docker/assets/104528282/b164e792-3f6a-43f8-b1f0-f83082a8da01)
![image](https://github.com/didin012/CICD-Pipeline-EC2-Jenkins-SonarQube-Docker/assets/104528282/96454129-b9c2-4630-8384-22f0c07bf304)

13.	Go to ```Manage Jenkins``` > ```Tools``` > Add ```SonarQube Scanner```

![image](https://github.com/didin012/CICD-Pipeline-EC2-Jenkins-SonarQube-Docker/assets/104528282/00f52d5a-4bd5-4f8f-b6fc-80528b5ebf28)

14.	Please input the public IP address of the SonarQube EC2 instance for the Server URL. Make sure to add ```http://``` and port ```:9000```

![image](https://github.com/didin012/CICD-Pipeline-EC2-Jenkins-SonarQube-Docker/assets/104528282/8b34ee29-751b-4511-8962-2c28e55becb8)

15.	Click on add token and select Secret text for Kind and paste the token you have generated earlier.

![image](https://github.com/didin012/CICD-Pipeline-EC2-Jenkins-SonarQube-Docker/assets/104528282/91e6845c-d718-47cf-9621-83e4f4f4eae8)

## Testing our SonarQube in Freestyle Project

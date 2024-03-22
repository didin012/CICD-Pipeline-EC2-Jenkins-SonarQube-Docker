# CI/CD Pipeline: Jenkins, SonarQube, and Docker all in EC2 Slave Setup
## Infrastructure

![image](https://github.com/didin012/Jenkins-EC2-CICD/assets/104528282/2e33feb2-3cc1-4c5c-8a1c-7aec2e66563c)

## Launching our EC2 Instances
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

1.	Go to your Jenkins Project and select **Configure**. Go to **Build Steps** and add Execute SonarQube Scanner. Put the ```sonar.projectKey=<your_project_name>``` inside the **Analysis properties**.

![image](https://github.com/didin012/CICD-Pipeline-EC2-Jenkins-SonarQube-Docker/assets/104528282/a0cf14df-7eba-4136-8384-9ca9d9877e0c)

2.	Save the pipeline and start the **Build**

![image](https://github.com/didin012/CICD-Pipeline-EC2-Jenkins-SonarQube-Docker/assets/104528282/526b6d34-501f-4042-a90f-46c6c87b3bc1)
![image](https://github.com/didin012/CICD-Pipeline-EC2-Jenkins-SonarQube-Docker/assets/104528282/0bfab305-45c6-4ac6-9902-e5635b4da06f)

3.	Check the SonarQube server if it passed for more detailed information

![image](https://github.com/didin012/CICD-Pipeline-EC2-Jenkins-SonarQube-Docker/assets/104528282/9ce2d479-6bd2-4cc9-b94b-1b0c9f1c07bf)

## Configuring Docker Instance
1.	Go to the Docker instance via SSH then set the hostname to docker <br>
```$ sudo hostnamectl set-hostname docker```<br>
```$ /bin/bash```
2.	Follow the commands below:<br>
**Add Docker's official GPG key:**<br>
```$ sudo apt-get update```<br>
```$ sudo apt-get install ca-certificates curl```<br>
```$ sudo install -m 0755 -d /etc/apt/keyrings```<br>
```$ sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc```<br>
```$ sudo chmod a+r /etc/apt/keyrings/docker.asc```<br>
**Add the repository to Apt sources:**<br>
```$ echo \ "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \ (. /etc/os-release && echo "$VERSION_CODENAME") stable" | \ sudo tee /etc/apt/sources.list.d/docker.list > /dev/null```<br>
```$ sudo apt-get update```<br>
```$ sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin```<br>
**To check**<br>
```$ sudo docker run hello-world```
3.	Create a website directory and this is where we will store the files<br>
```$ mkdir website```
### IMPORTANT! Configuring SSH Authentications between our Servers
1. Generate a ssh key on the jenkins<br>
```$ ssh-keygen```
2. Open up the sshd config of the remote SSH server (Docker)<br>
```$ sudo su```<br>
```$ vim /etc/ssh/sshd_config```<br>
3. Change or uncomment the following inside the sshd_config then save<br>
```
PubKeyAuth yes
PasswordAuth yes
KbdInteractive yes
UsePAM yes
```

![image](https://github.com/didin012/CICD-Pipeline-EC2-Jenkins-SonarQube-Docker/assets/104528282/dc04b0fa-2657-4235-8f3a-24d3ee971229)
![image](https://github.com/didin012/CICD-Pipeline-EC2-Jenkins-SonarQube-Docker/assets/104528282/2ee892e3-c240-4b72-b498-aa0572e4c0c2)
![image](https://github.com/didin012/CICD-Pipeline-EC2-Jenkins-SonarQube-Docker/assets/104528282/88c5e3ac-437c-440d-bcdb-8d1d868eedd8)

4. Restart sshd<br>
```$ systemctl restart sshd```
5. Set a password on remote docker SSH (my sample password is ```admin123```) make sure to remember this.<br>
```$ passwd ubuntu```
6.	 Try to connect your jenkins SSH to remote SSH from its terminal

##### Jenkins SSH Terminal

```$ sudo su jenkins```<br>
```$ ssh ubuntu@<remote_ssh_ip>```<br>
```$ exit```
  
7. Copy jenkins pub id to remote SSH server<br>
```$ ssh-copy-id ubuntu@<remote_ssh_ip>```<br>

8. In remote SSH (Docker instance), concatenate the ```id_rsa``` key on the remote SSH server then copy it inside to the **Manage Jenkins** > **Credentials**<br>
```$ cat ~/.ssh/id_rsa```

9. Run the following command for applying privileges<br>
```$ sudo chmod 700 ~/.ssh```<br>
```$ sudo chmod 640 ~/.ssh/authorized_keys```<br>
```$ sudo chmod chown $USER ~/.ssh```<br>
```$ sudo chmod chown $USER ~/.ssh/authorized_keys```

#### OPTIONAL: If it still doesn’t work go to your Jenkins SSH instance then run again these 4 lines above 

10.	After creating credentials, go to **Manage Jenkins** > **System** > **SSH remote hosts**. input the following required details.

### You authenticated and now have access to Docker EC2 instance via Jenkins EC2 instance

## Configure Jenkins for Docker instance

1. Open Jenkins **Dashboard** > **Manage Jenkins** > **System** > **Add server group**

![image](https://github.com/didin012/CICD-Pipeline-EC2-Jenkins-SonarQube-Docker/assets/104528282/26131b22-1893-4d22-a663-25999fa56389)

2. Click Add Server List then put the Docker server

![image](https://github.com/didin012/CICD-Pipeline-EC2-Jenkins-SonarQube-Docker/assets/104528282/d3f874c0-2a10-486c-a098-82b493e08712)

3. Click **Save**

## Making a Pipeline Project

1.	Make a New Item and select ```Pipeline```.
2.	In the Configuration enable ```GitHub hook trigger for GITscm polling```.
3.	Copy the file below. You can make a ```Jenkinsfile``` and store it in the repository then point it out on the Build Steps your GitHub repo for SCM.

![image](https://github.com/didin012/CICD-Pipeline-EC2-Jenkins-SonarQube-Docker/assets/104528282/d4ac7ba8-568e-4839-8fca-27fdd6346288)

4.	Click **Save**.
5.	Configure Docker instance **Security group** and **add inbound rules** with ```8085``` port with ```0.0.0.0/0```.

![image](https://github.com/didin012/CICD-Pipeline-EC2-Jenkins-SonarQube-Docker/assets/104528282/962146e4-8fdc-4819-b88d-9f341078175f)

6.	Add permission on running docker commands on our Docker EC2 instance.<br>
```$ sudo usermod -aG docker ubuntu```<br>
```$ newgrp docker```<br>
To check if it works: <br>
```$ docker ps```

![image](https://github.com/didin012/CICD-Pipeline-EC2-Jenkins-SonarQube-Docker/assets/104528282/a074f162-9cb4-4c0b-a68d-8d30a0d5dbb6)

## Check if the Pipeline is Running Correctly

1. Try to Save and Build your Pipeline project

![image](https://github.com/didin012/CICD-Pipeline-EC2-Jenkins-SonarQube-Docker/assets/104528282/04eb7f3b-56a0-4f1c-a0c5-43bd0fa78e8b)
![image](https://github.com/didin012/CICD-Pipeline-EC2-Jenkins-SonarQube-Docker/assets/104528282/6dac3152-e85e-4413-8efc-ca7b16e6a5e1)

2. Click for **Build Now** and your Pipeline should be running as expected (all boxes are in green color)

![image](https://github.com/didin012/CICD-Pipeline-EC2-Jenkins-SonarQube-Docker/assets/104528282/5bc630c1-ffdb-4c8d-a3f1-c06712d907f3)

3.	Now let’s check for our website so enter ```<dockerinstanceIP>:8085``` in your browser.

![image](https://github.com/didin012/CICD-Pipeline-EC2-Jenkins-SonarQube-Docker/assets/104528282/b2cc2186-f980-4a0b-842d-486f320bff77)

### There you go! We have successfully deployed our website to docker after running a code scanning using SonarQube.

## Testing our GitHub Webhooks if your website will Update 

1.	Let us test our website if it will update whenever there are a commits in our git repository.
2.	First is go to your repository, then click for the ```index.html```. Then click for **Edit** this file

![image](https://github.com/didin012/CICD-Pipeline-EC2-Jenkins-SonarQube-Docker/assets/104528282/f9df6fcc-3a1a-42e4-ba07-8960cc00d6b0)

3.	Inside the file search for this line highlighted in the image then try to replace some other words for that line. Mine would be<br>
```<h1>Hello<br>Congrats<br>Everyone!</h1>```

![image](https://github.com/didin012/CICD-Pipeline-EC2-Jenkins-SonarQube-Docker/assets/104528282/0d06d95c-c020-4693-b33f-b44a78d33ca1)

4.	After changing, click **Commit changes…** then it commits
5.	As you can see on the Jenkins Pipeline, it automatically runs since we have a GitHub Webhook associated with this Pipeline project.

![image](https://github.com/didin012/CICD-Pipeline-EC2-Jenkins-SonarQube-Docker/assets/104528282/733644d8-c635-4406-9a86-7cf07f2a29d0)

6.	Let’s refresh our page again to see the changes in our website

![image](https://github.com/didin012/CICD-Pipeline-EC2-Jenkins-SonarQube-Docker/assets/104528282/c0a4dc17-5b78-4d2a-a846-521d8a2d40f1)

### There you go! There is an update to our website due to new commits in our GitHub repository.

## Cleaning Up
1.	Delete all 3 EC2 instance in the EC2 instance lists

## Troubleshooting

1.	You might encounter something similar to this one where it stops on SonarQube stage. This is due to our instance was stopped.

![image](https://github.com/didin012/CICD-Pipeline-EC2-Jenkins-SonarQube-Docker/assets/104528282/e53d9ade-43da-402e-986f-b2af097cfe0b)

2.	What you can do is that restart the SonarQube EC2 instance then run this command again to power up the SonarQube<br>
```$ ./<name_of_the_unzipped_folder>/bin/linux-x86-64/sonar.sh console```


# Task report week 2

## **Create Jenkins VM with internet access**

1. Register on aws and run 2 virtual machines with Ubuntu 20.04.

    <img width="800" alt="Screenshot 2021-10-17 at 13 43 42" src="https://user-images.githubusercontent.com/76659421/137624104-4b0e6ca6-4449-4326-8da8-3225f854ded6.png">


2. Install java 1.8 and Git in the VM.

    ```bash
    sudo apt-get update
    sudo apt-get upgrade
    #install Java 1.8
    sudo apt-get install openjdk-8-jre
    #install Git
    sudo apt-get install git
    ```

3. Install Jenkins with enabling autostart on startup.
    
     This is the Debian package repository of Jenkins to automate installation and upgrade. To use this repository, first add the key to your system:
    ```
    wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -
    ```    
    Then add a Jenkins apt repository entry:
    ```bash
    sudo sh -c 'echo deb https://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
    ```
    Update your local package index, then finally install Jenkins:
    ```bash
    sudo apt-get update
    sudo apt-get install jenkins
    ```
    Enabling autostart on startup:
    ```bash
    sudo systemctl enable jenkins 
    ```
    Check Jenkins status:
    ```bash
    sudo systemctl status jenkins
    ```

4. Setup custom port 8081 for Jenkins.
   
    Open file ``vi /etc/defaul/jenkins`` and change port 8080 >> 8081:

    <img width="800" alt="Screenshot 2021-10-17 at 14 58 52" src="https://user-images.githubusercontent.com/76659421/137626321-34e3193e-c7c2-41d4-8fa2-2c4f8f91f8d9.png">

5.  Setup plugin on Jenkins - GitHub and Role-based authorization strategy 

    Go ``Dashboard->Manage Jenkins->Manage Plugins``:

    <img width="500" alt="Screenshot 2021-10-17 at 15 06 39" src="https://user-images.githubusercontent.com/76659421/137626579-ab6f3276-1180-43ce-a405-c4ffaa8cfc7a.png">
    
    <img width="800" alt="Screenshot 2021-10-17 at 15 07 16" src="https://user-images.githubusercontent.com/76659421/137626578-f479a6f4-1381-4bfe-9060-652337b56f42.png">

6.  Add new user.

    <img width="387" alt="Screenshot 2021-10-17 at 15 23 03" src="https://user-images.githubusercontent.com/76659421/137627086-4204da6d-6e0a-45b0-9a3f-eeb678f58910.png">

---
## **Create Agent VM**

1.  Install openjdk-8-jre, Git.
    ```bash
    sudo apt-get update
    sudo apt-get upgrade
    #install Java 1.8
    sudo apt-get install openjdk-8-jre
    #install Git
    sudo apt-get install git
    ```

2.  Prepare SSH keys 
    ```bash
    sudo adduser jenkins1
    sudo su jenkins1
    ssh-keygen -b 4096
    cat ~/.ssh/id_rsa.pub #copy this 
    exit #back to root
    mkdir /var/lib/jenkins1
    ```
    
3.  Connect agent to master node 
    Add new node ``worker``:
    <img width="900" alt="Screenshot 2021-10-17 at 15 45 12" src="https://user-images.githubusercontent.com/76659421/137627949-1f12f100-e2a8-44b1-ada6-e4acbbf6cd7a.png">
    Add Credentials:
    <img width="900" alt="Screenshot 2021-10-17 at 15 51 39" src="https://user-images.githubusercontent.com/76659421/137627946-b43e08dc-c800-4613-b583-0632e421be28.png">
    Result:
    
    <img width="1147" alt="Screenshot 2021-10-17 at 17 09 01" src="https://user-images.githubusercontent.com/76659421/137630993-4946e6d3-15bf-4335-a8b4-51e5c828bf70.png">
    Log:
    
    <img width="900" alt="Screenshot 2021-10-17 at 15 42 13" src="https://user-images.githubusercontent.com/76659421/137627951-7319bc2f-ca2d-46ca-80ca-532633fd0ac9.png">

---

## **Configure tools – NodeJS**

Manage Jenkins->Global tool configuration 

Add NodeJS installations with version of NodeJS and global npm packages to install (uglify-js, clean-css-cli) 
    <img width="900" alt="Screenshot 2021-10-17 at 15 50 03" src="https://user-images.githubusercontent.com/76659421/137627948-dce0b18f-2bc1-47c8-886e-3ed305684cc4.png">

---
## **Create “Multibranch Pipeline” pipeline job**

Jenkinsfile with declarative pipeline:
```groovy
pipeline{
	agent any
	tools {
		nodejs 'NodeJS'
	}
	stages{
	}

```

Result:
<img width="1186" alt="Screenshot 2021-10-17 at 18 09 00" src="https://user-images.githubusercontent.com/76659421/137633839-5efec45a-82c0-4538-8264-4f725226049d.png">

Log:
<img width="1282" alt="Screenshot 2021-10-17 at 18 08 16" src="https://user-images.githubusercontent.com/76659421/137633841-96a706c3-cb22-43d8-83d5-2d822a3304bd.png">

---

## **Setup the GitHub webhook to trigger the jobs**
1.  Change setting in Jenkins Configuration:

    <img width="900" alt="Screenshot 2021-10-17 at 17 38 44" src="https://user-images.githubusercontent.com/76659421/137633838-8ca1095b-baa9-429e-b950-0679501c3180.png">
2.  Create webhook on github GUI:

    <img width="900" alt="Screenshot 2021-10-17 at 17 54 25" src="https://user-images.githubusercontent.com/76659421/137633847-bce2e5fe-b361-4635-95f7-6554b83dc341.png">
3.  Generate token for Jenkins:

    <img width="900" alt="Screenshot 2021-10-17 at 17 34 31" src="https://user-images.githubusercontent.com/76659421/137633996-ea505446-ba63-44e2-a734-fcaaf63b914c.png">
4.  Githook work:

    <img width="900" alt="Screenshot 2021-10-17 at 18 25 15" src="https://user-images.githubusercontent.com/76659421/137634062-dae06ef9-e39d-436b-bf19-ff0059379ea7.png">

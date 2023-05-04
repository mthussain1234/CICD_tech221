# Building Jenkins server on aws

# Diagram


1. Create EC2 instance (we call this `mohammad-tech221-master-node`), using Ubuntu 18.04 LTS, should be present in recently used AMIs
2. For Security groups for the EC2 instance, we use the following security groups:

![image](https://user-images.githubusercontent.com/129314018/236260843-8e3b3029-32d3-4ec6-b1c2-a168f9cb43e2.png)

3. Set up Jenkins with Java
   - SSH into our instance we created
   - within the instance run these commands
 ```
sudo apt-get update
sudo apt-get install default-jdk -y


wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -
sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
sudo apt-get update
sudo apt-get install jenkins
sudo systemctl start jenkins
sudo systemctl enable jenkins

 

sudo ufw allow OpenSSH
sudo ufw enable

 

sudo cat /var/lib/jenkins/secrets/initialAdminPassword # password for jenkins
```
4. On the EC2 instance, under details after checking the box, copy and paste the public ipV4 address into search bar on browser
5. Type in the ip address in this format `<ip-address>:8080` -> this should direct you to the Jenkins set up page and prompt you to enter the admin password
6. In our code in **Step 3**, we have a command to show the password for jenkins, copy this and enter it at this stage when prompted by the webpage for jenkins
7. After doing this, it will prompt you to install plugins, we select `suggested plugins`
8. after doing this it will prompt you to enter login details, such as username, email, name and password
9. Store your username password and admin password for future use
10. 

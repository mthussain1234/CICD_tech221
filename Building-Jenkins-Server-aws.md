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
10. You are now in your own Jenkins server, follow the images shown below to install the relevant plugins, on the third image you will search for these plugins `Amazon EC2, NodeJS, Office 365 Connector, SSH Agent Plugin`.

![image](https://user-images.githubusercontent.com/129314018/236276672-8343db69-378c-4b1d-bd17-4d1518a7dfe9.png)
![image](https://user-images.githubusercontent.com/129314018/236276741-bf7d34d7-8474-4856-b6f4-6421cda3502c.png)
![image](https://user-images.githubusercontent.com/129314018/236276816-1e21e246-4635-484f-82fb-c17b647c26e4.png)

11. We now create a new item on our Jenkins server, follow the images below in order to create our first item `mohammad-ci-merge` which will allow for automated merging, -> be sure to click `freestyle project` aswell
12. Also,  follow [Using SSH with GitHub](https://github.com/mthussain1234/test-ssh#using-ssh-with-github), to create a new ssh key, we name it `mohammad-cicd-key`, which we will use for our new CICD pipeline.
      - SSH into your ec2 instance (master node) in Gitbash, and use commands `sudo su - jenkins` to get superuser rights (administrator) and to switch to the Jenkins instance, then we use `ssh -T git@github.com` to allow us to use our key on this jenkins server


![image](https://user-images.githubusercontent.com/129314018/236281935-19e39141-d855-4a8b-81ab-b7e285a09cc7.png)
![image](https://user-images.githubusercontent.com/129314018/236282330-56c2b29d-bdf7-470c-be39-6c7f93c8a51e.png)
![image](https://user-images.githubusercontent.com/129314018/236283807-492124a6-0003-4e79-8c60-fb01c381cf61.png)
![image](https://user-images.githubusercontent.com/129314018/236284100-ab8b2b84-2de3-492b-8dd4-cd0fcd2c09ed.png)

13. Create a new site after creating the above, this will link to the above site, follow screenshots below, we named this one `mohammad-ci-merge-dev`, to allow us to merge dev branch to main.

![image](https://user-images.githubusercontent.com/129314018/236284364-d6437c83-fb86-47ab-bb27-fa495eb52748.png)
![image](https://user-images.githubusercontent.com/129314018/236284424-c818df54-ad4e-4748-9b03-34c0f2bbebd8.png)
![image](https://user-images.githubusercontent.com/129314018/236284482-04b4795d-eb4e-480a-9468-04693620e975.png)
![image](https://user-images.githubusercontent.com/129314018/236284563-2baa00cf-23b6-4019-9624-7f90e9a1d0d9.png)

14. To make the actual pipeline work we must do our webhook trigger with this new jenkins server, [Follow these steps](https://github.com/mthussain1234/CICD_with_Jenkins#webhook-creation), and change the url so it matches your link on your Jenkins server.

15. To test this, we can go on GitBash, `cd` to our relevant repository, and use `git checkout dev` to go to dev branch, make changes to your code, then we run `git add .` then `git commit -m "xxxx"` then `git push` to push it to the GitHub, if both jobs set up as above, the changes should be present
on both branches `main` and `dev`

## Deploy app

1. To Deploy our app, we first name it `mohammad-ci-sparta-app`, and we configure as shown below.

![image](https://user-images.githubusercontent.com/129314018/236286394-30fc71a3-0560-45f5-81ee-964a63d938d2.png)
![image](https://user-images.githubusercontent.com/129314018/236286473-d03b354f-f08b-410f-92e2-83ae6039f533.png)
![image](https://user-images.githubusercontent.com/129314018/236286635-6bf8c6f3-48d2-49bc-8f58-605240046c67.png)
![image](https://user-images.githubusercontent.com/129314018/236286703-12587def-b72a-49ab-aaf7-d2355b01d24f.png)
```
scp -v -r -o StrictHostKeyChecking=no app/ ubuntu@<ec2-app-ip>:/home/ubuntu/
ssh -A -o StrictHostKeyChecking=no ubuntu@<ec2-app-ip> <<EOF
#sudo apt install clear#

cd app

#sudo npm install pm2 -g
# pm2 kill
nohup node app.js > /dev/null 2>&1 &
```
2. Deploying this job, should cause it to work, and can be further tested by checking the `EC2-APP-IP:3000` in the search bar and should show the following screen or something similar

![image](https://user-images.githubusercontent.com/129314018/236287766-c4fc3d78-daac-4157-b94b-fdcb24ec56d2.png)

3. To fully test CICD pipeline, change your code to show visible changes, whcih we can find within `app` folder, in `views` should be under `index.ejs` and changing the text relevant to the app page, to show a difference.
4. Once saved, push these changes to your github, shown in **step 15** in the last section.
5. It should push the changes, to dev branch, if tests are successful, will merge with the main branch, and the deployment job we just created will deploy the new app with the new changes and, they should be present when retrying the  `EC2-APP-IP:3000`.






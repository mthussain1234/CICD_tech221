# CICD, Jenkins, CDE (git pull local test)

#  testing my own cicd piplline on jenkins v1

# testing ci from github with tech221

## What is Jenkins

* Open source automation server, facilitates/helps continuous integration and continuous delivery (CI/CD) pipelines
* Allows developers to automate building, testing, and deploying software applications
* CI = merging code from multiple developers in a shared repo multiple times a day, lest them detect integration issues earlier, and reduce time and effort needed to fix

## What other tools available

* GITLAB CI/CD = provide built in cicd capabilities, define and manage piplines directly inside gitlab repo
* Bamboo = CICD tool by atlassian, robust build, test, deployment automation capabilities
* GitHub Actions - CICD solution by GITHUB, allows devs to define worklows using YAML to automate sdlc



## Jenkins stages

* Source - pipeline retrives source code. checking the code and getting latest updates
* Build - source code compiled (readable, syntax free), dependencies resolved ( and artefacts generated, convert source code into deployable form eg exec files or libs
* Test - Various tests executed to ensure quality and functinality of application, eg unit tests, integration tests, and other automated tests
* Production - After tests, application is deployed to production environment, you prepare the infrastructure, by configuring app and deploying it to environment
* 

## Difference between continuous delivery (CD) and continuous deployment (CDE)

* CD = Continuous Delivery is an extension of continuous integration to make sure that you can release new changes to your customers quickly in a sustainable way. This means that on top of having automated your testing, you also have automated your release process and you can deploy your application at any point of time by clicking on a button. In continuous Delivery the deployment is completed manually.
* Continuous Deployment goes one step further than continuous delivery, with this practice, every change that passes all stages of your production pipeline is released to your customers, there is no human intervention, and only a failed test will prevent a new change to be deployed to production.

goal of cicd - achieve end to end autoamtion

![image](https://user-images.githubusercontent.com/129314018/235719214-a2dab7e2-ff88-441d-a7d8-56c17a4180fd.png)

* Local Host - represent local environment (computer)
* GitHub via SSH - We use SSH to securely connect to GitHub repository
* Jenkins via SSH - Will connect to the GitHub via SSH, allowing it to fetch code from the repository
* Jenkins uses GitHub Status API to update status of commit/pull request, can show if deployment was successful or not
* Jenkins also uses Deployment API to trigger and manage deployments
* WebHook trigger lets GitHub send notifications to Jenkins when an event happens in the repository, this can be a code push/pull, makes Jenkins perform a build or deployment process, it is like a signal for Jenkins to perform the actions needed depending on what takes place within the repository
* Master Node = Main jenkins server, Agent Node = computers, eg: EC2 instances
* Jenkins will use master node to manage and schedule tasks
* When there is lot of work, master node can share tasks with agent nodes, which perform tasks quickly by working on them simultaneously
* The link between both master and agent is like master node is distributing tasks to the agent nodes, and they will recieve updates on the progress.


## CI/CD - Jenkins to GitHub App deployment

### Pre-requisites
<details>
  <summary>[PREREQUISITES TO CLONE APP FOLDERS]</summary>
  
  We used the following commands in order to clone someones app folder and environment folder to our local machine
  which we in turn pushed to this repository.
  
  479  git clone https://github.com/khanmaster/sre_jenkins_cicd.git
  
  486  cp -R app /c/Users/tahir.LAPTOP-2JTDBK37/Documents/tech_221/CICD_tech221
  
  490  cd sre_jenkins_cicd/
  
  491  cp -R environment /c/Users/tahir.LAPTOP-2JTDBK37/Documents/tech_221/CICD_tech221
  
  495  git add .
  
  496  git commit -m "Cloned folders from other repository"
  
  497  git push origin main
  
  498  git fetch origin
  
  499  git merge origin/main
  
  500  cd Documents/
  
  501  cd tech_221/
  
  502  cd CICD_tech221/
  
  503  ls
  
  504  git status
  
  505  git commit -m "first commit"
  
  507  git status
  
  509  git push
</details>


### Jenkins to GitHub App deployment

1. Pre-requisites to starting with Jenkins, follow [Using SSH with GitHub](https://github.com/mthussain1234/test-ssh#using-ssh-with-github)
2. Make your keys, we had named them `mohammad-jenkins-key`, and be sure to follow the documentation, also make sure the `app` folders are cloned, as you can see in the contents of this github repository.
3. On Jenkins, click `New Item`.

![image](https://user-images.githubusercontent.com/129314018/235678316-948b83c1-5034-4647-afc6-eb37d9e21a99.png)

4. On the next page, enter the name we use `mohammad-CI` similar naming conventions are allowed, and select `freestyle project` and click Ok

![image](https://user-images.githubusercontent.com/129314018/235678494-d680aede-a141-4cfb-94be-fbec8ba44806.png)

5. On the next page, do your description as you please, we do it as shown below, select `Discard old builds`, and as shown below check the `Github project` to allow for the use of the app folder as discussed below
6. On the GitHub repository also shown below, copy the `HTTPS` link of the repo and copy it into the Jenkins `project url`

![image](https://user-images.githubusercontent.com/129314018/235699049-7c83fc8f-8cb6-4b6a-acb5-de1ec5cb4618.png)

7. Scrolling down, check the box as shown below, and type `sparta ubuntu node` and click the popup, on the label expression.

![image](https://user-images.githubusercontent.com/129314018/235699355-d16b50b9-4c53-4e23-9c72-5c3dccc3012b.png)

8. On Source code management click `Git`, and on repository URL, we can see below on the repository we click `Code` then click `ssh` and copy and paste that url in the repository URL.

![image](https://user-images.githubusercontent.com/129314018/235699909-30280628-5d2a-4579-8d07-663401c952ea.png)

9. You may be met with an error, if so click the `add` as shown below and click `Jenkins`

![image](https://user-images.githubusercontent.com/129314018/235700005-71b70e29-4420-4a4e-ac09-6b71c419f3ed.png)

10. A pop up should appear, and the ssh keys we created before hand, using GitBash, navigate to `ssh` folder and do `cat <private-key>` and copy the private key.
11. We then click the SSH dropdown on `Kind` and enter the key-name on Username, so they match
12. On `private key` click `enter directly` and paste your private key in the box as shown below, and press add.

![image](https://user-images.githubusercontent.com/129314018/235700561-3e5456a1-9277-4583-966f-7a451112aecf.png)

13. On branches to build, change `master` to `main` as that is the branch we are using

![image](https://user-images.githubusercontent.com/129314018/235702105-133d4640-edde-4ede-845a-13e4414f9e6d.png)

14. On Build environment, check the `provide node & npm ...` as shown below

![image](https://user-images.githubusercontent.com/129314018/235702267-97a23956-71c9-4517-90d2-d43933c4894d.png)

15. Scrolling down, `add build step` then click execute shell.

![image](https://user-images.githubusercontent.com/129314018/235702442-b3d41f78-436e-4449-bb11-99f7c14f81ee.png)

16. In `execute shell` enter (and save after) : 
```
cd app
npm install
npm test
```

17. Click `Build now` and wait for the build history to update, when the new build shows up, click the dropdown as shown below, and click `console output`

![image](https://user-images.githubusercontent.com/129314018/235704026-6c4ccbd1-c692-41eb-9598-e9ebcde7a55e.png)

18. After clicking `console output` we can scroll down to see checks passed, and we can see what it can look like for a succesful test, shown below.

![image](https://user-images.githubusercontent.com/129314018/235704124-d9a22683-5a16-49d1-9cc3-f62c245a0076.png)

### Webhook creation

1. Create webhook for Jenkins/endpoint
2. create a webhook in github for repo where we have app code
3. test webhook - testing status code 200
4. make change to github readme and commit change

![image](https://user-images.githubusercontent.com/129314018/235880570-4303e7c6-faed-4df1-bdd8-ee3a5bbecd2e.png)

![image](https://user-images.githubusercontent.com/129314018/235880905-2b521219-6731-44aa-9d15-f685e2f00783.png)

- save after checking box as shown below

![image](https://user-images.githubusercontent.com/129314018/235882064-94742946-8745-4acc-8865-1d9d624a4c55.png)

### On local machine - Jenkins 

1. On GitBash, we navigate to our directory linked to repo.
2. `git pull` to pull changes from the GitHub changes we made earlier
3. `nano README.md` make some changes to test it
4. `git add .` then `git commit -m "xxxx"` then `git push` to push it to the GitHub
5. Check Jenkins after pushing the new changes, and it should show you a new build being deployed as shown below.

![image](https://user-images.githubusercontent.com/129314018/235890506-64845f78-e9dc-4166-84db-cd6e5df40586.png)

### TAS - MERGE - NEW BRANCH - JENKINS

1. Create new job called mohammad-ci-merge, we do this by selecting our old mohammad-ci template
2. create dev branch on local host and make change to readme
3. we do this by `git branch dev` then `git checkout dev`
4. push to github which should trigger job
5. if test passed, merge code to main branch
6. we now switch to our target branch which is `main` by `git checkout main`
7. We then `git merge dev` which should merge our dev branch into the main branch
8. `git add .` then `git commit -m "xxxx"` then `git push origin main` and it should push the new merged branch, and we can test this by checking Jenkins, and it should show a new build being deployed.
9. Like before, it should show the updated code/readme on your GitHub

#### Automating this

1. Make a new job, we call it `mohammad-ci-merge-dev` and we as before create a template from `mohammad-ci-merge` and scroll down to source code management, additional behaviours and do as below. 

![image](https://user-images.githubusercontent.com/129314018/235922617-a80f8139-a5a1-4582-bbdb-af5410d17680.png)

2. On post build actions select git publisher, and select as shown below.

![image](https://user-images.githubusercontent.com/129314018/235922782-0a335a5d-1f90-4ddb-82d7-d04342116821.png)

3. Go to our `mohammad-ci-merge` job, scroll all the way down to post build actions and select post-build actions like below, and select the job we just created.

![image](https://user-images.githubusercontent.com/129314018/235923005-8055aa4b-bb93-4d96-8af7-04fb1e943741.png)

4. Save and we test as before, on GitBash, change code/readme, `git add .` -> `git commit -m "xxx"` -> `git push origin dev`.
5. once done, it will deploy it on `mohammad-ci-merge` which will update it on the `dev` branch and as we did post build actions, this triggers the `mohammad-ci-merge-dev` job to deploy, this will merge the `dev` branch with the new changes with the `main branch`
6. Test this by checking the repo on GitHub and navigating between both branches, and see if the changes made on the `dev` branch are the same as the `main` branch



## create 3rd job to push code to production

1. Launch instance from previous AMIs, we use our app AMI `ami-06042513c73c0c668`, and configure security groups as shown below 

![image](https://user-images.githubusercontent.com/129314018/235996432-b02016e9-74fb-4c81-95d5-5edb8308f906.png)

2. Launch Instance


3. Create a new job to deploy our sparta app we use `mohammad-sparta-app`
4. Configure the job as seen in the screenshots below in order

![image](https://user-images.githubusercontent.com/129314018/235975825-dc766cd1-286d-4d04-8bd0-70f0e482238e.png)
![image](https://user-images.githubusercontent.com/129314018/235975895-b99b6813-2f29-4127-8f31-1c7606c1d777.png)
![image](https://user-images.githubusercontent.com/129314018/235975976-aa5775be-9955-4d2f-b63f-b9b56ec5b7ce.png)
![image](https://user-images.githubusercontent.com/129314018/235976048-bb3fc050-3fa3-4403-86fa-582337ba1a0d.png)

5. As seen above, in the execute shell enter these commands changing the `ip` to your Public ipv4 addres found on 
EC2 instance.

```
scp -v -r -o StrictHostKeyChecking=no app/ ubuntu@<my-ip>:/home/ubuntu/
ssh -A -o StrictHostKeyChecking=no ubuntu@<my-ip> <<EOF
#sudo apt install clear#

cd app

#sudo npm install pm2 -g
# pm2 kill
nohup node app.js > /dev/null 2>&1 &
```
6. Save changes and build to test the deployment of the app
7. Type public ip with `:3000` at the end to see if the app has deployed

![image](https://user-images.githubusercontent.com/129314018/235997171-ba85ba80-e607-45e1-b950-3fd6b7b675cc.png)


## Building Jenkins server o AWS

- create ec2 on aws with correc linux distro (18.04)
- set up jenkins master first - use t2.medium
- rebuild pipeline
- once it works
- build testing node (agent node) - needs to launch when needed  -t2.micro
- restrict build on agent/testing node
- dependencies for Jenkins Master and agent nodes
  - node env
  - we need linux env
  - ubuntu 18.04 lts
- DO NOT USE APP OR DB AMI
- sg group for jenkins master and agent node
- HINT : Check github ports - connections IPs
- once working - complete documentation for end to end automation





























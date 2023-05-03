# CICD, Jenkins, CDE (git pull local test)

# code added from git branch - dev

# automating with dev branch to merge

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
4. mke change to github readme and commit change

![image](https://user-images.githubusercontent.com/129314018/235880570-4303e7c6-faed-4df1-bdd8-ee3a5bbecd2e.png)

![image](https://user-images.githubusercontent.com/129314018/235880905-2b521219-6731-44aa-9d15-f685e2f00783.png)

- save after checking box
![image](https://user-images.githubusercontent.com/129314018/235882064-94742946-8745-4acc-8865-1d9d624a4c55.png)













# Steps

* Generate new ssh key in .ssh folder
* copy key to github repo where you have app code - repo -> settings
* make a change to code and push to github
* generate new ssh key called mohammad-jenkins, copy public key to repo and private key to jenkins - 
* set up the webhook in github with jenkins end point - jenkins ip (includes port)
* 


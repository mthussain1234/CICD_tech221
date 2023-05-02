# CICD, Jenkins, CDE

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

![image](https://user-images.githubusercontent.com/129314018/235678316-948b83c1-5034-4647-afc6-eb37d9e21a99.png)

![image](https://user-images.githubusercontent.com/129314018/235678494-d680aede-a141-4cfb-94be-fbec8ba44806.png)

![image](https://user-images.githubusercontent.com/129314018/235699049-7c83fc8f-8cb6-4b6a-acb5-de1ec5cb4618.png)

![image](https://user-images.githubusercontent.com/129314018/235699355-d16b50b9-4c53-4e23-9c72-5c3dccc3012b.png)

![image](https://user-images.githubusercontent.com/129314018/235699909-30280628-5d2a-4579-8d07-663401c952ea.png)

![image](https://user-images.githubusercontent.com/129314018/235700005-71b70e29-4420-4a4e-ac09-6b71c419f3ed.png)

![image](https://user-images.githubusercontent.com/129314018/235700561-3e5456a1-9277-4583-966f-7a451112aecf.png)

![image](https://user-images.githubusercontent.com/129314018/235702105-133d4640-edde-4ede-845a-13e4414f9e6d.png)

![image](https://user-images.githubusercontent.com/129314018/235702267-97a23956-71c9-4517-90d2-d43933c4894d.png)

![image](https://user-images.githubusercontent.com/129314018/235702442-b3d41f78-436e-4449-bb11-99f7c14f81ee.png)

In `execute shell` enter : 
```
cd app
npm install
npm test
```

Then save.









# Steps

* Generate new ssh key in .ssh folder
* copy key to github repo where you have app code - repo -> settings
* make a change to code and push to github
* generate new ssh key called mohammad-jenkins, copy public key to repo and private key to jenkins - 
* set up the webhook in github with jenkins end point - jenkins ip (includes port)
* 


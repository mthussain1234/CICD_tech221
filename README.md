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

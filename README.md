# Deploy ARM template using Azure Devops

## Introduction

In this project, I will deploy ARM template using Azure Devops. I will use Continous Integration of ARM template with Azure Pipelines. 

## prepare your project

In This project we have ARM template and Azure DevOps organization ready for creating the pipeline. The following steps show how to make sure you're ready:

You have an Azure DevOps organization. If you don't have one, create one for free. If your team already has an Azure DevOps organization, make sure you're an administrator of the Azure DevOps project that you want to use.

![](https://github.com/AbiVavilala/Deploy-ARM-with-Azure-DevOps/blob/main/images/ProjectPic.png)

 You need to import your gitHub  repo to Azure Repos.

![](https://github.com/AbiVavilala/Deploy-ARM-with-Azure-DevOps/blob/main/images/Importrepo.png)

You have an ARM template that defines the infrastructure for your project. I have ARM tepmate in the repo. the template will create storage account. Virtual Network, Public IP, NSG, subnet, NIC, VM.

## Create Pipeline

I will add new pipeline
![](https://github.com/AbiVavilala/Deploy-ARM-with-Azure-DevOps/blob/main/images/createpipeline.png)

I will specify my code is stored in Azure Repos git.
![](https://github.com/AbiVavilala/Deploy-ARM-with-Azure-DevOps/blob/main/images/pipelinerepo.png)

select the repo we just imported to Azure Repos Git in this project and then select starter pipeline.
![](https://github.com/AbiVavilala/Deploy-ARM-with-Azure-DevOps/blob/main/images/starterpipeline.png)

add two tasks to the pipeline. first one is copying files and second one is publish artifact. please find the code below.

```
# Starter pipeline
# Start with a minimal pipeline that you can customize to build and deploy your code.
# Add steps that build, run tests, deploy, and more:
# https://aka.ms/yaml

trigger:
- main

pool:
  vmImage: ubuntu-latest

steps:
- script: echo Hello, world!
  displayName: 'Run a one-line script'

- script: |
    echo Add other tasks to build, test, and deploy your project.
    echo See https://aka.ms/yaml
  displayName: 'Run a multi-line script'
- task: CopyFiles@2
  inputs:
    SourceFolder: '$(agent.builddirectory)'
    Contents: '**'
    TargetFolder: '$(build.artifactstagingdirectory)'

- task: PublishBuildArtifacts@1
  inputs:
    PathtoPublish: '$(Build.ArtifactStagingDirectory)'
    ArtifactName: 'storagedrop'
    publishLocation: 'Container'
```
now lets review the pipeline before we run

![](https://github.com/AbiVavilala/Deploy-ARM-with-Azure-DevOps/blob/main/images/Reviewpipeline.png)

now lets save and run the pipleline
![](https://github.com/AbiVavilala/Deploy-ARM-with-Azure-DevOps/blob/main/images/saveandrunpipeline.png)

pipeline has run succesfully.

![](https://github.com/AbiVavilala/Deploy-ARM-with-Azure-DevOps/blob/main/images/pipelinerunsucess.png)

this pipeline has produced an atrifact. we will use this artifact to create resources mentioned in our template. In our ARM template we are creating a storage account.


let's create a release pipeline and use artifact we got from our build process.
![](https://github.com/AbiVavilala/Deploy-ARM-with-Azure-DevOps/blob/main/images/releasepipeline.png)

for the release pipeline let's select empty job. 
![](https://github.com/AbiVavilala/Deploy-ARM-with-Azure-DevOps/blob/main/images/emptyjob.png)


now let's add ARM template to the task
![](https://github.com/AbiVavilala/Deploy-ARM-with-Azure-DevOps/blob/main/images/addarmtemplate.png)

let's fill in the details so that resource get's created.
![](https://github.com/AbiVavilala/Deploy-ARM-with-Azure-DevOps/blob/main/images/releasepipeline1.png)

we need to integrate our build into the release pipeline. the artifact generated will be added.

![](https://github.com/AbiVavilala/Deploy-ARM-with-Azure-DevOps/blob/main/images/releasepipeline2.png)

I added artifact generated into the release pipeline.
![](https://github.com/AbiVavilala/Deploy-ARM-with-Azure-DevOps/blob/main/images/releasepipeline3.png)

now let's add task into the arm template added.
![](https://github.com/AbiVavilala/Deploy-ARM-with-Azure-DevOps/blob/main/images/releasepipeline4.png)

let's create the release.
![](https://github.com/AbiVavilala/Deploy-ARM-with-Azure-DevOps/blob/main/images/createrelease.png)


you can see that resource group is created and storage account is created. please see the logs image below.

![](https://github.com/AbiVavilala/Deploy-ARM-with-Azure-DevOps/blob/main/images/releasepipelinesuccess.png)

I checked in Azure that new resource group is created and storage account is created.
![](https://github.com/AbiVavilala/Deploy-ARM-with-Azure-DevOps/blob/main/images/resourcecreated.png)


## I will add continous deployment and release to the pipeline
now we will add trigger to the release pipeline. this will make sure any changes to the repo will create the new build and will trigger to release pipeline. this will create new resources.

I will enable continoys deployment on the pipleline. when I make change to the repo the pipeline will get executed and resource will be created. Edit the release pipeline

![](https://github.com/AbiVavilala/Deploy-ARM-with-Azure-DevOps/blob/main/ER1.png)

click on the trigger that will let me enable continous deployment

![](https://github.com/AbiVavilala/Deploy-ARM-with-Azure-DevOps/blob/main/ER2.png)

enable create a new release when new build is available and enable pull trigger
![](https://github.com/AbiVavilala/Deploy-ARM-with-Azure-DevOps/blob/main/ER3.png)

Now save the settings. We have enabled continous deployment. 
![](https://github.com/AbiVavilala/Deploy-ARM-with-Azure-DevOps/blob/main/ER4.png)

Now we will edit the repo. I will add more resources in the template. I will add storage account, public ip, Virtual network, subnet, NSG, NIC, Disk and Virtual machine.

![](https://github.com/AbiVavilala/Deploy-ARM-with-Azure-DevOps/blob/main/addresources.png)

commit the changes and you will see new bulild will be created and that will trigger release pipeline.
![](https://github.com/AbiVavilala/Deploy-ARM-with-Azure-DevOps/blob/main/newbuild.png)

new release will trigger too.
![](https://github.com/AbiVavilala/Deploy-ARM-with-Azure-DevOps/blob/main/newrelease.png)

new release will run and once it's successful resource will be created.
![](https://github.com/AbiVavilala/Deploy-ARM-with-Azure-DevOps/blob/main/newreleasesucess.png)

I will check in Azure and all the resources are created.

![](https://github.com/AbiVavilala/Deploy-ARM-with-Azure-DevOps/blob/main/newresourcecreated.png)

This is how you can deploy ARM template with Azure Devops. you can enabel contionus deployment to make sure when you add resource to the template they get created.











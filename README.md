# Deploy ARM template using Azure Devops

## Introduction

In this project, I will deploy ARM template using Azure Devops. I will use Continous Integration of ARM template with Azure Pipelines. 

## prepare your project

In This project we have ARM template and Azure DevOps organization ready for creating the pipeline. The following steps show how to make sure you're ready:

You have an Azure DevOps organization. If you don't have one, create one for free. If your team already has an Azure DevOps organization, make sure you're an administrator of the Azure DevOps project that you want to use.


You've configured a service connection to your Azure subscription. The tasks in the pipeline execute under the identity of the service principal. For steps to create the connection, see Create a DevOps project.

You have an ARM template that defines the infrastructure for your project. I have ARM tepmate in the repo. the template will create storage account. Virtual Network, Public IP, NSG, subnet, NIC, VM.

## Create Pipeline
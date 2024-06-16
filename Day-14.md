# DEPLOY VOTING APPLICATION USING AZURE DEVOPS

REF LINK : ``` https://github.com/dockersamples/example-voting-app ```

## START AZURE DEVOPS
``` https://azure.microsoft.com/en-us/products/devops ```
- START FREE

### USES :
- PYTHON
- NODE JS
- .NET

![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/69682eb8-f583-4475-ae9d-8f7251b998d0)
- THIS IS .NET, PYTHON BASED WEB APPLICATION
- USING IN MEMORY DATABASE REDIS
- VOTES ARE STORED BY .NET TO POSTGRES DB
- RESULT APP - Written in NODE JS

## Application flow
- When user votes information is stored in a IN-MEMORY DATABASE REDIS
- .NET app takes that information can stores in POSTGRES sql 
- RESULT -- Will reads the data from the POSTGRES 

- ![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/35eaf1aa-f798-46bf-809a-9b6b6678008e)

steps:
--
- PROJECT CODE : https://github.com/dockersamples/example-voting-app/tree/main

- ![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/da6dad0b-b40f-4a25-af0c-274947930d6b)

- Does not requried any auth be'coz it is public repo
- Set main branch - ![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/17ca05ea-0520-417b-9a2b-0d3807c79746)
- ![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/33c6bf77-2581-45e7-90d9-da6cc8823fe1)
- Set as default branch
- Create a REGISTRY - ![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/319df9b2-92e9-43d6-acd9-b8510b74544b)
- Before that create a Resource group as well.
- ![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/ec17dc3f-ff79-4046-bcc4-9a7f651b0cf9)
- ![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/15cb6798-b2ee-4875-b7f1-9335c66d2a9b)
- Here we have CONFIGURE YOUR PIPELINE Option which means - It will have a template based on that, For this demo we are deploying to Build and push to Azure container registry
- ![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/f32f0c2b-7b9b-4d55-9ebb-0b239aa007dc)
- validate and configure
- ![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/b0d914b2-a3fa-47c9-84c0-9d44024c1a7e)
- ![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/73e7ae0c-e935-4895-bbda-3e53148d8592)
- Here we are only including result folder -- which is one microservice
```
variables:
  # Container registry service connection established during pipeline creation
  dockerRegistryServiceConnection: 'DFHGDFG-9165-4e1d-DFSDF-a5e01ff2df7e'
  imageRepository: 'resultapp'
  containerRegistry: 'pavankumar0097.azurecr.io'
  dockerfilePath: '$(Build.SourcesDirectory)/result/Dockerfile'
  tag: '$(Build.BuildId)'
```
- Here dockerRegistryServiceConnection is connection between container registery and pipeline
- We can use dockerhub or any other Registry as well -- store the password in ENVIRONMENTS section and read it from their.
- As we are creating 3 pipelines for 3 microservices, We are giving imageRepository name -- differently
- check dockerfilepath as well

```
 # Agent VM image name
  vmImageName: 'ubuntu-latest'
```
- Here Agent is created BY AZURE AND MANGAED BY AZURE
- BUT FOR FREE TRIAL WE DON'T AN OPTION TO CREATE
- WE HAVE TO CREATE OUR OWN AGENT

```
# User Agent VM image name
  pool:
    name: 'azureagent'
```
- THIS IS WE HAVE CREAT A VM TO RUN THE PIPELINE
- USER AGENT


- ![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/01b75593-129a-4734-976a-3211631325d5)
- HERE TASK IS USED TO WORK ON DIFFERENT TASKS LIKE IF WE WANT TO WRITE SHELL SCRIPT WE HAVE WRIET IN THE TASK LIKE SHELL AS OF NOW WE NEED TO CREATE A DOCKER, SO WE ARE WRITING DOCKER AND TAKING LATEST VERSION

- ![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/4a9e3f66-0da0-48eb-ad0c-1037a6479bf5)
- IF WE ARE USING DOCKER REGISTRY -- THEN WE HAVE TO USE DOCKER LOGIN, BUIL & PUSH, AS OF NOW WE ARE USING AUZRE REGISTRY CONTAINER. SO no need of any login auth for this.

PIPELINE:
--
```
# Docker
# Build and push an image to Azure Container Registry
# https://docs.microsoft.com/azure/devops/pipelines/languages/docker

trigger:
 paths:
   include:
     - result/*

resources:
- repo: self

variables:
  # Container registry service connection established during pipeline creation
  dockerRegistryServiceConnection: '598c3015-9165-4e1d-8ed9-a5e01ff2df7e'
  imageRepository: 'resultapp'
  containerRegistry: 'pavankumar0077.azurecr.io'
  dockerfilePath: '$(Build.SourcesDirectory)/result/Dockerfile'
  tag: '$(Build.BuildId)'

  # User Agent VM image name
  pool:
    name: 'azureagent'

stages:
- stage: Build
  displayName: Build
  jobs:
  - job: Build
    displayName: Build
    steps:
    - task: Docker@2
      displayName: Build an image to container registry
      inputs:
        containerRegistry: '$(dockerRegistryServiceConnection)'
        repository: '$(imageRepository)'
        command: 'build'
        Dockerfile: 'result/Dockerfile'
        tags: '$(tag)'
        
- stage: Push
  displayName: Push
  jobs:
  - job: Push
    displayName: Push
    steps:
    - task: Docker@2
      displayName: Push an image to container registry
      inputs:
        containerRegistry: '$(dockerRegistryServiceConnection)'
        repository: '$(imageRepository)'
        command: 'push'
        tags: '$(tag)'
```
- Save & Run
- IT WILL FAIL BECAUSE OF NO VM AGENT IS PRESENT AT THIS TIME

- ![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/ba2bc4da-f70f-48d5-88a9-5a87cc0d3abd)
- WE NEED CREATE AN VM AGENT NOW.

## NOTE : ``` https://learn.microsoft.com/en-us/azure/devops/pipelines/agents/linux-agent?view=azure-devops ```

- READ THE DOCS CAREFULLY FOR AGENT CONFIGURATIONS

- ![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/e7e10a50-5715-425e-aacd-73edb81a315b)


- ![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/6a713432-fb16-41f0-986f-5878dced7777)

- Create an agent and copy the code and install, agent will be installed
- Login to VM and install the steps provided
- ``` https://learn.microsoft.com/en-us/azure/devops/pipelines/agents/linux-agent?view=azure-devops ```

- ![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/5a9a9b58-9748-4b9a-b064-9e77d80dfe5f)


- To CREATE PAT -- ![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/f15e45e9-2e7d-40fa-a682-d7d2f1044a0c)
- GO TO PAT -- create pat
- IN VM

- SERVER ULR : ``` https://learn.microsoft.com/en-us/azure/devops/pipelines/agents/linux-agent?view=azure-devops ```
- ![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/29a68a0f-703a-434e-becc-7e48f5e41272)
- ORGANIZATION NAME
```
azureuser@azureagent:~$ mkdir myagent && cd myagent

azureuser@azureagent:~/myagent$ wget https://vstsagentpackage.azureedge.net/agent/3.239.1/vsts-agent-linux-x64-3.239.1.tar.gz

azureuser@azureagent:~/myagent$ tar zxvf vsts-agent-linux-x64-3.239.1.tar.gz

azureuser@azureagent:~/myagent$ ./config.sh

azureuser@azureagent:~/myagent$ ./config.sh

  ___                      ______ _            _ _
 / _ \                     | ___ (_)          | (_)
/ /_\ \_____   _ _ __ ___  | |_/ /_ _ __   ___| |_ _ __   ___  ___
|  _  |_  / | | | '__/ _ \ |  __/| | '_ \ / _ \ | | '_ \ / _ \/ __|
| | | |/ /| |_| | | |  __/ | |   | | |_) |  __/ | | | | |  __/\__ \
\_| |_/___|\__,_|_|  \___| \_|   |_| .__/ \___|_|_|_| |_|\___||___/
                                   | |
        agent v3.239.1             |_|          (commit 7feec17)

Error reported in diagnostic logs. Please examine the log for more details.
    - /home/azureuser/myagent/_diag/Agent_20240516-132717-utc.log
Cannot configure the agent because it is already configured. To reconfigure the agent, run 'config.cmd remove' or './config.sh remove' first.
- ![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/6d28ffa9-51b7-45db-bdb2-3313c9d68cbf)

azureuser@azureagent:~/myagent$ ./run.sh
```
![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/f91082ac-1c73-41ac-85ac-5f665d24e15f)

## IN THE AGENT WE NEED DOCKER AS WELL
```
 sudo apt install docker.io
 sudo usermod -aG docker azureuser
 sudo systemctl restart docker
```

### TEMPLATE INFO :
- Triggers : Trigger are basically used when should this pipeline trigger ( When push or PR or commit happends this should be triggered automatically )
- PATH BASED TRIGGERS :  THIS TRIGGERS ARE USED TO TRIGGER A SPECIFIC FOLDER CHANGES, THAT WE HAVE MENTIONED.
- STAGES : Test, Build, Image push, Deployment
- IN STAGES -- WE HAVE JOBS -- IN JOBS WE HAVE STEPS
- VARIABLES - TO STORE THE VARIABLES

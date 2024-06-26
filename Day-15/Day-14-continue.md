## CREATE KUBERNETES CLUSTER



- ![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/7eab7754-66a9-41e6-9a4b-5bb1e4caab42)
- AZURE COMES WITH PRE-CONFIG ENV FOR K8S - DEV/TEST IS FOR TESTING AND DEV
- WE HAVE PROD CLUSTER AS WELL
- WE HAVE ADVANCED FREATURE LIKE AUTO-UPDATE
- WE HAVE TO USE AGENTPOOL BE'COZ AKS IS THE MANAGED KUBERNETES SERVICE BUT TO RUN YOUR WORKLOAD OR SYSTEM WORKLOADS PODS & APPLICATIONS WE NEED A VIRTUAL MACHINE THIS POOL ACTUALLY PROVIDES THE RESOUCES FOR THE COMPUTE
- NODE POOL IS THE POOL OF THE VM's FOR OUR CLUSTER

- ![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/4fd4b94b-b70f-48d7-af8b-a0ed25aa0af4)

- BASICALLY WE WE SELECT SCALE METHOD AS AUTO-SCALE WE WILL BE USING VIRUTAL MACHINE SCALE SET FEATURE OR AUTOSCALING FEATURE OF THE AZURE WHERE THE KUBERENTES CLUSTERS CAN BE SCALED AUTOMATICALLY
- THAT MEANS IF YOUR KUBERNETES CLUSTER IS SETUPED TO 1 NODE INTIALLY WHEN THERE IS A MORE TRAFFIC KUBERNETES CLUSTER OR YOUR APPLICATION THEN THE VM SCALE SET AUTOMATICALLY BASED ON THE MIN AND MAC NODE COUNT.

### NOTE : KEEP THE NODE COUNT AS LIMIT THE REASON IS THAT IN SOME CASES WHEN OUR FIREWALL IS BREACHED AND IF SOME HACKER IS ABLE TO ACCESS YOUR CLUSTER YOUR APPLICATIONS THEN THEY CAN PERFORM SOME DENIAL OF SERVICE ATTACK 
### AND IF WE ARE SCALING IT TO SOME NUMBER LIKE 10 20 100 YOUR APPLICATION WITH WITHSTAND THE HACKER ATTACK THE DENAL OF SERVICE ATTACK (DOS) WHERE HACKER IS SENDING 1 MINILLION REQUEST TO YOUR APPLICATION, APPLICATION 
### MIGHT WITH STAND BE'COZ YOU HAVE ENABLED AUTO SCALING, BUT THE PROBLEM IS YOU WILL CHARGED VERY HEAVILY BY THE AZURE.

- WE HAVE ALSO ADD MULTIPLE WORK LOADS 1 FOR KUBERNETES SYSTEM WORKLOADS
- 1 FOR APPLICAITON WORKLOADS.


- ![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/9dd21bc8-dffd-4010-b508-77511cd819d9)

CI CD FLOW
--

- ![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/c9549776-700e-4e22-8c95-8ab237829765)


- ![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/3186f5d4-c2c5-4041-9f74-576998d6c0fb)

THINGS WE HAVE TO DO IN K8S CLUSTER
--
Step 1 : Login to K8s cluster
Step 2 : Install ARGO CD IN K8S CLUSTER
STEP 3 : CONFIGURE ARGOCD
STEP 4 : UPDATE SHELL SCRIPT -- REPO -- NEW IMAGE IN ACS


### Step 1 : Login to K8s cluster
```
az aks get-credentials --name azuredevops --overwrite-existing --resource-group azurecicd

 Examples from AI knowledge base:
az aks get-credentials --name MyManagedCluster --overwrite-existing --resource-group MyResourceGroup
Get access credentials for a managed Kubernetes cluster. (autogenerated)

az aks get-credentials --admin --name MyManagedCluster --resource-group MyResourceGroup
Get access credentials for a managed Kubernetes cluster. (autogenerated)

az aks list
List managed Kubernetes clusters. (autogenerated)

C:\Users\dasar>kubectl version --client
Client Version: v1.30.0
Kustomize Version: v5.0.4-0.20230601165947-6ce0bf390ce3

C:\Users\dasar>kubectl get pods
No resources found in default namespace.

C:\Users\dasar>kubectl get pods -A
NAMESPACE     NAME                                  READY   STATUS    RESTARTS   AGE
kube-system   azure-ip-masq-agent-pr7wc             1/1     Running   0          43m
kube-system   cloud-node-manager-vrc98              1/1     Running   0          43m
kube-system   coredns-767bfbd4fb-7n52v              1/1     Running   0          43m
kube-system   coredns-767bfbd4fb-gvmcx              1/1     Running   0          42m
kube-system   coredns-autoscaler-c6649b67c-d6m9t    1/1     Running   0          43m
kube-system   csi-azuredisk-node-zqlw7              3/3     Running   0          43m
kube-system   csi-azurefile-node-hrhdv              3/3     Running   0          43m
kube-system   konnectivity-agent-5f979667f9-n5s6d   1/1     Running   0          43m
kube-system   konnectivity-agent-5f979667f9-xssnt   1/1     Running   0          42m
kube-system   kube-proxy-grsxj                      1/1     Running   0          43m
kube-system   metrics-server-5cd44496f4-kgznz       2/2     Running   0          42m
kube-system   metrics-server-5cd44496f4-pspbw       2/2     Running   0          42m

```

Step 2 : Install ARGO CD
--
``` https://argo-cd.readthedocs.io/en/stable/getting_started/ ```

```
KUBECTL SHOULD BE INSTALLED BEFORE 

kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

kubectl create namespace argocd

kubectl get pods -n argocd
```

Step 3 : COnfigure ARGO CD
--
- CONNECT ARGO CD WITH AZURE REPO
- LOGIN TO ARGO CD

```
kubectl get pods -n argocd

kubectl get secrets -n argocd

kubectl edit secret argocd-initial-admin-secret -n argocd

echo am5ub0hqdHBvSzFMbmJ3VQ== | base64 --decode

- Here i am using windows so i am getting error while decoding -- So i am using ``` https://www.base64decode.org/ ``` to decode, If your case it will work normally use only UBUNTU machines.

- Now to access ARGO CD IN UI WE NEED TO CHANGE ARGOCD-SERVER FROM CLUSTERIP TO NODE IP, TO DO THAT

kubectl get svc -n argocd
argocd-server                             ClusterIP   10.0.194.147   <none>        80/TCP,443/TCP               6m57s

kubectl edit svc argocd-server -n argocd
argocd-server                             NodePort    10.0.194.147   <none>        80:31141/TCP,443:31894/TCP   7m48s

HERE THIS IS HTTP PROTOCOL -- 80:31141/TCP,   HTTPS PROTOCOL  -- 443:31894/TCP

- Go to the end of the file and change from CLUSTER IP TO NODEPORT -- NodePort 

- TO GET THE EXTERNAL IP
kubectl get nodes -o wide

NAME                                STATUS   ROLES   AGE   VERSION   INTERNAL-IP   EXTERNAL-IP      OS-IMAGE
 KERNEL-VERSION      CONTAINER-RUNTIME
aks-agentpool-36596996-vmss000000   Ready    agent   56m   v1.28.9   10.224.0.4    172.178.40.178   Ubuntu 22.04.4 LTS   5.15.0-1061-azure   containerd://1.7.15-1

```

### ALLOW INBOUND TRAFFIC TO ACCESS


- SEARCH FOR VMSS - SCALE SET
- GO TO SCALE SET
- GO TO INSTANCES
- CLICK ON INSTANCE
- NETWORK SETTINGS
- ![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/d9797bee-451b-4ddf-aa4f-1ed50012999d)
- HERE PORT RANGE IS ARGO CD - SERVICE HTTP PORT -- 31141

### LOGIN TO ARGO CD
- ![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/c8886246-8239-434a-9bc0-9952f795fe40)
- USERNAME : admin
- PASSWORD : DECODED INTIAL PASSWORD
- ![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/3ec8e23e-45fc-44dd-9185-e4fc3cb84fa9)

### NOW WE HAVE TO CONNECT ARGO CD WITH AZURE REPO 
- ARGO CD IS RUNNING IN K8S CLUSER
- WE EITHER NEED ACCESS TOKEN
- OR USERNAME AND PASSWORD
- CREATE ACCESS TOKEN
- ![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/68332b3a-312f-4e4a-a730-6b10783d1329)
- WE NEED TO CREATE A ACCESS TOKEN WITH READ PERMISSIONS ONLY ARGO CD ONLY READS
- CLICK ON THE SETTINGS ICON AND CREATE PAT
- GO TO ARGO SETTINGS --
- ![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/15c0f2c1-f14f-4935-a471-2114a6572c1e)
-  ![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/50c91968-9e1e-4726-bdb9-fe350dd9a9d7)
- IN THE REPO URL -- WE NEED TO REMOVE USERNAME FOR EXAMPLE
- https://dasarepavan007@dev.azure.com/dasarepavan007/Voting-app/_git/Voting-app
- INSTEAD OF dasarepavan007 -- REPLACE WITH PAT -- jlqkdfa6hyhsbawynb5SDFDSFsdfsdfdsdfasfsdfsddf

- FINALLY IT WILL LIKE THIS 

- https://sdfdsfasdfjkasdfjkasjkfdlasjdlfaf@dev.azure.com/dasarepavan007/Voting-app/_git/Voting-app

- ![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/87a7581a-df13-4aab-ab4f-4ffdadc2b9a2)

- CLICK ON CONNECT 

- ![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/1945cf72-d4b8-4d40-8a5c-6c1fb857c7ee)

CREATE APP IN ARGO CD
--
- ![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/e0df124a-f019-4850-8122-41351d0488d1)
- ![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/e80be610-9a79-41df-afe3-5839e6a556e3)
- HERE k8s-specfiication is MANIFEST FILES - YAML FILES PATH IN THE REPO
- ![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/369cf4ba-2776-4733-bcb3-bfcc90e9fded)
- ![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/b753749c-8cfe-41e8-ae73-e086b0118762)
- HERE WE DEPLOYED ALL THE MANIFEST FILES -- MICROSERVICE APPLICATION -- 9 APPS


NOW CREATE THE UPDATE SCRIPT IN THE AZURE PIPELINES
--
- WE ARE WRITING UPDATE SCRIPT TO PICK UP CHANGES FROM ACR AND UPDATE BACK TO REPO - MANIFEST FILES
- ![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/2af00092-7ce6-4e6f-9b52-a09a44e2a98f)
- ![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/d1675606-9660-4842-ae6e-e9a9dfd64cec)
- WE EVEN DON'T NEED TO GO TO ACR AS WELL WE NEED TO FIND THE PATTERN HOW NEW IMAGE IS STORED IN REGISTRY LIKE TAGS
- ONLY TAG WILL BE CHANGED REST ALL THINGS ARE SAME.
- TAG NUMBER IS -- BUILD NUMBER FROM THE PIPELINES SCRIPT
- WE NEED TO CREATE THE SHELL SCRIPT TO UPDATE THE TAG ANY CHANGES IN THE TAG -- BUILD ID - BUMBER ID IS DYNAMIC

SHELL SCRIPT
--
```
#!/bin/bash

set -x

# Set the repository URL
REPO_URL="https://<ACCESS-TOKEN>@dev.azure.com/<AZURE-DEVOPS-ORG-NAME>/voting-app/_git/voting-app"

# Clone the git repository into the /tmp directory
git clone "$REPO_URL" /tmp/temp_repo

# Navigate into the cloned repository directory
cd /tmp/temp_repo

# Make changes to the Kubernetes manifest file(s)
# For example, let's say you want to change the image tag in a deployment.yaml file
sed -i "s|image:.*|image: <ACR-REGISTRY-NAME>/$2:$3|g" k8s-specifications/$1-deployment.yaml

# Add the modified files
git add .

# Commit the changes
git commit -m "Update Kubernetes manifest"

# Push the changes back to the repository
git push

# Cleanup: remove the temporary directory
rm -rf /tmp/temp_repo
```
- HERE $1 IS WILL LIKE VOTE, RESULT OR WORKER, DB like that -- WHCIH MICROSERVICE WE ARE USING -- IT DEFINEDS
- $2 - REPO NAME -- like voting app, result app
- $3 -- BUILD TAG
  
- ![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/10e99a00-19c0-4fd6-bcc3-9497a18d2601)
- COPY SCRIPT HERE
- THE ACCESS TOKEN WILL BE STORED IN ENV AND READ FROM THE COMMAND LINE ARGUMENT
- ![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/ac1a43ec-4253-45bd-8e00-4bc8eacce9d7)
- UPDATE IN THE AZURE PIPELINES SCRIPT
- ![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/e93d0d61-5be6-463f-adc0-24862651efbb)
```
- stage: Upate
  displayName: Update
  jobs:
  - job: Update
    displayName: Update
    steps:
    - task: ShellScript@2
      inputs:
        scriptPath: 'scripts/updateK8sManifests.sh'
        args: 'vote $imageRepository $tag'
```
- NOW NEW IMAGE WILL BE CERATED AND ARGOCD WILL CHECK FOR THE CHANGES.

## NOTE : ARGOCD BYDEFAULT CHECK FOR THE CHANGES FOR 3 MINUTES, TO GET THE CHANGES FAST LIKE TO SYNC FAST WE NEED TO USE.

## REF LINK : ``` https://argo-cd.readthedocs.io/en/stable/operator-manual/upgrading/2.0-2.1/#replacing-app-resync-flag-with-timeoutreconciliation-setting ```

```
kubectl edit cm argocd-cm -n argocd
```

```
# Please edit the object below. Lines beginning with a '#' will be ignored,
# and an empty file will abort the edit. If an error occurs while saving this file will be
# reopened with the relevant failures.
#
apiVersion: v1
kind: ConfigMap
metadata:
  annotations:
    kubectl.kubernetes.io/last-applied-configuration: |
      {"apiVersion":"v1","kind":"ConfigMap","metadata":{"annotations":{},"labels":{"app.kubernetes.io/name":"argocd-cm","app.kubernetes.io/part-of":"argocd"},"name":"argocd-cm","namespace":"argocd"}}
  creationTimestamp: "2024-06-17T05:52:50Z"
  labels:
    app.kubernetes.io/name: argocd-cm
    app.kubernetes.io/part-of: argocd
  name: argocd-cm
  namespace: argocd
  resourceVersion: "4121"
  uid: cabbb1d9-5bdd-4113-a1ff-3aa9acebf984
data:
  timeout.reconciliation: 10s

```











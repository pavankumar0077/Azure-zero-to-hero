# APPLICATION IS DEVELOPED WITH DIFFERENT PROGRAMMING LANGUAGES 
# MULITPLE MICROSERVICES WITH DIFFERENT DATABASE.

- FOR IMAGES -- IT IS USING MYSQL
- FOR USER DATA -- IT IS USING MONGO DB

![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/aa2772fd-a8cc-402c-aa77-db2df33c9dce)

![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/b9c8848b-5902-4302-bdc5-393a6766de9a)

- HERE REDIS IS CREATED AS STATEFUL SET, TO GET THE PERSISTENT MEMORY
- BE'COZ REDIS GIVES THE FASTEST RETRIVAL OF DATA
- IF THIS IS LOST THEN DATA MAY LOSE.

![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/013aa5c7-e5f8-46f8-a51d-ecd58ab29769)

![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/f757632b-b168-4382-bbab-62d7eef477be)

## HELM CHARTS
- CHART.YAML
- TEMPALTE FOLDER
- VALUES.YAML

![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/1d80a8b6-2028-45c8-b4e4-a8637844ea54)

### WE CAN DIRECTLY DEPLOY USING MANIFEST FILES, USING KUBECTL APPLY -F ..YAML FILE.
- IF ORGANIZATIONS WE HAVE DEV, STAGE AND PROD
- FOR EVERY ENV WE HAVE SOME PARAMETERS CHANGES.
- LETS SAY FOR DEV ENV, EACH POD SHOULD NOT CROSS 512 MB RAM OR 1 CPU
- IN PROD ENV, IT CAN BE 1 GB RAM AND 2 CPU'S
- THERE CAN BE PARAMETERS THAT ARE DYNAMIC 
- IF WE MAINTAIN ONLY MANIFEST FILES AND IF WE HAVE 3 ENV'S THEN WE HAVE TO CREATE 30 MANIFEST FILES, IF WE HAVE 10 MICROSERVICES.

### IF WE USE HELM CHARTS
- INSIDE THE TEMPLATE FOLDER WE CAN DUMP  ALL THE MANIFEST FILES HERE
- AND CHANGE THE VALUES OF IT IN THE VALUES.YAML FILE
- PARAMETERS THAT WE WANT TO UPDATE IT, WE CAN UPDATE USING VALUES.YAML FILE.
- THE VALUES.YAML TAKES ALL THE VALUES AND UPDATE THEM IN THE TEMPLATES.YAML FILE.
- CHART.YAML IS TO UNDERSTAND THE META DATA OF THE CHART OR TO UNDERSTAND THE VERSION OF THE CHAT IN FUTURE.
- TEMPLATES FOLDER -- IS CALLED JINJA TEMPLATES

## NOTE : WE ARE DEPLOYING THE HELMS CHARTS THAT ARE AVAIABLE IN AKS -- HELM -- FOLDER V3 VERSION

PV in the project
--

![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/cb0b6252-261e-4c6f-9b1c-15ae65c3e590)

- WE ARE USING PERSISTENT VOLUME FOR REDIS IN THE PROJECT
- AZURE FIELS ( EFS )
- AZURE DISKS ( EBS )
- WE WILL DEFINED THE STORAGE VALUE IN THE PV CLAIM AND WE WE SAY DEFAULT
- THEN WE WILL GET DISK
- IF YOUR STORAGE IS ACCESS BY A SINGLE POD THEN GO WITH EBS IN AWS AND DISKS IN AZURE
- IF YOUR STORAGE ARE ACCESSED BY LOT FO PODS OR DIFFERENET CLUSTERS THEN USING EFS IN AWS AND VOLUMES IN AZURE.


```
C:\Users\dasar\pavan\three-tier-architecture-demo\AKS\helm>kubectl get pods -n robot-shop
NAME                         READY   STATUS    RESTARTS   AGE
cart-64c54bd6d7-bdcpx        1/1     Running   0          12m
catalogue-577d55dcc8-k4t2v   1/1     Running   0          12m
dispatch-886d77bc8-6cbm8     1/1     Running   0          12m
mongodb-7474db6fdf-ldsmg     1/1     Running   0          12m
mysql-5fb849d788-bm4w4       1/1     Running   0          12m
payment-6cc6b544db-jvsbq     1/1     Running   0          12m
rabbitmq-f688fb85f-mr2xb     1/1     Running   0          12m
ratings-56954c898b-xgplp     1/1     Running   0          12m
redis-0                      1/1     Running   0          12m
shipping-84c69c4c94-djrtc    1/1     Running   0          12m
user-554bfcf4f8-n6ng7        1/1     Running   0          12m
web-dc75f5d68-p7gvv          1/1     Running   0          12m

C:\Users\dasar\pavan\three-tier-architecture-demo\AKS\helm>
```

```
C:\Users\dasar\pavan\three-tier-architecture-demo\AKS\helm>kubectl get svc -n robot-shop
NAME        TYPE           CLUSTER-IP     EXTERNAL-IP      PORT(S)                       AGE
cart        ClusterIP      10.0.50.139    <none>           8080/TCP                      13m
catalogue   ClusterIP      10.0.11.117    <none>           8080/TCP                      13m
dispatch    ClusterIP      None           <none>           55555/TCP                     13m
mongodb     ClusterIP      10.0.114.18    <none>           27017/TCP                     13m
mysql       ClusterIP      10.0.113.235   <none>           3306/TCP                      13m
payment     ClusterIP      10.0.242.102   <none>           8080/TCP                      13m
rabbitmq    ClusterIP      10.0.138.218   <none>           5672/TCP,15672/TCP,4369/TCP   13m
ratings     ClusterIP      10.0.28.111    <none>           80/TCP                        13m
redis       ClusterIP      10.0.174.145   <none>           6379/TCP                      13m
shipping    ClusterIP      10.0.130.49    <none>           8080/TCP                      13m
user        ClusterIP      10.0.217.169   <none>           8080/TCP                      13m
web         LoadBalancer   10.0.207.167   52.149.168.223   8080:30494/TCP                13m

C:\Users\dasar\pavan\three-tier-architecture-demo\AKS\helm>
```
- NOW ALL THE PODS ARE RUNNING and SERVICES ARE RUNNING
- TO ACCCESS THE APPLICATION AS WE CAN USE WEB IS CREATED WITH TYPE LOADBALANCER, BUT WE CAN GO WITH THE INGRESS CONFIGURATION
- WE  CAN USE LOAD BALANCER AS WELL, BUT THE PROBLEM IS THAT WE DON'T GET THE EXTRA CAPABILITIES THAT INGRESS CONTROLLER GIVES.
- FOR EXAMPLE WITH INGRESS CONTROLLER -- WE CAN CONFIGURE WEB APPLICATION FIREWALL, PATH BASED ROUTING, HOST BASED ROUTING AND THERE ARE A LOF OF CAPABILITES THAT INGRESS OFFERS.
- WE CAN ACCESS THE APPLICATION WITH THE LOAD BALANCER IP 52.149.168.223:8080 AS WELL.
- http://52.149.168.223:8080/
- ![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/b9ffa9a3-63c2-4ec3-b4bf-e44f66e56313)
- WITH LOAD BALANCER IP
- ENABLE INGRESS CONTROLLER
- ![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/a8ff8dfc-cda1-4512-88e8-e139f5d9c6b9)
- IT IS VERY EASY IN AZURE, BUT IN AWS WE NEED TO DEPLOY THE HELM CHART FOR INGRESS CONTROLLER CONFIGURATIONS AND WE ALSO HAVE TO USE CSA DRIVER FOR EBS
- IN AZURE IT COMES NATIVELY 
- ingress controller
- kubectl apply -f ingress.yaml file which is in same folder

```
  C:\Users\dasar\pavan\three-tier-architecture-demo\AKS\helm>kubectl get ingress -n robot-shop
NAME         CLASS                       HOSTS   ADDRESS         PORTS   AGE
robot-shop   azure-application-gateway   *       48.217.210.74   80      3m56s
```
- NOW WE ARE UING INGRESS CONTROLLER IP ADDRESS
- http://48.217.210.74/ 
- PORT 80
- ![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/8912c0a9-c5b9-474d-abc4-7a4c91f817a2)
- ![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/13a360e9-7bfa-4f7d-bbcf-b6dbb0d1d9c5)

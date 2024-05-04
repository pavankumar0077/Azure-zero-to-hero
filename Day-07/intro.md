![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/eb1b94bf-0bcc-437b-9aa1-37236af75a98)

1. Resource Group
2. Virtual Network - While creating VNET we need to Enable Azure Bastion
- ![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/80eff5a2-2d3a-4561-a87c-834a323293fa)
- NOTE : Virtual Machine will deployed in a subnet this VM will have only private IP address, SO WE CAN'T SSH INTO THIS AS IT IS HAVING PRIVATE IP ADDRESS
- THAT'S WHY WE ARE USING AZURE BASTION, SO ANYONE WHO HAVE ACCESS TO THE BASTION CAN ACESS THE VM.
- ONE BASTION CAN USED WITH 'N' NO.OF VM's ALSO.
- AZURE PUBLIC IP
- ![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/4e8ba6cd-2735-49c6-8f69-8aad94f6e959)
- AZURE FIREALL
- We can create a azure policy to allow only my laptop ip address to access the VM or application deployed in VM.
- these things we can default in the policy while creating VNET.
- ![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/4d91adac-e284-452b-8ae7-febba64382ac)
- CIDR RANGE
- ![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/59212495-4f97-49d6-8d35-f85540611221)
- Azure automatically creates subnets for bastion, firewall, and traffic
- we have created a VNET with 4 subnets, -- firewall, firewall manager, bastion & this is for web application that we are going to deploy.
- Additionally we can create any number of subnets lets say for 3 tier application, DB subnets, Backend subnet like that.
- These are things that will get created for VNET
- ![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/7dd8fdd8-c410-4c62-aa2b-8b3a0ae05cb0)

3. CREATE A VM
- ![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/e7964e8a-44ad-4692-a0bd-c9d3971f2275)
- Delete NIC when VM is deleted  - SELECT CHECKBOX.
- IN THE ADVANCE TAB
- ![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/29814659-5513-4dc5-9213-6678a1dcd4e6)
- **CUSTOM DATA** - IS ONLY WHEN YOU CREATES THE VIRTUAL MACHINE IT GETS EXECUTED (AZURE CUSTOM DATA = AWS USER DATA)
- **USER DATA** - IT PERSISTS WITHIN THE VIRTUAL MACHINE THROUGHOUT ITS LIFETIME IT DOESN'T GET EXECUTED WHILE THE CREATION OF VIRTUAL MACHINE BUT IF YOU PASS
ANY FILE IN THE USER DATA THAT FILE WILL BE AVAILABLE WITHIN THE VIRTUAL MACHINE YOU CAN STOP THAT VIRTUAL MACHINE START IT BUT IT WILL BE AVAIALBLE ALL THE TIME
- WE WILL USE IT WHEN WE ARE USING VITUAL MACHINE SKILL SETS 

4. ACCESS THE VM
- ![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/67db638b-0bb4-4a03-98e6-cd72f504262f)
- IF we observe we don't have public IP address here.
- So how can we do SSH to VM. We can use BASTION HOST TO ACCESS OUR VM
- AZURE WILL CERATE A BASTION FOR US WHILE CREATING VNET WE NEED TO CHECK THE BOX TO CREATE BASTION
- SOME OF THE ADVANTAGES ARE
- ![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/646b00d5-8da2-4bde-adcc-70994812df91)
- 1. Bastion will be used as a proxy between dev's or devops engineers and the application that is installed on the private VM
  2. This bastion will help you to configure proper IAM policies (or) the proper identity policies where only certain people can access this BASTION and
   go thruogh the private virtual machine.
  3. So you can restrict the number of  poeple that can access this private machine and whoever is accessing you have the complete information like you can audit
  the people who are trying to access this private instance you can add security rules at the bastion level - all of these things are provided by the bastion
- WE CAN ACCESS THE VM BY BASTION
- ![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/88b017bd-1ff6-48bf-88f0-cda60b71716c)


5. GO TO BASTION
- ![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/c4dba1da-d909-416f-a4ad-5d84f04ac761)
- We have lot of controls that we can make here like deny, allow IAM and etc.

6. HOW TO ACCESS WEB-APP INSTALLED IN VM USING NGINX-WEBSERVER
- Only way to access it is through the particular FIREWALL
- We have to configure the firewall policy in such a way that if someone access the IP address of the FIREWALL in a particular port i will forward the request
to the VM.
- Go to firewalls - azure firewalls -- Click on firewall policy
- ![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/6ad10075-bde3-492e-b998-1fbfa68b06e2)
- Here we are using IN the settings -- DNAT rules -- NETWORK ADDRESS TRANSILATION RULES
- Priority -- least number is 100 and it goes to 6536, Whichever has the least number takes the maximum Priority
- 1st we need to create Rule collection
- 2nd we need to add Rule
- Source IP address is my ip address - My ip address in internet and give public ip address
- if we give (*) then everone can access our VM
- Destination ip address will be FIREWALL IP ADDRESS
- Go to firewall manager and firewalls
- ![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/675bc28c-74e1-4045-864f-6d4d86496548)
- Click on Firewall policy - Pava firewall policy
- We need Public FIREWALL IP ADDRESS NOT THE PRIVATE IP ADDRESS
- ![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/01c0d152-2775-40aa-91ef-9e3181588a79)
- Why we need Public IP address be'coz our firewall is in VNET, we can't access private IP of it, So we are using PUBLIC IP ADDRESS OF FIREWALL.
- Destination Ports : You can access the firewall on any port this is up to you can configure any anything.
- BUT THE TARGET PORT THAT IS THE PORT WHERE MY NGINX APPLICATION IS INSTALLED THAT HAS TO BE PORT 80 BE'COZ NGINX RUNS ON PORT 80
- We can confirm that our nginx is working fine by
- ![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/f1128c0e-a0d3-4303-a542-22353033a7aa)
- Here we can this deployed INDEX.HTML page if this is not seen there is some issue with nginx like not deployed or not running.
- Here **Translated Address** - TO IP ADDRESS OF VM
- ![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/4eb06c14-fa1f-4440-9f67-fdc0bd5211d6)
- **Translated Port** : 80 ( WHICH IS NGINX PORT NO )
- ![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/ebaae570-f62c-4552-8925-5b0b96663280)
- ALL PROPERTIES
- ![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/3ba1dbf8-70a9-4b4f-a621-742c340a9977)
- SOURCE TTYPE : WHOEVER ACCESSING THE NGIX APPLICATION IF IT IS ONE USER, IP ADRESS IF MANY THEN MANY IP ADDRESS, WE CAN PROVIDE SUBNET RANGE OR INDIVIDUAL IP ADDRESS
- DESTINATION IP ADDRESS : THIS DESTINATION IS NOT THE NGINX DESTINATION - THIS IS FIREWARE IP ADDRESS AND FIREWARE PORT - WE HAVE TO USE FIREWARE PUBLIC IP ADRESS AND ANY PORT OF OUR CHOICE
THERE CAN BE MANY OTHER APPLICATIONS AS WELL SO I AM USING 4000 FOR THIS NGINX APPLICATION IF CAN USE ANY IP OF OUR CHOICE.
- TRASNLATED TYPE : IS IP ADDRESS - BE'COZ WE ARE UING NAT NETWORK ADDRESS TRANSLATION, SO THE REQUEST THAT WE ARE SENDING TO THE FIREWALL WEILL BE TRANSLATED
THE PRIVATE IP ADDRESS OF SUBNET WHERE NGINX APPLICATION AND VM ARE INSTALLED.
- TRANSLATED PORT : NGINX PORT 80


![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/da5578f5-ed25-4291-9d67-a276851d0c08)

- web app
- ![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/678f5b71-aea6-4d9e-884e-a7c26a629b63)

- HERE ACCCESSING THE WEB APP FROM OUR IP - THAT IS MY LATTOP
- BASTION IS DIFFERENT - WHICH IS USED TO SSH THE VM

### TO ACCESS THE WEB APP DEPLOYED IN NGINX 
- FIREWALL IP ADDRESS
- FIREWARE PORT THAT WE HAVE ADDED IN DNAT RULES : 4000
- ![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/7ca388ab-9f90-48a9-a2d4-066c0d5251fc)
- ![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/acaf215d-9058-48f5-b0f6-6d1f2e66c2ba)


### NOTE : IN AZURE, BY DEFAULT EACH INSTANCE WILL BE ATTACHED WITH "ALLOWVNETINBOUND" Inboud rule in NSG. THAT ALLOWS DEFAULT ACCESS TO FIREWALL AND OTHER RESOURECES WITH IN THE VNET YOU CAN BLOCK THIS BY  CREATING A NEW INBOUND RULE WITH MORE PRIORITY.
### NOTE : EXACTLY WHAT I DID IN THE 310 RULE - BLOACKED ACCESS TO PORT 80 JUST TO BLOCK FIREWALL AS WELL.



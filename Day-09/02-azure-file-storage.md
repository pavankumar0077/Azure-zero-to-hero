# Azure File Storage

### EFS IN AWS
- VM
- PODS
- MULTIPLE PODS USE THE SINGLE VOLUME ( EFS IN AWS )
- ONE POD ONE VOLUME THEN IT IS CALLED MANAGED DISK. ( EBS IN AWS )

- FILES SHARE --
- ![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/3ba35110-a614-4d21-8d01-63fd52466ea9)
- BACKUP IS IMP WHERE MULTIPLE VM's CAN CONNECT TO THIS, USED FOR FILE SHARING.
- IT IS IMP TO HAVE A BACKUP, WE CAN CREATE A BACKUP POLICY
- REVIEW AND CREATE.
- PROTOCOL USED BY FILE SYSTEM IN AZURE IS SBM
- AS WE KNOW THIS CAN CONNECT TO MULTIPLE VM'S AND EVEN FOR KUBERNETES PODS.
- ![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/0ec7e1d5-2c96-49e2-97f2-e7452efcc9f7)
- ![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/0bfeda9e-ecf0-4abc-91b8-e28a3cd7c923)
- Depending on the OS we can get the file system script to use
- ![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/093fcb16-1d4d-45ae-addd-05a501ac8bb2)
- EXECUTE THIS TO THE LINUX OR MACOS OR WINDOWS(POWER SHELL SCRIPT) THEN WE WILL CONNECTED TO THE FILE SHARE RESOURCE.




1. What is it?

    Azure File Storage is a fully managed file share service in the cloud.
    It provides the Server Message Block (SMB) protocol for sharing files across applications and VMs in the Azure cloud.
    Azure File Storage is useful for applications that require shared file access, such as configuration files or data files.

2. When to use it?

    Use Azure File Storage when you need a shared file system that can be accessed from multiple VMs or applications.
    It is suitable for scenarios like storing configuration files, sharing data between applications, and serving as a common storage location for applications in a cloud environment.

3. Example from DevOps Engineer point of view?

    A DevOps engineer may leverage Azure File Storage to store configuration files that are shared among multiple application instances.
    In a deployment pipeline, scripts or configuration files stored in Azure File Storage can be mounted to VMs or containers during the deployment process.

4. Equivalent service in AWS:

    The equivalent service in AWS is Amazon Elastic File System (EFS). EFS provides scalable file storage for use with Amazon EC2 instances, supporting the Network File System (NFS) protocol.

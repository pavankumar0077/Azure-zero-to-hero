# Azure Queue Storage

- 1000 requests at a time will be maintained in a queue.
- SQS ON AWS

- ![image](https://github.com/pavankumar0077/Azure-zero-to-hero/assets/40380941/8711c1e9-cb9f-4633-b2bb-157dbadbcde1)
- Mostly probably the messages in the queue are updated using AUTOMATION SCRIPT
- APPLICAITON PROCESS THE EACH QUEUE IN THE ORDER
- 

1. What is it?

    Azure Queue Storage is a message queue service that allows the decoupling of components in a distributed application.
    It provides a reliable way to store and retrieve messages between application components, ensuring asynchronous communication.

2. When to use it?

    Use Azure Queue Storage when you need to enable communication and coordination between different parts of a distributed application.
    It is suitable for scenarios like handling background jobs, managing tasks asynchronously, and facilitating communication between loosely coupled components.

3. Example from DevOps Engineer point of view?

    A DevOps engineer may use Azure Queue Storage to implement a message queue for processing background tasks or managing communication between microservices.
    During deployment, scripts can enqueue messages to trigger specific actions or coordinate tasks between different components.

4. Equivalent service in AWS:

    The equivalent service in AWS is Amazon Simple Queue Service (SQS). SQS provides a fully managed message queue service for decoupling components in a distributed system.

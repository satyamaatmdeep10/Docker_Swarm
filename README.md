# Docker_Swarm
Deploying web app using Docker Swarm

### what is Docker Swarm?
Docker Swarm is a container orchestration tool that simplifies managing multiple Docker hosts by grouping them into a single cluster. It enables seamless scalability, allowing you to easily scale services up or down by adjusting container replicas. With built-in high availability, Docker Swarm ensures applications remain operational by redistributing containers in case of node failures. It includes automatic load balancing, distributing traffic across containers to optimize performance, and service discovery, which lets containers communicate using service names. Docker Swarm provides secure communication between nodes using TLS encryption and supports rolling updates for zero-downtime deployments. Its simple networking feature creates an overlay network, enabling containers across nodes to communicate securely. Overall, Docker Swarm is ideal for managing, scaling, and securing distributed containerized applications with ease.

# Overview
The project demonstrates how to deploy a scalable and fault-tolerant web application using Docker Swarm on AWS. It sets up a cluster of three nodes (one manager and two workers) where the application is deployed as a Docker service. The service is replicated across all nodes to ensure high availability and load balancing. The application is exposed to users via a specified port, making it accessible through any node's IP address. The project also provides flexibility to scale up or down by adding/removing worker nodes, ensuring a robust and production-ready deployment environment.

## Solution
### Step1:- 
We have created three new instances on the AWS portal:
Swarm-manager,
Swarm-worker1,
Swarm-worker2


### Step2:-
In all three instances, we have configured the inbound rules to allow:
Custom TCP -- 2377 -- Anywhere IPv4, 
Custom TCP -- 8001 -- Anywhere IPv4

### Step3 :-
We have installed Docker with Docker Engine on all three nodes.

A) Update Your System -  sudo apt update
   sudo apt upgrade -y
   
B) Install Required Dependencies - _sudo apt install apt-transport-https ca-certificates curl software-properties-common -y_

c) Add Docker’s Official GPG Key - _curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg_

D) Add Docker APT Repository - _echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee 
    /etc/apt/sources.list.d/docker.list > /dev/null_
    
E)  Install Docker Engine -_ sudo apt update
    sudo apt install docker-ce docker-ce-cli containerd.io -y_
    
F) Verify Docker Installation - _sudo docker --version
    sudo systemctl status docker_
    

### Step4 :-
 Next, we open the Swarm Manager node and run the command: _sudo docker swarm init_ .This command initializes an empty Docker Swarm on the manager node.

### Step5 :-
 After initializing the swarm on the swarm-manager node, a key will be generated to add other nodes as workers. We copy this key and run it on the other nodes (swarm-worker1 and swarm-worker2) to add them to the swarm as worker nodes. Now, both worker nodes are successfully added to the swarm.

### Step6 :- 
To check all the nodes in the manager, run the following command on the swarm-manager node: _docker swarm node ls_ .This will list all the nodes in the swarm, showing their status and roles (manager or worker).

### Step7 :-
Now, on the Manager Node, we create a service by running the following command: _sudo docker service create --name django-app-service --replicas 3 --publish 8001:8001 trainwithshubham/react-django-app:latest
_ . This command creates a service named django-app-service, with 3 replicas, and exposes it on port 8001. The service uses the trainwithshubham/react-django-app:latest image.

### Step8 :-
Now, run the following command on the Manager Node to list all the services: _sudo docker service ls_ .This will display the active services in the swarm, including the django-app-service we just created.

### Step9 :-
Now, the service will create a container on the Manager Node. To check the running containers, run the following command: _sudo docker ps_ .This will list all the active containers, including the ones created by the django-app-service.

### Step10 :- 
Now, the service will be running on all three nodes. To check this, grab the IP address of any of the nodes and access the service by appending port 8001. For example: As <Any_ip_of_3_vms>:8001 This will allow you to verify that the application is accessible on all three nodes through the specified port.

### Step11 :-
If you want to remove a node from the swarm environment, you can run the following command on the specific worker node you wish to remove: _sudo docker swarm leave_ . This will remove the worker node from the swarm. If the node is the manager, you need to first demote it to a worker before leaving the swarm.

### Step12 :- 
After removing one of the worker nodes using the sudo docker swarm leave command, the status of that node will be reflected as "Down" in the swarm-manager when you run the docker swarm node ls command. This indicates that the node is no longer part of the swarm and is not contributing to the service's operation.




 

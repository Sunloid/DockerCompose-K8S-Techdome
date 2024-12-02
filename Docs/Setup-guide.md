# Setup for the Project 
This file contains all the information needed for setting up the project. 

## AWS Instance 
The main problem I ran into while developing this project was the disk space. Docker files  which include images, containers, volumes, build data and cache require a large amount of space. There are two ways of dealing with this one using a instance type which features a huge amount of space (t2.large) or using a small instance (t2.micro) and installing a EBS volume on the same. 

The easier method would be to use a instance which huge amount of memory like t2.large . For security enable all traffic and use ubuntu linux. 
 ![Alt text](<image (26).png>)


## Downloads in the Instance
Downloads like docker and git are simple and straight forward but for kubernetes there are complications which I will explain in this section. 
```
sudo su 
apt install && upgrade -y 
apt install git docker.io docker-compose npm nodejs -y 
```
npm is being installed to install dependencies in frontend and backend. 
git is being installed to pull frontend and backend projects from the repositories. 

Terraform installation and cluster formation: 
```

```


## Infrastructure Setup 
```
Docker-k8s/
│
├── backend/
│   ├── Dockerfile
│   ├── controller
│   ├── database
│   ├── models
│   ├── routes
│   ├── server.js
│   ├── package.json
│   └── .env (backend environment variables)
│
├── frontend/
│   ├── Dockerfile
│   ├── src/
│   ├── public/
│   ├── package.json
│
├── k8s/
│   ├── frontend-deployment.yaml
│   ├── backend-deployment.yaml
│   └── mongo-deployment.yaml
│
└── docker-compose.yaml (optional, for local development)
```

The project infrastructure would exactly be like this one. Make 3 directories in the project directory and name them frontend, backend and k8s. In frontend 
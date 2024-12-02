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
apt install -y apt-transport-https ca-certificates curl gpg
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.31/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.31/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list
apt update -y 
apt install -y kubelet kubeadm kubectl
kubeadm init 
```
Make sure to run these commands one by one. 
You can create more instances and add more nodes into the cluster but that is optional. 


## Infrastructure of the project
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


## Frontend Directory and Image
The project infrastructure would exactly be like this one. Make 3 directories in the project directory and name them frontend, backend and k8s.

In the  frontend directory run this command 
```
git clone https://github.com/Sunloid/Techdome-frontend-fork
```

This will clone the frontend repository which I posted. Remove things like the package-lock and readme file. Add a dockerfile and copy paste this code. 
```
# Stage 1: Build the React app
FROM node:16 AS build
WORKDIR /app

# Copy only package.json and install dependencies
COPY package.json ./
RUN npm install

# Copy the rest of the project files and build the app
COPY . .
RUN npm run build

# Stage 2: Serve the app using Nginx
FROM nginx:alpine
WORKDIR /usr/share/nginx/html

# Remove the default Nginx static assets
RUN rm -rf ./*

# Copy built React app from the build stage
COPY --from=build /app/build ./

# Expose the port the app runs on
EXPOSE 80

# Start Nginx
CMD ["nginx", "-g", "daemon off;"]
```
Then the frontend directory should start looking something like the frontend folder from this repo 

Next build the frontend image 
```
docker build -t frontend . 
```

<>





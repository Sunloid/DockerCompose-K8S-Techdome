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

Edit the api.js file in the apiConfig of the src folder and use the public ip of the instance. 

![Alt text](<image (28).png>)

Next build the frontend image 
```
docker build -t frontend . 
```
![Alt text](<image (29).png>)


## Backend Image and Directory 
Make a backend directory in the same place you made frontend and use this 
```
git clone https://github.com/Sunloid/Techdome-backend-fork
```

Remove useless files like the readme and package-lock.json and create a dockerfile and copy paste this. 
```
# Use official Node.js image
FROM node:16 AS build
WORKDIR /app

# Copy package.json and package-lock.json (if available)
COPY package.json ./

# Install dependencies inside the container
RUN npm install

# Copy the rest of the backend files (controllers, routes, server.js, etc.)
COPY . .

# Expose the port your backend is running on (default is 5000)
EXPOSE 5000

# Start the backend server
CMD ["npm", "start"]
```

GO the .env file and fill in the necessary information present there. 

Then build the backend image 
```
docker build -t backend . 
```
![Alt text](<image (30).png>)

## Docker-compose file 
GO to the directory where frontend and backend are present and create a docker-compose.yml file and copy paste this code. 
```
version: '3.8'

services:
  frontend:
    image: frontend
    container_name: Frontend
    ports:
      - 3000:80
    depends_on:
      - backend

  backend:
    image: backend
    container_name: Backend
    environment:
      - DB=mongodb://database:27017/mydatabase4
    ports:
      - 5000:5000
    depends_on:
      - database

  database:
    image: mongo
    container_name: Database
    ports:
      - 27017:27017
```

Change the value of DB with the name of the database you see fit. 

To check if the images and code are running porperly or not we will deploy them on docker-compose and for that use this code. 
```
docker-compose up -d
```

![Alt text](<image (31).png>)

Use the pubic IP address and the ports 3000 and 5000 to check if the frontend and backend are working or not. If they are then you would have a website like this 

![Alt text](<image (32).png>)

![Alt text](<image (33).png>)

![Alt text](<image (34).png>)

Check if the api calls are working or not in the network tab in the user-data section.

After that is done remove docker compose 
```
docker-compose down 
```

## Kubernetes Setup 
Go to the same directory where the frontend, backend and docker-compose are and create another directory called k8s. 

Make 3 more files with the names: 
- backend-Deployment.yml 
- frontend-Deployment.yml 
- database-Deployment.yml 

and copy paste the following codes in all of them 

frontend-Deployment.yml: 
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: frontend-deployment
spec:
  replicas: 1
  template:
    metadata:
      labels:
        app: frontend
    spec:
      containers:
        - name: frontendcontainer
          image: frontend
          ports:
            - containerPort: 3000
```

backend-Deployment.yml: 
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: backend-deployment
spec:
  replicas: 1
  template:
    metadata:
      labels:
        app: backend
    spec:
      containers:
        - name: backendcontainer
          image: backend
          ports:
            - containerPort: 5000
          env:
            - name: DB
              value: "mongodb://database:27017/mydatabase"
```

database-Deployment.yml:
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mongo-deployment
spec:
  replicas: 1
  template:
    metadata:
      labels:
        app: mongo
    spec:
      containers:
        - name: mongo
          image: mongo
          ports:
            - containerPort: 27017
```
<>

Now create pods of the same: 
```
kubectl apply -f frontend-Deployment.yml
kubectl apply -f backend-Deployment.yml
kubectl apply -f database-Deployment.yml
```

Create 3 more files with the names: 
- frontend-service.yml 
- backend-service.yml 
- database-service.yml 

Now copy paste the following codes in those files

frontend-service: 
```
apiVersion: v1
kind: Service
metadata:
  name: frontend-service
spec:
  type: NodePort
  selector:
    app: frontend
  ports:
    - protocol: TCP
      port: 80
      targetPort: 3000
      nodePort: 3000  # Access frontend at this port on the public IP
```

backend-service:
```
apiVersion: v1
kind: Service
metadata:
  name: backend-service
spec:
  type: NodePort
  selector:
    app: backend
  ports:
    - protocol: TCP
      port: 5000
      targetPort: 5000
      nodePort: 5000  # Access backend at this port on the public IP
```

database-service.yml: 
```
apiVersion: v1
kind: Service
metadata:
  name: database-service
spec:
  type: ClusterIP
  selector:
    app: database
  ports:
    - protocol: TCP
      port: 27017
      targetPort: 27017
```

Finally apply the following and deploy the pods on the local kubernetes cluster: 
```
kubectl apply -f frontend-service.yml
kubectl apply -f backend-service.yml
kubectl apply -f database-service.yml
```

The frontend and backend will now be accessible 

Frontend: http://<PUBLIC_IP>:3000

Backend: http://<PUBLIC_IP>:5000

<>

<>

<>

<>

<>
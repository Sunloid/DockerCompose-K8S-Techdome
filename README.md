# Docker-Compose and Kubernetes Multi-Container application
This project is a full stack web application consisting of a frontend, backend and database. The application is containerized using Docker and orchestrated with Kubernetes for local deployment. 

- **Frontend**: Built with React.js it communicates with the backend to provide a dynamic user interface. 
- **Backend**: Developed using Node.js, it handles the API requests and connects MongoDB for data storage.
- **DataBase**: MongoDB is used as the database to store application data. 

The project leverages Docker Compose for managing multiple services locally and Kubernetes for scalable deployment. This setup allows for the easy development, testing and deployment of the application in a containerized environment, ensuring that all components work seamlessly together. 

## Project Structure
```
my-project/
│
├── frontend/                         # Frontend code (React, Vue, etc.)
│   ├── Dockerfile                    # Dockerfile to build the frontend container
│   ├── .env                          # Environment variables specific to frontend
│   ├── public/                       # Static files like images, icons
│   ├── src/                          # React/Vue source files
│   ├── package.json                  # Frontend dependencies and scripts
│   └── README.md                     # Information about the frontend
│
├── backend/                          # Backend code (Node.js, Express, etc.)
│   ├── Dockerfile                    # Dockerfile to build the backend container
│   ├── .env                          # Environment variables specific to backend
│   ├── src/                          # Backend source code
│   ├── package.json                  # Backend dependencies and scripts
│   └── README.md                     # Information about the backend
│
├── database/                         # Database (MongoDB) related configurations
│   └── Dockerfile                    # Dockerfile to run MongoDB if you want custom setup
│
├── kubernetes/                       # Kubernetes deployment and service files
│   ├── frontend-deployment.yaml      # Deployment file for the frontend
│   ├── frontend-service.yaml         # Service file for the frontend
│   ├── backend-deployment.yaml       # Deployment file for the backend
│   ├── backend-service.yaml          # Service file for the backend
│   ├── mongo-deployment.yaml         # Deployment file for the MongoDB
│   └── mongo-service.yaml            # Service file for the MongoDB
│
├── docker-compose.yml                # Docker Compose configuration for local setup
├── .gitignore                        # Files and directories to ignore in Git
└── README.md                         # Project overview and setup instructions
```
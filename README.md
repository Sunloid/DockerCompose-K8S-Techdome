# Docker-Compose and Kubernetes Multi-Container application
This project is a full stack web application consisting of a frontend, backend and database. The application is containerized using Docker and orchestrated with Kubernetes for local deployment. 

- **Frontend**: Built with React.js it communicates with the backend to provide a dynamic user interface. 
- **Backend**: Developed using Node.js, it handles the API requests and connects MongoDB for data storage.
- **DataBase**: MongoDB is used as the database to store application data. 

The project leverages Docker Compose for managing multiple services locally and Kubernetes for scalable deployment. This setup allows for the easy development, testing and deployment of the application in a containerized environment, ensuring that all components work seamlessly together. 

## Project Structure
```
DockerCompose-K8S-Techdome/
│
├── frontend/                         # Frontend code (React, Vue, etc.)
│   ├── Dockerfile                    # Dockerfile to build the frontend container
│   ├── public/                       # Static files like images, icons
│   ├── src/                          # React source files
│   └── package.json                  # Frontend dependencies and scripts
│
├── backend/                          # Backend code (Node.js, Express, etc.)
│   ├── Dockerfile                    # Dockerfile to build the backend container
│   ├── Controller                    # Used to control and link the actions of blog and user 
│   ├── Database                      # Setup of the mongoDB database
│   ├── Models                        # Models of blog and user for the database
│   ├── Routes                        # Used for routing specific things to database
│   ├── server.js                     # Defining the api and cloudinary
│   ├── .env                          # Environment variables specific to backend
│   └── package.json                  # Backend dependencies and scripts
│
├── kubernetes/                       # Kubernetes deployment and service files
│   ├── frontend-deployment.yaml      # Deployment file for the frontend
│   ├── frontend-service.yaml         # Service file for the frontend
│   ├── backend-deployment.yaml       # Deployment file for the backend
│   ├── backend-service.yaml          # Service file for the backend
│   ├── mongo-deployment.yaml         # Deployment file for the MongoDB
│   └── mongo-service.yaml            # Service file for the MongoDB
│
├── Docs/                             # Project specific documents  
│   ├── Error-log.md                  # Contains all the Errors and Challenges I faced in this project
│   ├── Deployment-strategy.md        # My Deployment Strategy
│   └── Setup-guide.md                # Complete step by step for the setup of this project
│
├── docker-compose.yml                # Docker Compose configuration for local setup
└── README.md                         # Project overview and setup instructions
```
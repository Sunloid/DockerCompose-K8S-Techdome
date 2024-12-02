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
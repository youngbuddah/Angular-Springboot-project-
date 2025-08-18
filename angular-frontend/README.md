# Angular Project Setup & Dockerization Guide

This guide provides step-by-step instructions for setting up an Angular project from scratch on Ubuntu and Dockerizing it for deployment.

---

## ⚡ Part 1: Angular Project Setup on Ubuntu

### Step 1: Install Node.js and npm

```bash
sudo apt update
sudo apt install nodejs npm
```

### Step 2: Check Node.js and npm versions

```bash
node -v
npm -v
```

### Step 3: Install Angular CLI globally

```bash
sudo npm install -g @angular/cli@14.2.1
```

### Step 4: Verify Angular CLI installation

```bash
ng --version
```

### Step 5: Angular Setup for Environments

Angular provides an environment system. Inside `src/environments/` you’ll find:

- `environment.ts` (for development)
- `environment.prod.ts` (for production)

Update them like this:

**environment.ts (local dev):**
```typescript
export const environment = {
  production: false,
  apiUrl: 'http://localhost:8080/api'
};
```

**environment.prod.ts (EC2 prod):**
```typescript
export const environment = {
  production: true,
  apiUrl: 'http://<BACKEND-EC2-PUBLIC-IP>:8080/api'
};
```

### Step 6: Edit Service Class in Angular

```bash
cd src/app/services/
vim worker.service.ts
```
Edit this line in your service:
```typescript
private getUrl: string = "http://<insert-backend-public-ip>:8080/api/v1/workers";
```

### Step 7: Install Project Dependencies and Build

**Option 1: Serve via Angular CLI**

```bash
npm install
ng build
cd dist/angular-frontend
ng serve --host 0.0.0.0 --port=80
```

**Option 2: Serve via Nginx**

1. Install Nginx:
```bash
sudo apt update
sudo apt install nginx -y
```
2. Copy Angular build to Nginx web root:
```bash
sudo rm -rf /var/www/html/*
sudo cp -r dist/angular-frontend/* /var/www/html/
```
3. Restart Nginx:
```bash
sudo systemctl restart nginx
```
4. Access your app via:
```
http://<frontend-ec2-public-ip>/
```

### Step 8: Deploy the Angular Application

Transfer the contents of the `dist/` directory to your server or hosting provider to make your Angular application available to users.

---

## ⚡ Part 2: Dockerizing the Angular Project

Docker allows you to package your Angular application into a container, making it portable and easily deployable.

### Prerequisites

- Docker installed on your system.  
  [Install Docker on Ubuntu](https://docs.docker.com/engine/install/).

### Step 1: Create a Dockerfile

In the root directory of your Angular project, create a file named `Dockerfile`:

```Dockerfile
# Use official Node.js image as the base image
FROM node:14-alpine as build

# Set the working directory in the container
WORKDIR /usr/src/app

# Copy package.json and package-lock.json (if available)
COPY package*.json ./

# Install project dependencies
RUN npm install

# Copy the rest of the application code
COPY . .

# Build the Angular application
RUN npm run build

# Use NGINX as the production server
FROM nginx:alpine

# Copy the built artifact from the previous stage to NGINX web server directory
COPY --from=build /usr/src/app/dist/angular-frontend /usr/share/nginx/html

# Expose port 80 to the outside world
EXPOSE 80

# Start NGINX server when the container starts
CMD ["nginx", "-g", "daemon off;"]
```

### Step 2: Build the Docker Image

```bash
docker build -t angular-app:latest .
```

### Step 3: Run the Docker Container

```bash
docker run -d -p 80:80 angular-app:latest
```

Open your browser and navigate to `http://localhost` to see your Angular application running inside Docker.

---

# Expose port 80 to the outside world
EXPOSE 80

# Start NGINX server when the container starts
CMD ["nginx", "-g", "daemon off;"]

# Screenshot
![Screenshot](Screenshot%20from%202025-08-17%2019-48-02.png)


✍️ **Author:** Abhay Bendekar  
🔗 [LinkedIn](https://www.linkedin.com/in/abhay-bendekar-75474b372/)

Project: Automate Code Deployment Using CI/CD Pipeline
📌 Objective:
Set up a CI/CD pipeline using GitHub Actions that:

Builds a Node.js app

Creates a Docker image

Pushes the image to DockerHub automatically when code is pushed to GitHub

🧰 Tools & Technologies:
Node.js (Simple web app)

Docker

GitHub

GitHub Actions

DockerHub

✅ Final Project Structure:
pgsql
Copy
Edit
node-docker-ci/
├── Dockerfile
├── app.js
├── package.json
├── package-lock.json
├── .github/
│   └── workflows/
│       └── main.yml
└── README.md


🛠️ Step 1: Create a Simple Node.js Web App
Create a folder and inside it:

app.js

{
  "name": "myapp",
  "version": "1.0.0",
  "description": "A simple Node.js app for Jenkins CI/CD",
  "main": "app.js",
  "scripts": {
    "start": "node app.js",
    "test": "echo \"No test yet\" && exit 0"
  },
  "dependencies": {
    "express": "^4.18.2"
  }
}

package.json
{
  "name": "myapp",
  "version": "1.0.0",
  "description": "A simple Node.js app for Jenkins CI/CD",
  "main": "app.js",
  "scripts": {
    "start": "node app.js",
    "test": "echo \"No test yet\" && exit 0"
  },
  "dependencies": {
    "express": "^4.18.2"
  }
}

npm install
🐳 Step 2: Create Dockerfile
# Use official Node.js image
FROM node:18

# Set working directory
WORKDIR /app

# Copy files
COPY package*.json ./
RUN npm install

COPY . .

# Expose the app port
EXPOSE 3000

# Start the app
CMD ["npm", "start"]

🔄 Step 3: Create GitHub Actions Workflow
.github/workflows/main.yml
yaml
Copy
Edit
name: CI/CD Pipeline

on:
  push:
    branches:
      - main  # or "master" if your branch is named that


Step 4: Create main.yml
name: CI/CD Pipeline with Docker

on:
  push:
    branches: [ master]

jobs:
  build-and-run:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v3

    - name: Set up Docker
      uses: docker/setup-buildx-action@v2

    - name: Build Docker image
      run: docker build -t myapp .

    - name: Run Docker container
      run: docker run -d -p 3000:3000 myapp


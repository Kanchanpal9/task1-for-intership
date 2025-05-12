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
js
Copy
Edit
const http = require('http');
const port = 3000;

const requestHandler = (req, res) => {
  res.end('Hello from Node.js App deployed with CI/CD 🚀');
};

const server = http.createServer(requestHandler);

server.listen(port, () => {
  console.log(`Server running at http://localhost:${port}`);
});
package.json
json
Copy
Edit
{
  "name": "node-app",
  "version": "1.0.0",
  "description": "Simple Node.js app for CI/CD demo",
  "main": "app.js",
  "scripts": {
    "start": "node app.js",
    "test": "echo \"No tests yet\" && exit 0"
  },
  "author": "kanchanpal",
  "license": "ISC"
}
Run these in terminal to install dependencies:

bash
Copy
Edit
npm install
🐳 Step 2: Create Dockerfile
Dockerfile
Dockerfile
Copy
Edit
# Base image
FROM node:18

# Set working directory
WORKDIR /app

# Copy files
COPY . .

# Install dependencies
RUN npm install

# Expose port
EXPOSE 3000

# Start app
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

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v3

    - name: Set up Node.js
      uses: actions/setup-node@v3
      with:
        node-version: '18'

    - name: Install dependencies
      run: npm install

    - name: Run tests
      run: npm test

    - name: Log in to DockerHub
      uses: docker/login-action@v3
      with:
        username: ${{ secrets.DOCKERHUB_USERNAME }}
        password: ${{ secrets.DOCKERHUB_TOKEN }}

    - name: Build Docker image
      run: docker build -t ${{ secrets.DOCKERHUB_USERNAME }}/node-app:latest .

    - name: Push Docker image
      run: docker push ${{ secrets.DOCKERHUB_USERNAME }}/node-app:


      🔐 Step 4: Set GitHub Secrets
Go to your GitHub repo → Settings → Secrets and variables → Actions → New repository secret:

DOCKERHUB_USERNAME = your DockerHub username

DOCKERHUB_TOKEN = DockerHub Access Token from here


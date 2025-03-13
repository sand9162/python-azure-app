Python Azure App Documentation


📌 Project Overview

This project is a simple Python web application deployed to Microsoft Azure using Docker and GitHub Actions for CI/CD. The app is built using Flask (a lightweight Python framework) and follows a containerized approach for easy deployment.

🚀 Setup Instructions
1. Prerequisites
Python (version 3.8+)
Docker
GitHub account
Azure account with a subscription
Azure CLI installed (az)
2. Clone the Repository
bash
Copy
Edit
git clone https://github.com/sand9162/python-azure-app.git
cd python-azure-app
3. Create and Activate a Virtual Environment
bash
Copy
Edit
python -m venv venv
source venv/bin/activate   # For Linux/macOS
# OR
.\venv\Scripts\activate   # For Windows
4. Install Dependencies
bash
Copy
Edit
pip install -r requirements.txt
5. Run the App Locally
bash
Copy
Edit
python app.py
The app should be accessible at:
cpp
Copy
Edit
http://127.0.0.1:5000
🏗️ File/Folder Structure
css
Copy
Edit
python-azure-app-main/
├── .gitignore
├── Dockerfile
├── app.py
├── requirements.txt
├── .github/
│   └── workflows/
│       └── deploy.yml
├── templates/
│   └── index.html
└── tests/
    └── test_app.py
1. .gitignore
Lists files and directories to ignore when pushing to GitHub.
2. Dockerfile
Defines how to build the Docker container:

Dockerfile
Copy
Edit
# Use an official Python runtime as the base image
FROM python:3.8-slim

# Set the working directory
WORKDIR /app

# Copy the files to the container
COPY . .

# Install dependencies
RUN pip install -r requirements.txt

# Expose the application port
EXPOSE 5000

# Command to run the application
CMD ["python", "app.py"]
3. app.py
Main Flask application:

python
Copy
Edit
from flask import Flask, render_template

app = Flask(__name__)

@app.route('/')
def home():
    return render_template('index.html')

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
Renders index.html template when the root URL is accessed.
4. requirements.txt
Lists dependencies:

ini
Copy
Edit
Flask==2.0.3
5. .github/workflows/deploy.yml
GitHub Actions workflow for CI/CD:

yaml
Copy
Edit
name: Deploy to Azure

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Login to Azure
        run: az login --service-principal -u ${{ secrets.AZURE_CLIENT_ID }} -p ${{ secrets.AZURE_CLIENT_SECRET }} --tenant ${{ secrets.AZURE_TENANT_ID }}

      - name: Build Docker image
        run: |
          docker build -t my-python-app:${{ github.sha }} .
          docker tag my-python-app:${{ github.sha }} myregistry.azurecr.io/my-python-app:${{ github.sha }}

      - name: Push Docker image to Azure Container Registry
        run: |
          docker push myregistry.azurecr.io/my-python-app:${{ github.sha }}

      - name: Deploy to Azure App Service
        run: |
          az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name my-python-app --deployment-container-image-name myregistry.azurecr.io/my-python-app:${{ github.sha }}
💡 Secrets Required:

AZURE_CLIENT_ID – Azure Service Principal ID
AZURE_CLIENT_SECRET – Azure Service Principal Secret
AZURE_TENANT_ID – Azure Tenant ID
6. templates/index.html
HTML template for the homepage:

html
Copy
Edit
<!DOCTYPE html>
<html>
<head>
    <title>Python Azure App</title>
</head>
<body>
    <h1>Welcome to the Python Azure App!</h1>
</body>
</html>
7. tests/test_app.py
Unit test for the Flask app:

python
Copy
Edit
import pytest
from app import app

@pytest.fixture
def client():
    with app.test_client() as client:
        yield client

def test_home(client):
    response = client.get('/')
    assert response.status_code == 200
    assert b"Welcome to the Python Azure App!" in response.data
🐳 Build and Run with Docker
1. Build the Docker Image
bash
Copy
Edit
docker build -t python-azure-app .
2. Run the Docker Container
bash
Copy
Edit
docker run -d -p 5000:5000 python-azure-app
Open browser and visit:
cpp
Copy
Edit
http://127.0.0.1:5000
☁️ Deploy to Azure
1. Create Azure Resource Group
bash
Copy
Edit
az group create --name myResourceGroup --location eastus
2. Create Azure Container Registry
bash
Copy
Edit
az acr create --resource-group myResourceGroup --name myRegistry --sku Basic
3. Login to Azure Container Registry
bash
Copy
Edit
az acr login --name myRegistry
4. Tag and Push Docker Image
bash
Copy
Edit
docker tag python-azure-app myregistry.azurecr.io/python-azure-app:v1
docker push myregistry.azurecr.io/python-azure-app:v1
5. Create Azure App Service Plan
bash
Copy
Edit
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku B1 --is-linux
6. Deploy Container to Azure
bash
Copy
Edit
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name python-azure-app --deployment-container-image-name myregistry.azurecr.io/python-azure-app:v1
🧪 Run Tests
bash
Copy
Edit
pytest tests/
🔥 CI/CD with GitHub Actions
Push code to main branch → GitHub Actions builds and deploys the app to Azure automatically.
⚠️ Troubleshooting
Issue	Solution
App not loading	Check Docker logs (docker logs <container_id>)
Build failing in GitHub Actions	Ensure Azure login details are set in GitHub secrets
Port conflict	Ensure port 5000 is not being used by another process

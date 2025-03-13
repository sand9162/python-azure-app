
# Python Azure App Documentation  

## 📌 Project Overview
This project is a simple Python web application deployed to **Microsoft Azure** using **Docker** and **GitHub Actions** for CI/CD. The app is built using **Flask** (a lightweight Python framework) and follows a containerized approach for easy deployment.

---

## 🚀 Setup Instructions
### 1. Prerequisites
- Python (version 3.8+)
- Docker
- GitHub account
- Azure account with a subscription
- Azure CLI installed (`az`)

### 2. Clone the Repository
```bash
git clone https://github.com/sand9162/python-azure-app.git
cd python-azure-app
```

### 3. Create and Activate a Virtual Environment
```bash
python -m venv venv
source venv/bin/activate   # For Linux/macOS
# OR
.env\Scriptsctivate   # For Windows
```

---

## 🏗️ File/Folder Structure
```
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
```

---

## 🐳 Build and Run with Docker
### 1. Build the Docker Image
```bash
docker build -t python-azure-app .
```

### 2. Run the Docker Container
```bash
docker run -d -p 5000:5000 python-azure-app
```
- Open browser and visit:
```
http://127.0.0.1:5000
```

---

## 🔥 CI/CD with GitHub Actions
- Push code to `main` branch → GitHub Actions builds and deploys the app to Azure automatically.

---

## ✅ Summary
✅ Flask-based Python app  
✅ Docker containerized deployment  
✅ CI/CD with GitHub Actions  
✅ Hosted on Azure  

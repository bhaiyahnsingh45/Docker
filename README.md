
# 🐳 Docker Guide for Python, Streamlit & FastAPI Projects ![Docker Logo](https://user-images.githubusercontent.com/72096831/200459883-2ff14cb5-9378-4e5a-bbae-c8d423fb9220.png)

A complete Docker workflow guide for Python-based applications including **Streamlit**, **FastAPI**, **GLPK (Pyomo)**, **Vector Databases**, and **Cloud Deployment (AWS & Azure)**.



---

## 📦 1. Install Docker (Mac & Windows)

### 🍎 Mac (Docker Desktop)

1. Download Docker Desktop:
   👉 [https://www.docker.com/products/docker-desktop](https://www.docker.com/products/docker-desktop)

2. Install and start Docker

3. Verify installation:

```bash
docker --version
docker run hello-world
```

---

### 🪟 Windows (Using WSL2)

#### Step 1: Install WSL

```bash
wsl --install
```

#### Step 2: Install Docker Desktop (with WSL backend)

* Download Docker Desktop
* Enable:

  * ✅ WSL 2 integration
  * ✅ Ubuntu (or your distro)

#### Step 3: Verify

```bash
docker --version
docker run hello-world
```

---

## 📁 2. Dockerfile Examples

### 🐍 Basic Python App

```dockerfile
FROM python:3.8

WORKDIR /folder_name

COPY . /folder_name

RUN pip install -r requirements.txt

EXPOSE 5000

CMD ["python", "run.py"]
```

---

### 📊 Streamlit App with GLPK

```dockerfile
FROM python:3.9-slim

WORKDIR /app

COPY . /app

RUN apt-get update && \
    apt-get install -y glpk-utils && \
    pip install pyomo && \
    pip install -r requirements.txt

EXPOSE 8501

ENTRYPOINT ["streamlit", "run", "main.py", "--server.port=8501", "--server.address=0.0.0.0"]
```

---

### ⚡ FastAPI App

```dockerfile
FROM python:3.10

WORKDIR /app

COPY . /app

RUN apt-get update && \
    apt-get install -y glpk-utils && \
    pip install pyomo && \
    pip install -r requirements.txt

EXPOSE 8000

CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
```

---

## 📦 3. Docker Image Commands

| Action         | Command                             |
| -------------- | ----------------------------------- |
| 🔻 Pull image  | `docker pull username/image_name`   |
| ⬆️ Push image  | `docker push username/image_name`   |
| 🛠 Build image | `docker build -t image_name .`      |
| 📋 List images | `docker images`                     |
| ❌ Remove image | `docker rmi image_name`             |
| 💾 Save image  | `docker save image_name > file.tar` |
| 📦 Load image  | `docker load -i file.tar`           |

---

## 📦 4. Docker Container Commands

| Action              | Command                             |
| ------------------- | ----------------------------------- |
| ▶️ Run container    | `docker run -it image_name`         |
| ⏹ Stop container    | `docker stop container_name`        |
| 🗑 Remove container | `docker rm container_name`          |
| 🔍 List containers  | `docker ps -a`                      |
| 💬 Enter container  | `docker exec -it container_id bash` |

---

## 🚀 5. Run Container

```bash
docker run -d -p 5000:5000 --name my_container image_name
```

---

## 🧩 6. Docker Compose

```bash
docker-compose up -d
docker-compose down
```

---

# 🔍 7. Run Vector Databases (OpenSearch & Elasticsearch)

---

## 🟡 OpenSearch (Recommended for RAG / AI)

### Pull Image

```bash
docker pull opensearchproject/opensearch:latest
```

### Run Container

```bash
docker run -d \
  -p 9200:9200 \
  -p 9600:9600 \
  -e "discovery.type=single-node" \
  --name opensearch \
  opensearchproject/opensearch:latest
```

### Access

```
http://localhost:9200
```

---

## 🔵 Elasticsearch

### Pull Image

```bash
docker pull elasticsearch:8.12.0
```

### Run Container

```bash
docker run -d \
  -p 9200:9200 \
  -e "discovery.type=single-node" \
  --name elasticsearch \
  elasticsearch:8.12.0
```

---

## 📌 Notes

* OpenSearch = better for **open-source + RAG**
* Elasticsearch = enterprise + paid features

---

# 🏗 8. Build, Tag & Push Docker Image

---

## Step 1: Build Image

```bash
docker build -t my_app .
```

---

## Step 2: Tag Image

```bash
docker tag my_app username/my_app:latest
```

---

## Step 3: Login to Docker Hub

```bash
docker login
```

---

## Step 4: Push Image

```bash
docker push username/my_app:latest
```

---

# ☁️ 9. Push Docker Image to AWS (ECR)

---

## Step 1: Create ECR Repo

```bash
aws ecr create-repository --repository-name my_app
```

---

## Step 2: Authenticate

```bash
aws ecr get-login-password --region ap-south-1 | \
docker login --username AWS --password-stdin <account_id>.dkr.ecr.ap-south-1.amazonaws.com
```

---

## Step 3: Tag Image

```bash
docker tag my_app:latest <account_id>.dkr.ecr.ap-south-1.amazonaws.com/my_app
```

---

## Step 4: Push

```bash
docker push <account_id>.dkr.ecr.ap-south-1.amazonaws.com/my_app
```

---

# ☁️ 10. Push Docker Image to Azure (ACR)

---

## Step 1: Login

```bash
az login
```

---

## Step 2: Create Registry

```bash
az acr create --resource-group myGroup --name myRegistry --sku Basic
```

---

## Step 3: Login to ACR

```bash
az acr login --name myRegistry
```

---

## Step 4: Tag Image

```bash
docker tag my_app myregistry.azurecr.io/my_app
```

---

## Step 5: Push

```bash
docker push myregistry.azurecr.io/my_app
```

---

# 🔧 11. Troubleshooting: pip DNS Issue

### Error:

```
Retrying after connection broken...
```

### Fix:

```bash
sudo nano /etc/resolv.conf
```

Add:

```
nameserver 8.8.8.8
nameserver 8.8.4.4
```


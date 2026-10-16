# DevOps Tools – Installation and Configuration Guide

Environment: Windows 10/11, WSL Ubuntu, PowerShell (Administrator)

## 1. Install WSL and Ubuntu

PowerShell (Administrator):

```
wsl --install
```

OR

```
wsl --install -d Ubuntu
```

Verify:

```
wsl --list --verbose
```

Ubuntu:

```
sudo apt update -y
sudo apt upgrade -y
```

## 2. Install Python

Ubuntu:

```
sudo apt install python3 python3-pip python-is-python3 -y
python --version
pip3 --version
```

Windows:

```
choco install python -y
python --version
```

## 3. Install Git

Ubuntu:

```
sudo apt install git -y
git --version
```

Windows:

```
choco install git -y
git --version
```

Git configuration:

```
git config --global user.name "Your Name"
git config --global user.email "your-email@example.com"
git config --global init.defaultBranch main
git config --list
```

## 4. Install Docker

Windows:

[Download Docker Desktop](https://docs.docker.com/desktop/setup/install/windows-install/?utm_source=chatgpt.com)

Enable WSL 2 integration in Docker Desktop settings.

Ubuntu (native Docker Engine):

```
sudo apt update -y
sudo apt install docker.io -y

sudo systemctl enable --now docker
sudo usermod -aG docker $USER
newgrp docker

docker --version
docker ps
docker images
```

Test:

```
docker run hello-world
```

Note: When using Docker Desktop with WSL integration, a separate Docker Engine installation inside WSL is normally unnecessary.

## 5. Install Minikube and Kubectl

Windows PowerShell (Administrator):

```
choco install minikube -y
choco install kubernetes-cli -y
```

Start Minikube with Docker Desktop running:

```
minikube start --driver=docker
minikube status
kubectl get nodes
```

Verify:

```
minikube version
kubectl version --client
```

## 6. Install Terraform

Windows:

```
choco install terraform -y
terraform version
```

Ubuntu:

```
sudo apt update -y
sudo apt install wget gpg lsb-release -y

wget -O- https://apt.releases.hashicorp.com/gpg | \
gpg --dearmor | sudo tee /usr/share/keyrings/hashicorp-archive-keyring.gpg > /dev/null

echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | \
sudo tee /etc/apt/sources.list.d/hashicorp.list

sudo apt update
sudo apt install terraform -y
terraform version
```

## 7. Install Visual Studio Code

Download: [https://code.visualstudio.com/](https://code.visualstudio.com/?utm_source=chatgpt.com)

Recommended extensions:

| Publisher | Extension           |
| --------- | ------------------- |
| HashiCorp | HashiCorp Terraform |
| Microsoft | Azure Tools         |
| AWS       | AWS Toolkit         |
| Red Hat   | YAML                |
| Red Hat   | Ansible             |
| Microsoft | Kubernetes          |
| GitHub    | GitHub Actions      |
| GitHub    | GitHub Copilot      |
| Microsoft | Remote - WSL        |

## 8. Configure GitHub

Sign up: [https://github.com/](https://github.com/?utm_source=chatgpt.com)

Generate an SSH key:

```
ssh-keygen -t ed25519 -C "your-email@example.com"
```

Display public key:

```
cat ~/.ssh/id_ed25519.pub
```

Add the public key in GitHub → Settings → SSH and GPG keys.

Verify:

```
ssh -T git@github.com
```

## 9. Configure Docker Hub

Sign up: [https://hub.docker.com/](https://hub.docker.com/?utm_source=chatgpt.com)

Login:

```
docker login
```

Verify:

```
docker info
```

## 10. Install Application Runtimes

### Java (JDK 21)

Ubuntu:

```
sudo apt install openjdk-21-jdk -y
java -version
javac -version
```

Windows:

```
choco install temurin21 -y
java -version
```

### Node.js

Download: [https://nodejs.org/en](https://nodejs.org/en?utm_source=chatgpt.com)

Windows:

```
choco install nodejs-lts -y
node -v
npm -v
```

### .NET SDK

Download: [https://dotnet.microsoft.com/en-us/download](https://dotnet.microsoft.com/en-us/download?utm_source=chatgpt.com)

Verify:

```
dotnet --version
```

### Go

Download: [https://go.dev/dl/](https://go.dev/dl/?utm_source=chatgpt.com)

Verify:

```
go version
```

## 11. Install Jenkins

Ubuntu / WSL Ubuntu with systemd enabled:

Reference: [https://www.jenkins.io/doc/book/installing/linux/](https://www.jenkins.io/doc/book/installing/linux/?utm_source=chatgpt.com)

Install Java:

```
sudo apt update -y
sudo apt install fontconfig openjdk-21-jre wget -y
java -version
```

Add Jenkins repository:

```
sudo mkdir -p /etc/apt/keyrings

sudo wget -O /etc/apt/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2026.key

echo "deb [signed-by=/etc/apt/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian-stable binary/" | \
sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null
```

Install Jenkins:

```
sudo apt update
sudo apt install jenkins -y
```

Start Jenkins:

```
sudo systemctl enable --now jenkins
sudo systemctl status jenkins
```

Configure Jenkins Docker access (native Docker Engine on the same host):

```
sudo usermod -aG docker jenkins
sudo systemctl restart jenkins
```

Get initial administrator password:

```
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

Access:

```
http://localhost:8080
```

## 12. Install Ansible

Ubuntu:

```
sudo apt update -y
sudo apt install ansible -y
ansible --version
```

Test local connection:

```
ansible localhost -m ping -c local
```

## 13. Install AWS CLI

Windows:

```
choco install awscli -y
aws --version
```

Configure:

```
aws configure
```

Verify authenticated identity:

```
aws sts get-caller-identity
```

## 14. Install Azure CLI

Windows:

```
choco install azure-cli -y
az version
```

Login:

```
az login
az account show
```

## 15. Install Helm

Windows:

```
choco install kubernetes-helm -y
helm version
```

Verify:

```
helm list
```

## 16. Install Prometheus and Grafana

Using Docker Compose:

Create `docker-compose.yml`:

```
services:
  prometheus:
    image: prom/prometheus:latest
    ports:
      - "9090:9090"
    volumes:
      - ./prometheus.yml:/etc/prometheus/prometheus.yml:ro

  grafana:
    image: grafana/grafana:latest
    ports:
      - "3000:3000"
```

Create `prometheus.yml`:

```
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: prometheus
    static_configs:
      - targets: ["localhost:9090"]
```

Start:

```
docker compose up -d
docker compose ps
```

Access:

| Tool       | URL                                                                   |
| ---------- | --------------------------------------------------------------------- |
| Prometheus | [http://localhost:9090](http://localhost:9090?utm_source=chatgpt.com) |
| Grafana    | [http://localhost:3000](http://localhost:3000?utm_source=chatgpt.com) |

Grafana initial login is typically `admin` / `admin`. Change the password when prompted.

Configure Grafana data source:

1. Open Grafana → Connections → Data sources.
2. Select Prometheus.
3. Enter `http://prometheus:9090`.
4. Select Save & test.

## 17. Verify All Installed Tools

```
python --version
git --version
docker --version
kubectl version --client
minikube version
terraform version
java -version
node -v
npm -v
dotnet --version
go version
ansible --version
aws --version
az version
helm version
```

Additional service verification (Ubuntu):

```
sudo systemctl status jenkins
sudo systemctl status docker
```

Run Windows/Chocolatey commands in PowerShell and Ubuntu/APT commands inside Ubuntu. Not every tool needs to be installed in both environments.

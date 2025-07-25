# Defending Containers Through Advanced Security Features of Docker and Kubernetes
This repository demonstrates how to secure containerized environments using best practices and open-source tools in Docker and Kubernetes. It includes hands-on steps to set up Minikube, deploy containers with namespace isolation, enforce resource limits, and run vulnerability scans.

# Objective
To provide a secure deployment environment using:
- Docker for containerization
- Kubernetes for orchestration
- PowerShell scripts for automation
- Tools like Trivy and Kube-Bench for scanning and hardening

# Technologies & Tools
- Docker
- Kubernetes (Minikube)
- PowerShell
- Trivy, Kube-Bench, Kubectl
- Windows with Hyper-V

# Project Structure
docker-kubernetes-container-security/
├── docker/             # Dockerfile and hardening configs
├── kubernetes/         # YAML manifests (deployment, namespace, limits)
├── scripts/            # Automation scripts (PowerShell)
├── docs/               # Documentation and notes
├── .gitignore          # Git ignore rules
├── LICENSE             # Open-source license
└── README.md           # You're here!

# Setup Instructions
### Step 1: System Preparation
- Run PowerShell as Administrator.
- Enable **Hyper-V**:
  ```powershell
  dism.exe /Online /Enable-Feature:Microsoft-Hyper-V /All
  ```
- Reboot your system after enabling Hyper-V.

### Step 2: Minikube Installation
```powershell
Invoke-WebRequest -Uri https://github.com/kubernetes/minikube/releases/latest/download/minikube-windows-amd64.exe -OutFile 'C:\minikube\minikube.exe'

$env:Path += ";C:\minikube"
[Environment]::SetEnvironmentVariable("Path", $env:Path, [System.EnvironmentVariableTarget]::Machine)
```
### Step 3: Start Minikube Cluster
```powershell
minikube config set driver hyperv
minikube config set memory 4096
minikube config set cpus 2
minikube start
```
### Step 4: Deploy NGINX in Namespaces
```powershell
kubectl create namespace development
kubectl create namespace production

kubectl run nginx-dev --image=nginx:1.23-alpine -n development
kubectl run nginx-prod --image=nginx:1.23-alpine -n production
```
## PowerShell Scripts
Located under `/scripts` folder for automation:

| Script Name             | Purpose                                |
|-------------------------|----------------------------------------|
| start_minikube    | Launches Minikube with settings        |
| deploy_nginx      | Deploys NGINX pods to namespaces       |
| apply_resource_limits | Applies CPU/memory limits to pods    |
| security_scan     | Trivy-based vulnerability scan         |

##  Dockerfile Overview
```Dockerfile
FROM nginx:1.23-alpine

RUN rm /etc/nginx/conf.d/default.conf
# COPY nginx.conf /etc/nginx/nginx.conf

HEALTHCHECK --interval=30s --timeout=10s --start-period=5s CMD wget --spider http://localhost || exit 1

USER nginx
EXPOSE 80
```

- Minimal base image
- Healthcheck added
- Runs as non-root user
- Ready for K8s

## Security Features Demonstrated
- Namespace isolation (`development`, `production`)
- Container hardening (non-root, no default configs)
- Resource limits for pods
- Image scanning using **Trivy**
- Pod security via **Kube-Bench**

## Future Enhancements
- Integrate CI/CD for auto-scan and deploy
- Secrets management via Vault
- Network Policies and RBAC restrictions

##  Author

**Bansi Pambhar**  
🔗 [GitHub Profile](https://github.com/bansi-pambhar)




# Chapter 7: Container Apps & Azure Container Registry (ACR)

## Overview
This chapter focuses on container-based application deployment using Azure Container Apps and ACR. You will build a Docker image, push it to ACR, deploy it to Azure, and configure scaling rules.

## What You Will Learn
- Build and push container images to ACR  
- Deploy Azure Container Apps  
- Use Managed Identities for secure image pulls  
- Configure autoscaling based on HTTP load or CPU usage  

## Scenario
Developers want to deploy a microservice quickly without maintaining infrastructure. You will containerize an app, store it in ACR, and deploy it into Azure Container Apps.

---

## Lab Activities

### 1. Create Azure Container Registry
```bash
az acr create -n <acrName> -g <rgName> --sku Basic
```

### 2. Build & Push Docker Image
```bash
az acr login --name <acrName>
docker build -t <acrName>.azurecr.io/myapp:v1 .
docker push <acrName>.azurecr.io/myapp:v1
```

### 3. Create a Container Apps Environment
Use the portal:
- Resource: **Container Apps Environment**
- Choose an existing Log Analytics workspace or create one  

### 4. Deploy a Container App
Use Managed Identity:
- Assign **AcrPull** role:
```bash
az role assignment create   --assignee <clientId>   --scope $(az acr show -n <acrName> --query id -o tsv)   --role "AcrPull"
```

Deploy:
- Image: `<acrName>.azurecr.io/myapp:v1`
- CPU: 0.5  
- Memory: 1.0 Gi  

### 5. Autoscaling
Container App → **Scale**

Example rule:
- Scale out: when requests > 20 RPS  
- Minimum replicas: 1  
- Maximum replicas: 5  


# Best Buy Cloud-Native Application

This repository contains a cloud-native, microservices-based demo application for Best Buy’s online store, inspired by the Algonquin Pet Store (On Steroids) architecture. The application demonstrates browsing products, placing orders, and managing product inventory. It also integrates with AI services for generating product descriptions and images.

## Updated Architecture Diagram

![bestar](https://github.com/user-attachments/assets/670e611f-9e66-43a8-a534-d46136212ed9)

**Key Components:**
- **Store-Front:** Customer-facing UI for browsing products and placing orders.
- **Store-Admin:** Employee-facing UI for managing product catalog (CRUD) and viewing orders.
- **Order-Service:** Accepts incoming orders from Store-Front, sends order data to Azure Service Bus queue.
- **Product-Service:** Manages product data, integrates with AI-Service for product descriptions and images.
- **Makeline-Service:** Listens to Azure Service Bus, processes orders, updates MongoDB with order statuses.
- **AI-Service:** Uses GPT-4 and DALL-E (via OpenAI or Azure OpenAI) for generating product descriptions and images.
- **MongoDB:** Persists product and order data.
- **rabbitMq:** RabbitMQ for an order queue.

## Application Functionality

1. **Customers (Store-Front):**
   - View a list of products with AI-generated descriptions and images.
   - Add products to the cart and place orders.

2. **Admins (Store-Admin):**
   - Create, read, update, and delete products.
   - Trigger AI-Service calls to generate new product descriptions and images.
   - View incoming and processed orders.

3. **Order Processing (Order-Service & Makeline-Service):**
   - Order-Service sends new orders to Azure Service Bus.
   - Makeline-Service listens to the queue, processes orders, and updates their status in MongoDB.

4. **AI Integration (AI-Service):**
   - GPT-4: Generates marketing-friendly product descriptions.
   - DALL-E: Generates product images based on the product description keywords.

## Deployment Instructions

### Prerequisites

- **Kubernetes Cluster (AKS recommended)**
  - Ensure `kubectl` is configured to communicate with your cluster.
- **OpenAI / Azure OpenAI**
 Set Up the AI Backing Services
To enable AI-generated product descriptions and image generation features, you will deploy the required **Azure OpenAI Services** for GPT-4 (text generation) and DALL-E 3 (image generation). This step is essential to configure the **AI Service** component in the Best buy Store application.
## Steps to Deploy

 **Clone this repository.**
 
###  Retrieve and Configure API Keys

1. **Get API Keys**:
   - Go to the **Keys and Endpoints** section of your Azure OpenAI resource.
   - Copy the **API Key (API key 1)** and **Endpoint URL**.

2. **Base64 Encode the API Key**:
   - Use the following command to Base64 encode your API key:
     ```bash
     echo -n "<your-api-key>" | base64
     ```
   - Replace `<your-api-key>` with your actual API key.

---

### Update AI Service Deployment Configuration in the `Deployment Files` folder.
1. **Modify Secretes YAML**:
   - Edit the `secrets.yaml` file.
   - Replace `OPENAI_API_KEY` placeholder with the Base64-encoded value of the `API_KEY`. 
2. **Modify Deployment YAML**:
   - Edit the `aps-all-in-one.yaml` file.
   - Replace the placeholders with the configurations you retrieved:
     - `AZURE_OPENAI_DEPLOYMENT_NAME`: Enter the deployment name for GPT-4.
     - `AZURE_OPENAI_ENDPOINT`: Enter the endpoint URL for the GPT-4 deployment.
     - `AZURE_OPENAI_DALLE_ENDPOINT`: Enter the endpoint URL for the DALL-E 3 deployment.
     - `AZURE_OPENAI_DALLE_DEPLOYMENT_NAME`: Enter the deployment name for DALL-E 3.

   Example configuration in the YAML file:
   ```yaml
   - name: AZURE_OPENAI_API_VERSION
     value: "2024-07-01-preview"
   - name: AZURE_OPENAI_DEPLOYMENT_NAME
     value: "gpt-4-deployment"
   - name: AZURE_OPENAI_ENDPOINT
     value: "https://<your-openai-resource-name>.openai.azure.com/"
   - name: AZURE_OPENAI_DALLE_ENDPOINT
     value: "https://<your-openai-resource-name>.openai.azure.com/"
   - name: AZURE_OPENAI_DALLE_DEPLOYMENT_NAME
     value: "dalle-3-deployment"
   ```

2. **Set up ConfigMaps and Secrets:**
   ```bash
   kubectl apply -f Deployment_Files/config-maps.yaml
   kubectl apply -f Deployment_Files/secrets.yaml


### Deploy the application:
Deploy all the yaml files from the Deployment Files folder one by one
```bash
kubectl apply -f Deployment_Files/store-front.yaml
kubectl apply -f Deployment_Files/store-admin.yaml
kubectl apply -f Deployment_Files/product-service.yaml
kubectl apply -f Deployment_Files/order-service.yaml
kubectl apply -f Deployment_Files/ai-service.yaml
kubectl apply -f Deployment_Files/mongodb.yaml
kubectl apply -f Deployment_Files/make-line.yaml
kubectl apply -f Deployment_Files/rabbitmq.yaml
kubectl apply -f Deployment_Files/admin-tasks.yaml
````
## Validate the deployment:
```bash
kubectl get pods
kubectl get services

````


1. **Table for Microservice Repository**:
   - Visit the GitHub repositories for the services listed below and fork them into your own GitHub account:
   
   | Service            | Description                                | Github Repo                                                                 |
   |--------------------|--------------------------------------------|-----------------------------------------------------------------------------|
   | `store-front`      | Web app for customers to place orders      | [store-front-A2](https://github.com/meetpatel1389/store-front-A2.git)       |
   | `store-admin`      | Web app for store employees                | [store-admin-A2](https://github.com/meetpatel1389/store-admin-A2.git)       |
   | `order-service`    | Handles order placement                    | [order-service-A2](https://github.com/meetpatel1389/order-service-A2.git)   |
   | `product-service`  | Handles CRUD operations on products        | [product-service-A2](https://github.com/meetpatel1389/product-service-A2.git) |
   | `makeline-service` | Processes and completes orders             | [makeline-service-A2](https://github.com/meetpatel1389/makeline-service-A2.git) |
   | `ai-service`       | AI-based product descriptions and images   | [ai-service-A2](https://github.com/meetpatel1389/ai-service-A2.git)         |
   | `virtual-customer` | Simulates customer order creation          | [virtual-customer-A2](https://github.com/meetpatel1389/virtual-customer-A2.git) |
   | `virtual-worker`   | Simulates order completion                 | [virtual-worker-A2](https://github.com/meetpatel1389/virtual-worker-A2.git)  |
## Implement a CI/CD Pipeline for Each Microservice
1. **Fork the all the above Repositories**
2. 2. **Set Up Secrets**
   - Go to **Settings > Secrets and variables > Actions** in each forked repository.
   - Add the following repository secrets:
     - `DOCKER_USERNAME`: Your Docker Hub username.
     - `DOCKER_PASSWORD`: Your Docker Hub password.
     - `KUBE_CONFIG_DATA`: Base64-encoded content of your Kubernetes configuration file (`kubeconfig`). This is used for authentication with your Kubernetes cluster.
       - Run the following commands to get `KUBE_CONFIG_DATA` after connecting to your AKS cluster:
     ```bash
     cat ~/.kube/config | base64 -w 0 > kube_config_base64.txt
     ``` 
     Use the content of this file as the value for the KUBE_CONFIG_DATA secret in GitHub.
     3. **Set Up Environment Variables (Repository Variables)**
   - Go to **Settings > Secrets and variables > Actions** in each forked repository.
   - Add the following repository variables:
     - `DOCKER_IMAGE_NAME`: The name of the Docker image to be built, tagged, and pushed (For example: `store-front-l9`).
     - `DEPLOYMENT_NAME`: The name of the Kubernetes deployment to update (For example: `store-front`).
     - `CONTAINER_NAME`: The name of the container within the Kubernetes deployment to update (For example: `store-front`).
     
---

## Step 2: Create the Workflow File
1. **Create the Workflow Directory**:
   - In each forked repository, create a directory named `.github/workflows/`.

2. **Add the Workflow File**:
   - Copy the `ci_cd.yaml` file from the `Workflow Files` folder into the `.github/workflows/` directory of each forked repository.
---
## Step 3: Understand the Workflow Structure

- The `ci_cd .yaml` file defines a **GitHub Actions workflow** that automates the CI/CD pipeline for your application.
# Table of Docker Images

| Service          | Docker Image Link   |
|------------------|---------------------|
| Store-Front       |  [DOCKER HUB LINK](https://hub.docker.com/r/meetpatel1389/store-front)    |
| Store-Admin     |  [DOCKER HUB LINK ](https://hub.docker.com/r/meetpatel1389/store-admin)   |
| Order-Service     |  [DOCKER HUB LINK](https://hub.docker.com/r/meetpatel1389/order-service)    |
| Product-Service   | [Docker Hub Link  ](https://hub.docker.com/r/meetpatel1389/product-service)   |
| Makeline-Service  | [Docker Hub Link](https://hub.docker.com/r/meetpatel1389/makeline-service/tags)     |
| AI-Service        | [Docker Hub Link](https://hub.docker.com/r/meetpatel1389/ai-service)     |
| Vitrual-customer    |  [DOCKER HUB LINK ](https://hub.docker.com/r/meetpatel1389/virtual-customer)   |
| Vitrual- worker    | [ DOCKER HUB LINK ](https://hub.docker.com/r/meetpatel1389/virtual-worker)   |

# Demo video
[Click_Here_to_Watch_Video](https://youtu.be/B1EGbN0Xy8s)


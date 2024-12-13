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
- **OpenAI / Azure OpenAI Credentials**
  - Retrieve API keys and/or endpoint and store them as Kubernetes Secrets.
- **MongoDB Credentials & Storage**
  - Prepare PersistentVolume and PersistentVolumeClaim for MongoDB data.
  - Store MongoDB credentials as Kubernetes Secrets.

## Steps to Deploy

1. **Clone the repository.**

2. **Set up ConfigMaps and Secrets:**
   ```bash
   kubectl apply -f Deployment_Files/config-maps.yaml
   kubectl apply -f Deployment_Files/secrets.yaml


### Deploy the application:
```bash
kubectl apply -f Deployment_Files/
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

# Table of Docker Images

| Service          | Docker Image Link   |
|------------------|---------------------|
| Store-Front       |  (https://hub.docker.com/r/meetpatel1389/store-front)    |
| Order-Service     | Docker Hub Link     |
| Product-Service   | Docker Hub Link     |
| Makeline-Service  | Docker Hub Link     |
| AI-Service        | Docker Hub Link     |

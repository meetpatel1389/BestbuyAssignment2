# Best Buy Cloud-Native Demo Application

This repository contains a cloud-native, microservices-based demo application for Best Buy’s online store, inspired by the Algonquin Pet Store (On Steroids) architecture. The application demonstrates browsing products, placing orders, and managing product inventory. It also integrates with AI services for generating product descriptions and images, and uses a managed backing service (Azure Service Bus) for order queueing.

## Updated Architecture Diagram

![Logical Application Architecture Diagram](assets/Algonquin%20Pet%20Store%20On%20Steroids.png)

**Key Components:**
- **Store-Front:** Customer-facing UI for browsing products and placing orders.
- **Store-Admin:** Employee-facing UI for managing product catalog (CRUD) and viewing orders.
- **Order-Service:** Accepts incoming orders from Store-Front, sends order data to Azure Service Bus queue.
- **Product-Service:** Manages product data, integrates with AI-Service for product descriptions and images.
- **Makeline-Service:** Listens to Azure Service Bus, processes orders, updates MongoDB with order statuses.
- **AI-Service:** Uses GPT-4 and DALL-E (via OpenAI or Azure OpenAI) for generating product descriptions and images.
- **MongoDB:** Persists product and order data.
- **Azure Service Bus (Managed Queue):** Replaces RabbitMQ for message passing between Order-Service and Makeline-Service.

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
- **Azure Service Bus**
  - Create a Service Bus namespace and a queue (e.g., `orders`).
  - Store connection strings as Kubernetes Secrets.
- **OpenAI / Azure OpenAI Credentials**
  - Retrieve API keys and/or endpoint and store them as Kubernetes Secrets.
- **MongoDB Credentials & Storage**
  - Prepare PersistentVolume and PersistentVolumeClaim for MongoDB data.
  - Store MongoDB credentials as Kubernetes Secrets.

### Steps

1. **Clone Repositories:**
   ```bash
   git clone <Store-Front Repo Link>
   git clone <Store-Admin Repo Link>
   git clone <Order-Service Repo Link>
   git clone <Product-Service Repo Link>
   git clone <Makeline-Service Repo Link>
   git clone <AI-Service Repo Link>

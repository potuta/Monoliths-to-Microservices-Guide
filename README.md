# 📘 Monoliths to Microservices Guide

This is a comprehensive guide of the things I learned in understanding modern application architecture — from monoliths to microservices, containers, Docker, Kubernetes, reverse proxies, databases, and micro frontends. Includes conceptual explanations and practical examples.

---

## 1. Monolith vs Microservices

### Monolith
- **Definition:** A single, unified codebase where all features are deployed together.
- **Pros:**
  - Easier to start.
  - Simple deployment pipeline.
  - Shared memory and transactions.
- **Cons:**
  - Harder to scale specific parts.
  - Long build & deploy times.
  - Tightly coupled teams and code.

### Microservices
- **Definition:** Independent services, each owning a feature/domain (e.g., Payments, Users, Orders).
- **Pros:**
  - Independent scaling & deployment.
  - Technology flexibility.
  - Easier team ownership.
- **Cons:**
  - Distributed system complexity.
  - Requires inter-service communication (APIs, messaging).
  - Distributed transactions are harder.

👉 **Rule of thumb:** Start monolith → migrate to microservices when scaling, agility, or team independence requires it.

---

## 2. Containers & Docker

### What are Containers?
- Lightweight, isolated environments for running apps.
- Share the host OS kernel (unlike full VMs).
- Ensure consistency: “runs on my machine” = “runs on prod.”

### Docker
- A tool to build and run containers.
- Packages your app + dependencies into an **image**.
- Runs that image as a **container**.

### IIS vs Containers
- Traditional Windows hosting: app runs under IIS.
- Containers: app runs self-hosted (Kestrel for ASP.NET Core).
- IIS/Nginx/ALB then act as reverse proxies, not app hosts.

---

## 3. Kubernetes (K8s)

### Why?
When you have many containers across servers, you need orchestration:
- Scheduling (which node runs which container).
- Networking (service discovery).
- Scaling (replicas, auto-scaling).
- Healing (restart crashed containers).

### Key Concepts
- **Pod** → Smallest deployable unit (one or more containers).
- **Deployment** → Ensures pods run with desired state (replicas, version).
- **Service** → Stable DNS name + load balancing across pods.
- **Ingress** → Reverse proxy + routing + SSL termination.

### Local vs Cloud
- Local: Minikube or Docker Desktop.
- Cloud: AWS EKS, Azure AKS, GCP GKE.

---

## 4. Reverse Proxies & SSL Termination

### Why Reverse Proxy?
- Offload SSL/TLS (so apps don’t manage certs directly).
- Centralized routing for multiple services.
- Add caching, compression, security.

### Options
- **Windows:** IIS.
- **Linux:** Nginx or Apache.
- **Cloud:** AWS ALB, Azure Front Door, GCP Load Balancer.

### SSL Termination
- Happens at reverse proxy/load balancer.
- Proxy forwards plain HTTP to backend services.

---

## 5. Databases in Microservices

### Shared vs Separate
- **Shared DB**: Easier, but couples services tightly.
- **Separate DBs**: Each service owns its schema/DB, ensures independence.

### Deployment Options
1. Same DB server, multiple schemas.
2. Different DB servers, same engine.
3. Different DB engines per service.

### Distributed Transactions
- Monolith: single DB transaction.
- Microservices: split DBs.
- Solutions:
  - **Saga pattern** (orchestration/choreography).
  - **Outbox pattern** (event sourcing).
  - **Message queues** for eventual consistency.

---

## 6. Micro Frontends

### Concept
- Just like backend microservices, the UI can be split into independent modules.
- Each team delivers its feature as a micro app.

### Approaches
1. **Server-side composition**: Different routes served by different apps (e.g., `/payments`).
2. **Client-side composition**: Module Federation, Web Components.
3. **Iframe embedding**: Legacy/simple isolation.

### Benefits
- Independent deployment for UI teams.
- Technology flexibility.

### Challenges
- Styling consistency.
- Shared state.
- Routing coordination.

---

## 7. Putting It All Together

### Local Dev
- Use **Docker Compose** to run multiple services + DB on one machine.
- Networking via Docker internal network.

### Scaling
- Move to **Kubernetes** for orchestration.
- Services talk via K8s DNS (e.g., `http://payment`).

### Production (AWS Example)
- **ECS** (simpler): Deploy containers directly.
- **EKS** (Kubernetes): Full orchestration.
- **ALB**: SSL termination + routing.
- **RDS**: Managed DBs for each service.
- **Secrets Manager/Parameter Store**: Config & secret storage.

---

## ✅ Key Takeaways
- **Monoliths** are simpler, microservices bring scalability & independence.
- **Containers** package apps, **Docker** runs them.
- **Kubernetes** orchestrates containers across servers.
- **Reverse proxies** (IIS, Nginx, ALB) handle SSL and routing.
- **Databases** should be owned per service (not shared).
- **Micro frontends** extend the microservice philosophy to the UI.
- Start small, evolve as complexity and scale demand it.

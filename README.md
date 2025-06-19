# 🗳️ Kubernetes Voting App - K8s Microservices Project

This project showcases a **microservices-based voting application** deployed on a **Kubernetes cluster** using YAML manifests. It simulates a real-world system where users cast votes, which are processed and stored, and results are displayed in real time.

---

## 🚀 Project Overview

The application architecture consists of 5 core components running in isolated containers:

- 🐍 **Voting App** (Frontend - Python Flask)
- 🌐 **Result App** (Frontend - Node.js)
- 🔄 **Worker App** (Backend - .NET)
- 🧠 **Redis** (In-memory store for temporary vote storage)
- 🐘 **PostgreSQL** (Persistent storage for final vote results)

All services are deployed inside a custom Kubernetes namespace: `**vote**`.

---

## 🧱 Architecture Flow

1. **Users vote** through the *Voting App* UI.
2. Votes are temporarily stored in **Redis**.
3. The **Worker App** retrieves votes from Redis and pushes them to **PostgreSQL**.
4. The **Result App** reads from PostgreSQL and displays live results.

```mermaid
graph TD;
    A[User] --> B[Vote App];
    B --> C[Redis];
    C --> D[Worker App];
    D --> E[PostgreSQL];
    E --> F[Result App];

## 📦 Container Images Used

| Component       | Image                                       |
|----------------|---------------------------------------------|
| Voting App     | `kodekloud/examplevotingapp_vote`           |
| Result App     | `kodekloud/examplevotingapp_result`         |
| Worker App     | `kodekloud/examplevotingapp_worker`         |
| Redis          | `redis:alpine`                              |
| PostgreSQL     | `postgres:15-alpine`                        |

## ⚙️ Key Kubernetes Features Used

- ✅ **Namespaces**: All components are isolated under the `vote` namespace.
- 🛠️ **Deployments**: Ensures declarative, self-healing pod management for each component.
- 🌐 **NodePort Services**: Used to expose internal services to users via static ports.
- 💾 **EmptyDir Volumes**: Temporary volumes are used for `redis` and `postgres` data.
- 🌿 **Environment Variables**: Injected into containers like PostgreSQL for configuration.

## 🧪 How to Deploy

> Ensure your Kubernetes cluster (Minikube, kind, or any cloud provider) is running.

1. **Create the Namespace**:
```bash
kubectl create namespace vote
2. **Apply All YAML Files**:
```bash
kubectl apply -f . -n vote


---

```markdown
## 📊 Final Output

Once deployed successfully:

- ✅ The **Voting App** allows users to vote between two choices.
- 🔄 Votes are **temporarily stored in Redis**.
- ⚙️ The **Worker App** transfers these votes into **PostgreSQL**.
- 📈 The **Result App** displays real-time vote results.
- 🧩 All components communicate seamlessly inside the `vote` namespace, managed by Kubernetes.

This setup mimics a production-ready, microservices architecture running on Kubernetes.

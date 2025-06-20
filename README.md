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

Ensure your Kubernetes cluster (Minikube, kind, or any cloud provider) is running.

1. **Create the Namespace**:
```
kubectl create namespace vote
```

2. **Apply All YAML Files**:
```
kubectl create -f voting-app-deployment.yaml
kubectl create -f voting-app-service.yaml
kubectl create -f result-app-deployment.yaml
kubectl create -f result-app-service.yaml
kubectl create -f redis-deployment.yaml
kubectl create -f redis-service.yaml
kubectl create -f postgres-deployment.yaml
kubectl create -f postgres-service.yaml
kubectl create -f worker-app-deployment.yaml
```


## 📊 Final Output

Once deployed successfully:

- ✅ The **Voting App** allows users to vote between two choices.
- 🔄 Votes are **temporarily stored in Redis**.
- ⚙️ The **Worker App** transfers these votes into **PostgreSQL**.
- 📈 The **Result App** displays real-time vote results.
- 🧩 All components communicate seamlessly inside the `vote` namespace, managed by Kubernetes.

You can get the URLs for voting app and result app as follows.

Voting-app:
```
minikube service vote -n vote --url
```
Result-app:
```
minikube service result -n vote --url
```

## Accessing from Outside the Host (e.g., from your local browser when Minikube runs on EC2)
To access the applications from your browser externally (outside the EC2 host), you need to forward ports from your EC2 instance to the service ports inside the cluster using `kubectl port-forward`.

**Voting App Port Forwarding:**

```
kubectl port-forward --address 0.0.0.0 svc/vote 7080:8080
```
**Result App Port Forwarding:**
```
kubectl port-forward --address 0.0.0.0 svc/result 7081:8081
```

- `7080` and `7081` are external ports (Any two ports) you have to define on the EC2 instance. Need to open these in the security group.
- `8080` and `8081` is the internal service port your K8s services expose.
- The flag `--address 0.0.0.0` allows access from outside the localhost (e.g., your browser).

Now you can access the apps using `http://<EC2PublicIp>:7080` for voting app and `http://<EC2PublicIp>:7081` for result app.
![K8's project](https://github.com/user-attachments/assets/39cd164b-9790-407c-b440-d863b31933fa)


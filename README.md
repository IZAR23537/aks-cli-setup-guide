# ⚙️ AKS Cluster Setup via Azure CLI

This guide explains how to create and test a basic **Azure Kubernetes Service (AKS)** cluster using the **Azure CLI**.

---

## 📋 Prerequisites

- An [Azure account](https://azure.com)
- Azure CLI installed → [Install Guide](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli)
- Logged in to Azure CLI:
```bash
az login
```

---

## 🧭 Step-by-Step CLI Instructions

### 1. Set Your Environment Variables
```bash
RESOURCE_GROUP="aks-demo-rg"
CLUSTER_NAME="aks-demo-cluster"
REGION="eastus"
```

---

### 2. Create a Resource Group
```bash
az group create --name $RESOURCE_GROUP --location $REGION
```

✅ **Test**: Run `az group show --name $RESOURCE_GROUP` and confirm creation.

---

### 3. Create the AKS Cluster
```bash
az aks create   --resource-group $RESOURCE_GROUP   --name $CLUSTER_NAME   --node-count 1   --enable-addons monitoring   --generate-ssh-keys
```

✅ **Test**: Run `az aks show --resource-group $RESOURCE_GROUP --name $CLUSTER_NAME`

---

### 4. Connect to the Cluster
```bash
az aks get-credentials --resource-group $RESOURCE_GROUP --name $CLUSTER_NAME
kubectl get nodes
```

✅ **Test**: One node should show up with status "Ready".

---

### 5. Deploy a Test App
```bash
kubectl apply -f test-app/deployment.yaml
kubectl apply -f test-app/service.yaml
kubectl get all
```

✅ **Test**: Confirm that `flask-demo` pod is Running and `flask-service` has an `EXTERNAL-IP`.

Access the app via:
```bash
curl http://<EXTERNAL-IP>
```

---

### 6. Clean Up Resources
```bash
az group delete --name $RESOURCE_GROUP --yes --no-wait
```

---

## 🧪 Test Phase Checklist

| Phase         | Command/Test                        | Expected Outcome                    |
|---------------|-------------------------------------|-------------------------------------|
| Resource Group| `az group show`                     | Group exists                        |
| Cluster       | `az aks show`                       | Cluster details shown               |
| Connect       | `kubectl get nodes`                 | Node status: Ready                  |
| Deploy App    | `kubectl get pods`                  | Pod status: Running                 |
| Access App    | `curl http://<EXTERNAL-IP>`         | Response from Flask app             |
| Clean-up      | `az group delete`                   | Group deletion initiated            |

---
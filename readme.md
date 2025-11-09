# 🧠 Kubernetes Notes

---

## 🧩 PODS

```bash
# Create a Pod
kubectl run pod-name --image=image-name --restart=Never

# Get list of Pods
kubectl get pods

# View Pod logs
kubectl logs pod-name

# Describe Pod details
kubectl describe pod pod-name

# Execute a command inside a Pod
kubectl exec -it pod-name -- sh   # or /bin/bash
```

## ⚙️ ConfigMap

### Create a ConfigMap from literal value
>kubectl create configmap my-config --from-literal=APP_MESSAGE="Hello from ConfigMap"

### Get the ConfigMap in JSON format
>kubectl get configmap busybox-config-env -o json

Use ConfigMap inside a Pod (in container section):

```bash
envFrom:
  - configMapRef:
      name: busybox-config-env
```

## 🔐 Secret
### Create a Secret from literal value
>kubectl create secret generic my-secret --from-literal=APP_PASSWORD=SuperSecret123

Use a Secret in Pod environment variables:
```bash
env:
  - name: APP_PASSWORD
    valueFrom:
      secretKeyRef:
        name: app-secret
        key: APP_PASSWORD
```
### Load all values from both ConfigMap and Secret:

```bash
envFrom:
  - configMapRef:
      name: my-config    # ✅ loads all keys from ConfigMap
  - secretRef:
      name: my-secret    # ✅ loads all keys from Secret
```

## 🚀 Deployment
### Apply a deployment file (e.g., Nginx deployment)

>kubectl apply -f deployment-health-check.yaml

#### 🔍 Probes
livenessProbe → Checks if the container is alive.
🔁 If it fails → Pod is restarted.

readinessProbe → Checks if the container is ready to accept traffic.
🚫 If it fails → Pod is removed from Service endpoints (no restart).


## 🌐 Port Forwarding
### Expose a container port to local (for accessing from outside cluster)
>kubectl port-forward pod/pod-name 8080:80   #(Local port : Container port)


## PC && VPC
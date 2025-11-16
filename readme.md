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


## Jobs and CronJob
To execute some logic once or at particular time, we can schedule job or cronJob
in CronJob : schedule is imp field that need to configured as per requirement.

Note: In Job whatever we add in tempalte that same will be use in cronJob as jobTemplate



## PV && PVC

PV is used to allocate some storage which can be used in pod to maintain the POD data even after POD deleted
We are creating PVC means we claim the data that how much we required from PV and mount that PCV to POD


## Service 
In order to access the application from user perspective we can crete Service which is the entry point for our application

But on local, we need to forward port to access the application outside cluster.
Default type of Service is clusterIP
Like wise we have :

```bash
NodePort
ApplicationLoadBalancer
ClusterIP (Default)
External
```

>k port-forward service/ngnix-service 8282:8181


## RBAC

Role based access controlle

>kubectl auth whoami
>kubectl auth can-i get pods

check the ServiceAccount permission
> kubectlauth can-i get pods -n namespace --as=role-user

After Role binding we can check if the user/SA has the permission
>kubectl auth can-i get pods --as=devops-usr -n apcache



## InitContainer & SideCar Container

Init contianer used to configure something which is required to run the main container
SideCar container same as main container but this is just helper to support the main container

ex: vault-agent : to connect to vault and get the secret 
# Kubernetes Notes

## PODS
kubectl run pod-name --image=image-name restartPolicy=Never

kubectl get pods



## ConfigMap
kubectl create configmap my-config --from-literal=APP_MESSAGE="Hello from ConfigMap"

To get the cofigMap as JSON 
k get configmap busybox-config-env -o json
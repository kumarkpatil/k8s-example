# Kubernetes Notes

## PODS
>kubectl run pod-name --image=image-name restartPolicy=Never

>kubectl get pods

>kubectl logs pod-name

>kubectl describe pod pod-name

>kubectl exec -it pod-name "sh//bin/bash"

## ConfigMap
>kubectl create configmap my-config --from-literal=APP_MESSAGE="Hello from ConfigMap"

To get the cofigMap as JSON 
>kubectl get configmap busybox-config-env -o json

 In order to access configMap env from pods add below line in container section

`envFrom:
        - configMapRef:
            name: busybox-config-env`
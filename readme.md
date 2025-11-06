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

``` 
envFrom:
        - configMapRef:
            name: busybox-config-env
```
## Secret
>kubectl create secret generic my-secret --from-literal=APP_PASSWORD=SuperSecret123

In order to use the secret in POD env, use the below snippest in container section

``` 
env:
      - name:  APP_PASSWORD
        valueFrom:
          secretKeyRef:
            name:  app-secret
```

### Load all values from Secret as well as from ConfigMap

```
envFrom:
        - configMapRef:
            name: my-config    # ✅ loads all keys from ConfigMap
        - secretRef:
            name: my-secret    # ✅ loads all keys from Secret
```            

## Deployment

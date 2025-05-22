#### Create namespace
```sh
kubectl apply -f namespace.yaml
```
#### Check namespaces
```sh
kubectl get namespaces
```
Sample:
```sh
kubectl get namespaces
NAME              STATUS   AGE
default           Active   33m
development       Active   4m46s
kube-node-lease   Active   33m
kube-public       Active   33m
kube-system       Active   33m
production        Active   4m46s
```
#### Delete namespaces
```sh
kubectl delete -f namespace.yaml
```
#### Create Deployment
```sh
kubectl apply -f deployment.yaml
```
Sample:
```sh
kubectl apply -f deployment.yaml
deployment.apps/pod-info-deployment created
```
#### List all deployments in namespace
```sh
kubectl get deployments -n {{namespace}}
```
Sample:
```sh
kubectl get deployments -n development
NAME                  READY   UP-TO-DATE   AVAILABLE   AGE
pod-info-deployment   3/3     3            3           53s
```
#### Get pods in namespace
```sh
kubectl get pods -n {{namespace}}
```
Sample:
```sh
kubectl get pods -n development
NAME                                   READY   STATUS    RESTARTS   AGE
pod-info-deployment-68c8476764-4kd56   1/1     Running   0          4m4s
pod-info-deployment-68c8476764-6rdgv   1/1     Running   0          4m4s
pod-info-deployment-68c8476764-s2pwq   1/1     Running   0          4m4s
```
Sample with extra information (`-o wide` displays more information, like the IP addresses)
```sh
kubectl get pods -n development -o wide
NAME                                   READY   STATUS    RESTARTS   AGE   IP           NODE       NOMINATED NODE   READINESS GATES
pod-info-deployment-68c8476764-6rdgv   1/1     Running   0          58m   10.244.0.5   minikube   <none>           <none>
pod-info-deployment-68c8476764-s2pwq   1/1     Running   0          58m   10.244.0.4   minikube   <none>           <none>
pod-info-deployment-68c8476764-sh2hf   1/1     Running   0          35m   10.244.0.6   minikube   <none>           <none>
```
#### Delete pod
```sh
kubectl delete pod {{pod_name}} -n {{namespace}}
```
Sample
```sh
kubectl delete pod pod-info-deployment-68c8476764-4kd56  -n development
pod "pod-info-deployment-68c8476764-4kd56" deleted
```
Even after deleting a pod, there will always be 3 pods running, as configured in **deployment.yaml**.
```sh
kubectl get pods -n development
NAME                                   READY   STATUS    RESTARTS   AGE
pod-info-deployment-68c8476764-6rdgv   1/1     Running   0          24m
pod-info-deployment-68c8476764-s2pwq   1/1     Running   0          24m
pod-info-deployment-68c8476764-sh2hf   1/1     Running   0          41s
```
The old pod gets deleted and a brand new one spins up
#### Describe pod
```sh
kubectl describe pod {{pod_name}} -n {{namespace}}
```
Sample:
```sh
kubectl describe pod pod-info-deployment-68c8476764-6rdgv -n development
Name:             pod-info-deployment-68c8476764-6rdgv
Namespace:        development
Priority:         0
Service Account:  default
Node:             minikube/10.0.2.15
Start Time:       Thu, 22 May 2025 08:19:49 -0400
Labels:           app=pod-info
                  pod-template-hash=68c8476764
Annotations:      <none>
Status:           Running
IP:               10.244.0.5
IPs:
  IP:           10.244.0.5
Controlled By:  ReplicaSet/pod-info-deployment-68c8476764
Containers:
  pod-info-container:
    Container ID:   docker://b1d0e7f6414d47797d2fceb78fb1f87ee929c53564d6f1210e3bdbfabd883390
    Image:          kimschles/pod-info-app:latest
    Image ID:       docker-pullable://kimschles/pod-info-app@sha256:fa4f33bc2301bb242bdd078ac206d0e379dfed2e225d46a6952ff444ae6f4a7a
    Port:           3000/TCP
    Host Port:      0/TCP
    State:          Running
      Started:      Thu, 22 May 2025 08:19:59 -0400
    Ready:          True
    Restart Count:  0
    Environment:
      POD_NAME:       pod-info-deployment-68c8476764-6rdgv (v1:metadata.name)
      POD_NAMESPACE:  development (v1:metadata.namespace)
      POD_IP:          (v1:status.podIP)
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-v57t9 (ro)
Conditions:
  Type                        Status
  PodReadyToStartContainers   True
  Initialized                 True
  Ready                       True
  ContainersReady             True
  PodScheduled                True
Volumes:
  kube-api-access-v57t9:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
    ConfigMapName:           kube-root-ca.crt
    ConfigMapOptional:       <nil>
    DownwardAPI:             true
QoS Class:                   BestEffort
Node-Selectors:              <none>
Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type    Reason     Age   From               Message
  ----    ------     ----  ----               -------
  Normal  Scheduled  35m   default-scheduler  Successfully assigned development/pod-info-deployment-68c8476764-6rdgv to minikube
  Normal  Pulling    35m   kubelet            Pulling image "kimschles/pod-info-app:latest"
  Normal  Pulled     35m   kubelet            Successfully pulled image "kimschles/pod-info-app:latest" in 695ms (9.047s including waiting). Image size: 213980203 bytes.
  Normal  Created    35m   kubelet            Created container: pod-info-container
  Normal  Started    35m   kubelet            Started container pod-info-container
```
#### Enter container
Sample using `busybox` container
```sh
kubectl exec -it busybox-6c747767dd-98ndz -- /bin/sh
```
#### Inspect pod logs
```sh
kubectl logs {{pod_name}} -n {{namespace}}
```
Sample:
```sh
kubectl logs pod-info-deployment-68c8476764-s2pwq -n development
undefined
Example app listening on port 3000
```
#### Get services
```sh
kubectl get services -n {{namespace}}
```
Sample
```sh
kubectl get services -n development
NAME           TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
demo-service   LoadBalancer   10.96.153.153   <pending>     80:31755/TCP   27s
```
#### Delete kubernetes objects
```sh
kubectl delete -f {{file}}
```
Sample:
```sh
kubectl delete -f busybox.yaml
deployment.apps "busybox" deleted
```
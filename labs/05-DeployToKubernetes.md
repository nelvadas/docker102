# Deploy to Kubernetes 

1. Prepare a k8s Cluster
 
    Make sure you have a k8s cluster up and running 
the following options can be used
* Katacoda <https://www.katacoda.com/courses/kubernetes/>
* Docker Desktop <https://www.docker.com/products/docker-desktop>
* Kind ( Kubernetes in Docker ) <https://kind.sigs.k8s.io/docs/user/quick-start/>
* Minikube <https://kubernetes.io/docs/setup/learning-environment/minikube/>

The next part of the lab is done with DockerDesktop

Run the `cluster-info` command to check the cluster status

```
$kubectl cluster-info
Kubernetes master is running at https://127.0.0.1:6443
KubeDNS is running at https://127.0.0.1:6443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

```

2. Deploy The greeting service on your cluster

* create a working namespace
```
$ kubectl create ns greeting
namespace/greeting created
```

* Deploy 2 replicas of the greeting service 
```
$kubectl run greeting --image=nelvadas/greeting:2.0.0 --replicas=2 -n greeting
deployment.apps/greeting created
``` 
* check the running pods
```
kubectl get po -n greeting
NAME                       READY   STATUS    RESTARTS   AGE
greeting-6774bf9bb-8t4kj   1/1     Running   0          19s
greeting-6774bf9bb-b4gn5   1/1     Running   0          19s
```

* Expose the greeting service 
```
kubectl expose deploy/greeting --type=NodePort --port=8000 --target-port=8080
service/greeting exposed
```

* Check  the service  configuration
```
kubectl get  svc greeting
NAME       TYPE       CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE
greeting   NodePort   10.107.1.156   <none>        8000:31431/TCP   25s
```

* Call the service 

```
$ curl   127.0.0.1:31431/greeting
{"id":1,"content":"Hello, World!"}
```


3. More information
 * An introduction to kubernetes  <https://www.digitalocean.com/community/tutorials/an-introduction-to-kubernetes>
 * Kubernetes official docs <https://kubernetes.io>


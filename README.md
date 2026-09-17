# Build a Docker Image with ssh & create a kubernetes cluster
```
docker build -f Dockerfile.ssh-node -t ssh-node:v1.37.0 .

kind create cluster --config kind-3node.yaml
```
```
kubectl run nginx --image=nginx // install nginx
kubectl get pods -o wide

node$ crictl ps

ssh -p 2223 root@localhost // ssh to pod or control plane change the port id assigned in yaml file
```

# Install memcached
```
kubectl run coolcache --image=memcached
kubectl get pods
kubectl get pods -o yaml
kubectl describe pod coolcache
```
# Run hello world pod on cluster
```
kubectl run nginx --image nginx
kubectl get pod nginx -o yaml
k delete pod nginx // delete pod

kubectl create -f pod.yaml // create pod with manifest file

kubectl exec colorful -- ls -alh /bin
kubectl exec colorful -- dogecoind -daemon //should fail as dogecoind is not in pod image
kubectl exec -it colorful -- bash
```

# Port Forward
```
kubectl port-forward colorful 12345:80
curl localhost:12345

```

# Deployment
```
kubectl apply -f resources.yaml
kubectl get deploy frontend
NAME       READY   UP-TO-DATE   AVAILABLE   AGE
frontend   1/1     1            1           7m25s

Change replicas to 3
kubectl apply -f resources.yaml

kmohan@A02905 deployment % kubectl get deploy frontend    
NAME       READY   UP-TO-DATE   AVAILABLE   AGE
frontend   3/3     3            3           9m8s

kubectl get pods -l app=frontend -w // to watch on separate terminal

```
# Services
```
kubectl apply -f service.yaml
It will have configuration for
ports:
  - port: 80
    targetPort: 5000

Now any external pod requesting request at port 80 will be forwarded to 5000 to all underlying pod

kubectl get pods -o wide

NAME                       READY   STATUS    RESTARTS   AGE     IP                                 
colorful                   1/1     Running   0          3h39m   10.244.1.2   
coolcache                  1/1     Running   0          3h45m   10.244.5.3   
frontend-7bb45c46d-7rv47   1/1     Running   0          168m    10.244.1.3   
frontend-7bb45c46d-g67kg   1/1     Running   0          177m    10.244.2.4  
frontend-7bb45c46d-rnwx6   1/1     Running   0          168m    10.244.5.4  
nginx                      1/1     Running   0          3h49m   10.244.2.3  

create a new pod
kubectl run -it --rm --image=superorbital/toolbox shell

kubectl get pods -o wide
...
shell                      1/1     Running   0          74s     10.244.5.5   krishna-cluster-worker2   <none>           <none> ----> New Pod

Once in the pod
$ curl 10.244.1.2
{"color":"UNKNOWN.  Please set the $COLOR environment variable."}

So curl from shell pod at 80 is forward to colorful pod.
```

# Load Balancer
```
change spec to type: LoadBalancer
Ensure you have running 
sudo cloud-provider-kind /* process for LoadBalancer service to get external IP address */

Test Load Balancer
kubectl get svc frontend
kubectl get endpoints frontend 

kubectl run -it --rm --image=superorbital/toolbox bash

curl http://load-balancer-external-ip-address/

```

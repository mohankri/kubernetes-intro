kubectl run nginx --image=nginx // install nginx
kubectl get pods -o wide

node$ crictl ps

ssh -p 2223 root@localhost // ssh to pod or control plane change the port id assigned in yaml file

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


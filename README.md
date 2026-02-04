# Kubernetes Network Policies 📝

Network Policies is a Kubernetes security feature that controls both egress and ingress traffic to workloads, reducing attack surface by only using necessary ports to allow traffic. Once a network policy selects a pod, it will implicitly deny all network traffic. Its important to allow egress to your control planes coredns pod in your -n kube-system for DNS resolution, or else DNS will break. 

This production environment contains two namespaces with resource quotas, each with their own deployment and network policy to control traffic 

TCP 80 will allow you run curl commands for internal service discovery and service to service communication within your cluster network
TCP 443 allows traffic over HTTPS into an external APIs

# Apply Directories 📂

```bash
kubectl apply -f Namespaces
``` 

```bash
kubectl apply ResourceQuotas
```

```bash 
kubectl apply -f Deployments
```

```bash 
kubectl apply -f Policies
```

# Verify network policy for -n dev

```bash 
kubectl exec -it <pod-name> -n dev -- sh
``` 

```bash
curl nginx-dev.dev.svc.cluster.local:80
```
TCP 80 (HTTP)✅  should return a html

# Verify network policy for -n aurora-database

curl commands will hang since egress is denied on TCP 80

To test DNS configuration 

```bash 
kubectl exec -it <pod-name> -n aurora-database -- sh
``` 
from inside your pod 

```bash
cat /etc/resolv.conf
```

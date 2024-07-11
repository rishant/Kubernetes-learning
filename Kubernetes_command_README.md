```
Commands:
	1. Namespace
	2. KubeConfig
	3. Node
	4. Workloads [Pods, ReplicationController, ReplicaSet, Deployment]
	5. Service [NodePort, ClusterIP, LoadBalancer, ExternalName]
	6. Network []
	7. Storage [PersistentVolume, PersistentVolumeClaim, StorageClass]
	8. Configuration [ConfigMap, Secrets]
```	
	
# Important:
	terminal:~$ kubectl config
	terminal:~$ kubectl set
	terminal:~$ kubectl create namespace <namespace-name>
	terminal:~$ kubectl rollout history|undo
	
	
# kubernetes cli client - Return all Running resources
	terminal:~$ kubectl get all
	terminal:~$ kubectl get all --namespace=default
	terminal:~$ kubectl get all -n=default


# kubernetes cli client - Command Explaination | Help
	terminal:~$ kubectl explain pod
	terminal:~$ kubectl explain rc
	terminal:~$ kubectl explain ReplicationController

# kubernetes cli client - Trubleshoot | Edit Pod
	terminal:~$ kubectl edit pod <pod-name>

# kubernetes cli client - Details Description Pod
	terminal:~$ kubectl describe pod <pod-name>
	
# kubernetes cli client - Generate Yaml file [ Imperative command ]
	terminal:~$ kubectl run nginx --image=nginx --dry-run=client -o yaml
	
	## kubernetes cli client - Generate Yaml file [ Imperative command ] - Save Yaml output into file
	terminal:~$ kubectl run nginx --image=nginx --dry-run=client -o yaml > nginx_pod.yaml

# Go-InSide Conatiner of a Pods
	terminal:~$ kubectl exec <pod-name> -c <container-name> -it <linux-command> 
	
	## If only signle Conatiner
	terminal:~$ kubectl exec <pod-name> -it <linux-command> 

# View Conatiner logs of a Pods
	terminal:~$ kubectl --kubeconfig=kubernetes\uls-stg01.k8s.yaml logs <container-name> -f
	terminal:~$ kubectl logs <container-name> -f

# View Pods 
	terminal:~$ kubectl get pods
	terminal:~$ kubectl get pods | grep 'far'
	terminal:~$ kubectl get pods nginx-Pod --show-labels
	terminal:~$ kubectl get pods -o wide
	terminal:~$ kubectl --kubeconfig=kubernetes\uls-stg01.k8s.yaml get pods


# Set CONTEXT `kubectl` kubernetes cli client
	terminal:~$ kubectl --kubeconfig=kubernetes\uls-stg01.k8s.yaml config set-context --current --namespace=fpe-tools-stg

# Validate is `deployment.yaml` | `service.yaml` file currect.
	terminal:~$ kubectl --kubeconfig=kubernetes\uls-stg01.k8s.yaml apply -f deployment.yaml
	terminal:~$ kubectl apply -f deployment.yaml --dry-run
	terminal:~$ kubectl apply -f service.yaml --dry-run
	terminal:~$ kubectl apply -f deployment.yaml --server-dry-run
	terminal:~$ kubectl apply -f service.yaml --server-dry-run
	
	terminal:~$ kubectl apply -f deployment.yaml --dry-run=client

# Validate is `deployment.yaml` | `service.yaml` file currect.
	terminal:~$ kubectl --kubeconfig=kubernetes\uls-stg01.k8s.yaml apply -f deployment.yaml
	terminal:~$ kubectl apply -f deployment.yaml
	terminal:~$ kubectl apply -f service.yaml


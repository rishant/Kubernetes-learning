"# Kubernetes-learning" 

### References:

```
    https://kubernetes.io/docs/reference/kubectl/quick-reference/	
    https://spacelift.io/blog/kubernetes-cheat-sheet

    https://www.youtube.com/watch?v=_f9ql2Y5Xcc&list=PLl4APkPHzsUUOkOv3i62UidrLmSB8DcGC&index=8
    https://github.com/piyushsachdeva/CKA-2024/tree/main

    https://www.youtube.com/watch?v=yVLXIydlU_0&list=PLl4APkPHzsUUOkOv3i62UidrLmSB8DcGC&index=11
```

## Kubernetes RoadMap

```
Kubernetes
├── Cluster
│   ├── Node
│   │   ├── Master Node
│   │   │   ├── API Server
│   │   │   ├── Controller Manager
│   │   │   ├── Scheduler
│   │   │   └── etcd
│   │   ├── Worker Node
│   │       ├── Kubelet
│   │       ├── Kube-Proxy
│   │       └── Container Runtime
│   └── Namespace				[Isolation - namespace is a way to divide cluster resources between multiple users or applications]
├── Workloads
│   ├── Pod						[Deploy Image]
│   ├── ReplicationController	[Auto Scale (Container Replicas), Auto-Healing (Fault-tolarance spawn), Not-applied-with-existing-pods]
│   ├── ReplicaSet				[Auto Scale (Container Replicas), Auto-Healing (Fault-tolarance spawn), Applied-with-existing-pods]
│   ├── Deployment  			[Stateless Microservices application] (Rolling Updates | Rollbacks | Blue GreenCanary )
│   ├── StatefulSet 			[Statelful Database application]
│   ├── DaemonSet				[Logging, Monitoring]
│   ├── Job
│   └── CronJob
├── Services & Networking
│   ├── Service
│   │   ├── ClusterIP			[Inter communication - Expose Cluster IP --> to talk Inter Container communication --> example: Front App Container talk to Backend Application Container]
│   │   ├── NodePort	    	[External Communication - Expose Node Port --> to access publicly deployed --> Frontend Web Application --> via Browser]
│   │   ├── LoadBalancer
│   │   └── ExternalName
│   ├── Ingress
│   ├── NetworkPolicy
│   └── DNS
├── Storage
│   ├── Volume
│   │   ├── emptyDir
│   │   ├── hostPath
│   │   ├── nfs
│   │   ├── awsElasticBlockStore
│   │   ├── gcePersistentDisk
│   │   ├── azureDisk
│   │   ├── azureFile
│   │   ├── cephFS
│   │   ├── iscsi
│   │   ├── local
│   │   └── csi
│   ├── PersistentVolume
│   ├── PersistentVolumeClaim
│   └── StorageClass
├── Configuration
│   ├── ConfigMap
│   ├── Secret
│   ├── ResourceQuota
│   ├── LimitRange
│   ├── HorizontalPodAutoscaler
│   └── PodDisruptionBudget
├── Security
│   ├── ServiceAccount
│   ├── Role
│   ├── RoleBinding
│   ├── ClusterRole
│   ├── ClusterRoleBinding
│   └── PodSecurityPolicy
└── Extensions & Add-ons
    ├── Helm
    ├── Operators
    ├── CustomResourceDefinition (CRD)
    ├── Prometheus
    ├── Grafana
    ├── Fluentd
    ├── Istio
    ├── Linkerd
    └── Metrics Server
```



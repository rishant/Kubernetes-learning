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


## `Configmap`

Summary of ConfigMap Usage in Kubernetes:
| Method	| Use Case |
| ------------ | ----------|
| Environment Variables	| Store app configurations and inject them into the container. |
| Mounted as Files in a Volume |	Share configuration files with the container. |
| Command-Line Arguments |	Pass ConfigMap values as container arguments. |
| Entire Volume	| Mount all ConfigMap keys as individual files. |
| Other Kubernetes Resources |	Use in Deployments, StatefulSets, etc., for scaling purposes. |


## 1. ConfigMap - Environment Variables:

```
---configmap_env_variables.yaml---
  apiVersion: v1
  kind: ConfigMap
  metadata:
    name: app-config
  data:
    APP_NAME: "MyApp"
    APP_VERSION: "1.0"
----------------------------------

cmd:/> kubectl apply -f configmap_env_variables.yaml

--------deployment.yml----------
  apiVersion: v1
  kind: Pod
  metadata:
    name: app-pod
  spec:
    containers:
      - name: app-container
        image: nginx
        env:
          - name: APP_NAME
            valueFrom:
              configMapKeyRef:
                name: app-config
                key: APP_NAME
          - name: APP_VERSION
            valueFrom:
              configMapKeyRef:
                name: app-config
                key: APP_VERSION
----------------------------------------

```

## 2. Mounted as Files in a Volume

```
-----configmap_mount_as_file.yaml-----
  apiVersion: v1
  kind: ConfigMap
  metadata:
  name: app-config
  data:
    config.json: |
      {
      "app_name": "MyApp",
      "app_version": "1.0"
      }
--------------------------------------

cmd:/> kubectl apply -f configmap_mount_as_file.yaml

-------deploymnet.yaml-------
  apiVersion: v1
  kind: Pod
  metadata:
  name: app-pod
  spec:
  containers:
    - name: app-container
      image: nginx
      volumeMounts:
        - name: config-volume
      mountPath: /etc/config
  volumes:
    - name: config-volume
      configMap:
        name: app-config
---------------------------------------

Note: 
1. This will mount the config.json file into the container at /etc/config/config.json.
```

## 3. Command-Line Arguments

```
-------ConfigMap_cmdline.yaml--------
  apiVersion: v1
  kind: ConfigMap
  metadata:
  name: app-config
  data:
    greeting: "Hello, World!"
--------------------------------------

cmd:/> kubectl apply -f ConfigMap_cmdline.yaml    

-------deploymnet.yaml-------
  apiVersion: v1
  kind: Pod
  metadata:
  name: app-pod
  spec:
  containers:
    - name: app-container
      image: busybox
      command: ["echo"]
      args:
        - "$(GREETING)"
      env:
        - name: GREETING
      valueFrom:
        configMapKeyRef:
        name: app-config
        key: greeting
-------------------------------
```

## 4. ConfigMap as an Entire Volume

```
-------ConfigMap_entire_volume.yaml--------
  apiVersion: v1
  kind: ConfigMap
  metadata:
  name: app-config
  data:
    app.properties: |
      app.name=MyApp
      app.version=1.0
    db.properties: |
      db.name=MyDB
      db.version=2.0
--------------------------------------

cmd:/> kubectl apply -f ConfigMap_entire_volume.yaml   

-------deployment.yaml-----------------
  apiVersion: v1
  kind: Pod
  metadata:
    name: app-pod
  spec:
    containers:
      - name: app-container
        image: nginx
        volumeMounts:
          - name: config-volume
            mountPath: /etc/config
    volumes:
      - name: config-volume
        configMap:
          name: app-config
---------------------------------------

Notes:
    This will create two files inside the container:
        /etc/config/app.properties
        /etc/config/db.properties
```

## 5. Used by Another Kubernetes Resource

```
-------ConfigMap_another_k8s_resource.yaml--------
  apiVersion: v1
  kind: ConfigMap
  metadata:
    name: app-config
  data:
    APP_NAME: "MyApp"
---------------------------------------------------

cmd:/> kubectl apply -f ConfigMap_another_k8s_resource.yaml  

-------deployment.yaml-----------------
  apiVersion: apps/v1
  kind: Deployment
  metadata:
    name: app-deployment
  spec:
    replicas: 2
    selector:
      matchLabels:
        app: my-app
    template:
      metadata:
        labels:
          app: my-app
      spec:
        containers:
          - name: app-container
            image: nginx
            env:
              - name: APP_NAME
                valueFrom:
                  configMapKeyRef:
                    name: app-config
                    key: APP_NAME
-----------------------------------------

```
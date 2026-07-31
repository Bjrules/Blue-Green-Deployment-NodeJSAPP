## Create Service Account, Role, ClusterRole & Assign that role, And create a secret for Service Account and genrate a Token

### Creating Service Account


```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: jenkins
  namespace: webapps
```

### Create Role 


```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: app-role
  namespace: webapps
rules:
  - apiGroups:
        - ""
        - apps
        - autoscaling
        - batch
        - extensions
        - policy
        - rbac.authorization.k8s.io
    resources:
      - pods
      - secrets
      - componentstatuses
      - configmaps
      - daemonsets
      - deployments
      - events
      - endpoints
      - horizontalpodautoscalers
      - ingress
      - jobs
      - limitranges
      - namespaces
      - nodes
      - pods
      - persistentvolumes
      - persistentvolumeclaims
      - resourcequotas
      - replicasets
      - replicationcontrollers
      - serviceaccounts
      - services
    verbs: ["get", "list", "watch", "create", "update", "patch", "delete"]
```

### Bind the role to service account


```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: app-rolebinding
  namespace: webapps 
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: Role
  name: app-role 
subjects:
- namespace: webapps 
  kind: ServiceAccount
  name: jenkins 
```


### Create a ClusterRole for PV access

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: persistent-volume-access
rules:
  - apiGroups: [""]
    resources: ["persistentvolumes"]
    verbs: ["get", "list", "watch", "create", "update", "patch", "delete"]

```

### Bind the clusterRole to Jenkins Service Account

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: jenkins-persistent-volume-access
subjects:
  - kind: ServiceAccount
    name: jenkins
    namespace: webapps
roleRef:
  kind: ClusterRole
  name: persistent-volume-access
  apiGroup: rbac.authorization.k8s.io

```



### Generate token using service account in the namespace
[Create Token](https://kubernetes.io/docs/concepts/configuration/secret/)

```
kubectl apply -f secret-file.yaml -n webapps
```

```yaml

apiVersion: v1
kind: Secret
metadata:
  name: sa-secret
  annotations:
    kubernetes.io/service-account.name: "jenkins"
type: kubernetes.io/service-account-token

```
#### kindly get the token using this command
```
kubectl describe secret sa-secret -n webapps

```

**copy the token as it will be used for Jenkins-Kubernetes Authentication `secret text` K8s-token later.kindly refer to the Jenkinsfile of this Repo**
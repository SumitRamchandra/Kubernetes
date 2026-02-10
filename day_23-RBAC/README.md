# Commands used in the demo:

Generate the csr key and file

openssl genrsa -out krishna.key 2048
openssl req -new -key krishna.key -out krishna.csr -subj "/CN=krishna"

to check who you are:

kubectl auth whoami

to check if you have access to a particular resource

k auth can-i create po
k auth can-i create po --as krishna

To get the crt file in decoded format
kubectl get csr krishna -o jsonpath='{.status.certificate}'| base64 -d > krishna.crt   ---This needs to keep in mind 

# Sample YAML for role

apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: default
  name: pod-reader
rules:
- apiGroups: [""] # "" indicates the core API group
  resources: ["pods"]
  verbs: ["get", "watch", "list"]


# Sample YAML for role binding

apiVersion: rbac.authorization.k8s.io/v1
# This role binding allows "jane" to read pods in the "default" namespace.
# You need to already have a Role named "pod-reader" in that namespace.
kind: RoleBinding
metadata:
  name: read-pods
  namespace: default
subjects:
# You can specify more than one "subject"
- kind: User
  name: krishna # "name" is case sensitive
  apiGroup: rbac.authorization.k8s.io
roleRef:
  # "roleRef" specifies the binding to a Role / ClusterRole
  kind: Role #this must be Role or ClusterRole
  name: pod-reader # this must match the name of the Role or ClusterRole you wish to bind to
  apiGroup: rbac.authorization.k8s.io



# Set the new user:  

kubectl config set-credentials adam \
> --client-key=adam.key \
> --client-certificate=adam.crt 
User "adam" set.

# Follow below doc link for reference:
https://kubernetes.io/docs/tasks/tls/certificate-issue-client-csr/
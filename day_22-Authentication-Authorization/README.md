# Kubernetes Authentication and Authorization

## **Authentication vs Authorization**

**Authentication** verifies who you are (identity), while **Authorization** determines what you're allowed to do (permissions).

## **Authentication in Kubernetes**

Kubernetes supports multiple authentication mechanisms:

### 1. **X.509 Client Certificates**
- Kubelet and kubectl use client certificates signed by the cluster's CA
- Each user's certificate is identified by the Common Name (CN) and Organization (O) fields

### 2. **Static Token File**
- Users defined in a CSV file with tokens
- Format: `token,username,uid,groups`
- Passed via `--token-auth-file` flag to API server

### 3. **Static Password File**
- Users defined in a CSV file with passwords
- Format: `password,username,uid,groups`
- Less secure, rarely used

### 4. **Bootstrap Tokens**
- Used by kubelets to bootstrap themselves
- Stored in the `kube-system` namespace

### 5. **Service Account Tokens**
- Used by pods running in the cluster
- JWT tokens stored in `/var/run/secrets/kubernetes.io/serviceaccount/token`

### 6. **OpenID Connect (OIDC)**
- External identity providers (Google, GitHub, Keycloak)
- Integrates with corporate SSO systems

### 7. **Webhook Token Authentication**
- Delegates authentication to external webhook services
- For custom authentication logic

### 8. **Proxy Authentication**
- Uses headers like `X-Remote-User`, `X-Remote-Group`
- For reverse proxy authentication

---

## **Authorization in Kubernetes**

Kubernetes uses authorization modules to determine if a request is allowed:

### **1. RBAC (Role-Based Access Control)** ⭐ Most Common
- **Role**: Set of permissions for a specific namespace
- **ClusterRole**: Set of permissions for the entire cluster
- **RoleBinding**: Grants Role permissions to users/groups/service accounts in a namespace
- **ClusterRoleBinding**: Grants ClusterRole permissions cluster-wide

**Example RBAC Structure:**
```yaml
---
# Define permissions
kind: Role
metadata:
  name: pod-reader
  namespace: default
rules:
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["get", "list", "watch"]
---
# Grant permissions
kind: RoleBinding
metadata:
  name: read-pods
  namespace: default
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: Role
  name: pod-reader
subjects:
- kind: User
  name: alice
  apiGroup: rbac.authorization.k8s.io
```

### **2. ABAC (Attribute-Based Access Control)**
- Policy file defines access rules based on attributes (user, resource, action)
- Less flexible than RBAC; rarely used

### **3. Webhook Authorization**
- Delegates authorization decisions to external service
- For complex custom policies

### **4. Node Authorization**
- Special authorizer for kubelet requests
- Controls what nodes can access

### **5. AlwaysAllow / AlwaysDeny**
- AlwaysAllow: Permits all requests (insecure)
- AlwaysDeny: Denies all requests

---

## **Key Concepts**

| Concept | Purpose |
|---------|---------|
| **Subjects** | Users, groups, or service accounts being granted access |
| **Resources** | API objects (pods, services, deployments, etc.) |
| **Verbs** | Actions allowed (get, list, create, delete, patch, etc.) |
| **API Groups** | Logical grouping of APIs (core "", apps, batch, etc.) |

---

## **Common RBAC Patterns**

**View pods in a namespace:**
```yaml
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["get", "list"]
```

**Manage deployments:**
```yaml
- apiGroups: ["apps"]
  resources: ["deployments"]
  verbs: ["get", "list", "create", "update", "patch", "delete"]
```

**Full cluster admin (dangerous):**
```yaml
rules:
- apiGroups: ["*"]
  resources: ["*"]
  verbs: ["*"]
```

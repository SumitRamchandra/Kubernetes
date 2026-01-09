A namespace is like a separate folder or isolated place where you can run your resources without disturbing the other resources created in other namespaces.

by default K8s has below namespaces created:

NAME                

default, 
kube-node-lease, 
kube-public,     
kube-system,         
local-path-storage

you can create namespaces by imperative or declarative way.
$ kubectl create ns <NAME>
$ kubectl delete ns <NAME>

Remember to specify the required namespace when working the resources other wise K8s will consider the resources from default namespace.
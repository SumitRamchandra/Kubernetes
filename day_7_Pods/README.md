- What is POD?

Pod is a smallest deployable unit in kubernates. Ideally we can have 1 container per pod but also can have multiple container as well like helper container or monitoring agents, etc. 
Pod in K8s are ephemeral means they can die and get replaced means their IP's keep changing.

- There are two ways of crating kubernetes object:
1. Imperative - Through command or API calls 
Ex. Create an enginx pod through imperative way
 $ kubectl run <pod-name> --image=<image-name:tag>
 $ kubectl run nginx-pod --image=nginx:latest
![alt text](image.png)


2. Declarative - by creating the manifest files.
in this method we can directly use an yaml menifest file to create resources. 
- Task details

1. Create a pod and try to schedule it manually without the scheduler.

2. Login to the control plane node and go to the directory of default static pod manifests and try to restart the control plane components

3. Create 3 pods with the name as pod1, pod2 and pod3 based on the nginx image and use labels as env:test, env:dev and env:prod for each of these pods respectively.

4. Then using the kubectl commands, filter the pods that have labels dev and prod.


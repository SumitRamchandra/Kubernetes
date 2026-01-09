- Task details: 
1. create a pod with nginx as the image and add the nodeffinity with property requiredDuringSchedulingIgnoredDuringExecution and condition disktype = ssd

2. check the status of the pod and see why it is not scheduled

3. add the label to your worker01 node as distype=ssd and then check the status of the pod

4. It should be scheduled on worker node 1

5. create a new pod with redis as the image and add the nodeaffinity with property requiredDuringSchedulingIgnoredDuringExecution and condition disktype    without any value

6. add the label to worker02 node with disktype and no value

7. ensure that pod2 should be scheduled on worker02 node
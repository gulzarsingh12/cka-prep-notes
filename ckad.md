# Deployment

## Deployment strategy

### Recreate
Kubernetes strategy to delete all pods and recreate all the pods. In this strategy, all pods will be terminated, hence application wont be available 
during deployment.

### Rolling Update
This is the default kubernetes strategy where pods are terminated and recreated at the besed on the max unavailable and max surge. For example if 
deployment has set 4 replicas and max unavail and surge is 25% then updated are rolled by terminating 1 pod first and then bring up 1 pod. Once 1 
pod is up then 2 is termibated and recreated.
This will ensure no application downtime hence default deployment strategy.

### Blue Green
In this strategy, new version version is deployed and tested. if working, then old deployment is terminated. So in this case, if 4 pods are deployed 
then 4 new pods are created totalling 8 pods. 

### Canary
In this deployment, new changes are rolled with old one and tested to ensure it is working if working fine then new changes are deployed rolled out slowely to all pods.
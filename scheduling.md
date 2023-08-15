# Manual Scheduling
Suppose there is no scheduler set up. Then pods will be stuck in pending state.To schedule the pod manually when no
scheduler exists, we can set the **nodeName** property of pod definition file. 

But this is only possible for new pods creation. What if a pod is already created and we want to schedule it. This
can be done with Binding. You can apply below yaml from kubectl or submit rest api call


## binding.yaml
````
apiVersion: v1
kind: Binding
metadata:
  name: nginx
target:
  apiVersion: v1
  kind: Node
  name: node01
````

# cli
````
k run nginx --image=nginx
k apply -f binding.yaml
````

## rest
Only binding part as below
````
PODNAME=nginx
curl --header "Content-Type:application/json" \
--request POST \
--data '{ "apiVersion": "v1", "kind": "Binding", "metadata": { "name": "nginx" }, "target": { "apiVersion": "v1", "kind": "Node", "name": "node01" }}}'
https://$SERVER/api/v1/namespaces/default/pods/$PODNAME/binding/
````
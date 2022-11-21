## UNDERSTANDING KUBERNETES

This repo is for anyone who wants to appear for CKA(https://www.cncf.io/certification/cka/) exam and wants to know the basics and practice. I have created this repo while I am preparing for the exam to help other fellow engineers like me.

The folders in the repository contain the YAML files which contain the definition of most of the k8s objects like <i>Pods, services, replication controller etc</i>.

<b>Best course out there to practice and learn k8s fundamentals and prepare for the exam: https://www.udemy.com/course/certified-kubernetes-administrator-with-practice-tests/learn </b>


### IMPORTANT EXAM RESOURCES
<b>
  
 1) K8s command Cheatsheet: https://kubernetes.io/docs/reference/kubectl/cheatsheet/
  
2) Free Online k8s playground: https://www.katacoda.com/courses/kubernetes/playground
  
3) Practice tests : https://kodekloud.com/topic/practice-tests-deployments-2/
  
4) Using k8s Imperative commands to create Objects: https://kubernetes.io/docs/tasks/manage-kubernetes-objects/imperative-command/ 

</b>



### EXAM TIPS


As you might have seen already, it is a bit difficult to create and edit YAML files. Especially in the CLI. 

During the exam, you might find it difficult to copy and paste YAML files from browser to terminal. 

Using the ``` kubectl run``` and ``` kubectl create <object>``` command can help in generating a YAML template. And sometimes, you can even get away with just the ```kubectl run``` command without having to create a YAML file at all. For example, if you were asked to create a pod or deployment with specific name and image you can simply run the ```kubectl run``` or ``` kubectl create``` command.

Use the below set of commands and try the previous practice tests again, but this time try to use the below commands instead of YAML files. Try to use these as much as you can going forward in all exercises


```--dry-run```: By default as soon as the command is run, the resource will be created. If you simply want to test your command , use the ```--dry-run=client``` option. This will not create the resource, instead, tell you whether the resource can be created and if your command is right.

```-o yaml```: This will output the resource definition in YAML format on screen.



Use the above two in combination to generate a resource definition file quickly, that you can then modify and create resources as required, instead of creating the files from scratch.

Reference (Bookmark this page for exam. It will be very handy):

```yaml
https://kubernetes.io/docs/reference/kubectl/conventions/
```


## IMPORTANT ```kubectl``` COMMANDS



### Create an NGINX Pod

```yaml
kubectl run nginx --image=nginx
```

```yaml
kubectl run --image=nginx custom-nginx --port=8080
```  

The above command creates a new pod called ```custom-nginx``` using the nginx image and exposes it on container port ```8080```.

### Generate POD Manifest YAML file (-o yaml). Don't create it(--dry-run)

```yaml
kubectl run nginx --image=nginx --dry-run=client -o yaml
```



### Create a deployment

```yaml
kubectl create deployment --image=nginx nginx
```

### Generate Deployment YAML file (-o yaml). Don't create it(--dry-run)

```yaml
kubectl create deployment --image=nginx nginx --dry-run=client -o yaml
```

### Generate Deployment YAML file (-o yaml). Don't create it(--dry-run) with 4 Replicas (--replicas=4) and write it to a file.

```yaml
kubectl create deployment --image=nginx nginx --dry-run=client -o yaml > nginx-deployment.yaml
```

### Save it to a file, make necessary changes to the file (for example, adding more replicas) and then create the deployment.

```yaml
kubectl create -f nginx-deployment.yaml
```

OR

In k8s version 1.19+, we can specify the --replicas option to create a deployment with 4 replicas.

```yaml
kubectl create deployment --image=nginx nginx --replicas=4 --dry-run=client -o yaml > nginx-deployment.yaml
```


### Creating a Service and exposing a pod


#### 1) Using ```kubectl create```


```yaml
kubectl create service nodeport <myservicename> 
```

<b>This will not use the pods labels as selectors, instead it will assume selectors as ```app=redis```. You cannot pass in selectors as an option. So it does not work very well if your pod has a different label set. So generate the file and modify the selectors before creating the service </b>



#### 2) Using ```kubectl expose```

<b>This will automatically use the pod's labels as selectors</b>

```yaml
kubectl expose pod nginx  --port=80 --target-port=8000 --dry-run=client -o yaml 
```

OR

  ```yaml
  kubectl expose pod httpd --target-port=80 --port=80 -o yaml > service1.yaml
  ```

  then 

  ```yaml
  
  kubectl create -f service1.yaml

```


### EXAMPLES

Create a Service named redis-service of type ClusterIP to expose pod redis on port 6379

```yaml

kubectl expose pod redis --port=6379 --name redis-service --dry-run=client -o yaml

```


Or

```yaml
kubectl create service clusterip redis --tcp=6379:6379 --dry-run=client -o yaml 

```

Create a Service named nginx of type NodePort to expose pod nginx's port 80 on port 30080 on the nodes:

```yaml

kubectl expose pod nginx --type=NodePort --port=80 --name=nginx-service --dry-run=client -o yaml
```
(This will automatically use the pod's labels as selectors, but you cannot specify the node port. You have to generate a definition file and then add the node port in manually before creating the service with the pod.)

Or

```yaml
kubectl create service nodeport nginx --tcp=80:80 --node-port=30080 --dry-run=client -o yaml
```
(This will not use the pods labels as selectors)

Both the above commands have their own challenges. While one of it cannot accept a selector the other cannot accept a node port. I would recommend going with the ```kubectl expose``` command. If you need to specify a node port, generate a definition file using the same command and manually input the nodeport before creating the service.



### Selectors and Labels

Lables are properties assigned to each item.  Selectors helps us filter these items.









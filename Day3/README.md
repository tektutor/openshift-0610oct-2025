# Day 3

## Info - Container Images imported in Server1 and Server2
```
oc get imagestreams -n openshift | grep nginx
oc get imagestream -n openshift | grep spring-ms
oc get is -n openshift | grep wordpress
oc get is -n openshift | grep mariadb
```

## Lab - Finding Pod IP address and node where the Pod is running
```
oc project jegan
oc get pods -o wide
```

## Lab - Pod port-forwarding for quick developer testing purpose ( Not recommended for production )
```
oc project jegan
oc get pods
oc port-forward nginx-7b58f48fbb-5vh56 9999:8080
```
In the above command, the port 9999 is on the local machine and port 8080 is where nginx is listening inside the pod container. Hence, you need to choose some non-conflicting port in the place of 9999.

You can access the web page from your web browser
```
http://localhost:9999
```
<img width="1920" height="1168" alt="image" src="https://github.com/user-attachments/assets/3aae1e04-1e25-476e-9c31-cb14dbaac6dc" />
<img width="1920" height="1168" alt="image" src="https://github.com/user-attachments/assets/c0fc251a-06d2-4fcd-a0aa-fef5563bc89b" />

## Info - Kubernetes/Openshift Service
<pre>
- Service represents a group of load-balanced Pods from a single deployment
- Service has a unique name and stable IP address
- Hence, applications must be using service name to access one of the Pod endpoint
- While the pods behind the service can be replaced by other pods, due to scale up/down, we can use the stable service IP or Service name
- When the service is accessed via its name, it is called service discovery
- Kubernetes/Openshift supports 3 types of services
  1. ClusterIP Internal Service
  2. NodePort External service
  3. LoadBalancer External service
</pre>

## Lab - Let's deploy two applications under your project
```
oc project jegan
oc create deployment nginx --image=image-registry.openshift-image-registry.svc:5000/openshift/nginx:1.0 --replicas=3
oc create deployment hello --image=image-registry.openshift-image-registry.svc:5000/openshift/spring-ms:latest --replicas=3

oc get pods
```

Let's create an internal service for nginx deployment
```
oc expose deploy/nginx --type=ClusterIP --port=8080
```

Listing the services
```
oc get services
oc get service
oc get svc
```

If you wish to find more information about the service you create
```
oc describe svc/nginx
```


## Lab - Finding the containers in a Pod
```
oc get pod ovnkube-node-99dfk -n openshift-ovn-kubernetes -o jsonpath='{range .spec.containers[*]}{.name}{"\n"}{end}'
```

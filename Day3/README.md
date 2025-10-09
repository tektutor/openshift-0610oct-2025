# Day 3

## Info - Container Images imported in Server1 and Server2
```
oc get imagestreams -n openshift | grep nginx
oc get imagestream -n openshift | grep spring-ms
oc get is -n openshift | grep wordpress
oc get is -n openshift | grep mariadb
```

## Info - For detailed instructions on how to install and configure Metallb operator in Openshift
<pre>
https://medium.com/tektutor/using-metallb-loadbalancer-with-bare-metal-openshift-onprem-4230944bfa35  
</pre>

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

In order to access the nginx clusterip internal service, let's get inside the hello pod shell
```
oc rsh deploy/hello
curl http://nginx:8080
cat /etc/resolv.conf
```
In the above command
<pre>
nginx - is the name of the clusterip service
8080 - is the service port
</pre>

How service discovery works
<pre>
- When Pod containers are created by kubelet, kubelt configures the /etc/resolv.conf file with the DNS Service IP 
- when we access http://nginx:8080 url, this request will be forwarded to the DNS server configured in the /etc/resolv.conf
- The DNS Server IP we see in the /etc/resolv.conf is the DNS Service IP, which means behind that DNS Service there will 6 load-balanced DNS Pods
- on each node one DNS Pod runs, that will resolve the nginx service name to its corresponding service IP
- this is the way  the request will locate the service, the service has a selector label something like app=nginx
- the kube-proxy pod that runs in every node picks this selector label app=nginx and retrieves the endpoints
  oc get endpoints -l app=nginx
- as per the load-balancing algorithm configured in the kube-proxy it routes the call to one of the Pod endpoint listed in the endpoints string
</pre>


In order to expose the application for external access with a public url, you could create a route
```
oc expose svc/nginx
oc get routes
oc get route
```

From your lab machine web browser, you may access at the url
```
curl http://nginx-jegan.apps.ocp4.palmeto.org
```

Once you are done with this exercise, you may delete the resources created in your project
```
oc project jegan
oc delete deploy/nginx svc/nginx route/nginx
```

## Lab - Editing resources
```
oc project jegan
oc edit deploy/nginx
oc edit rs/nginx-aabbcc
oc edit pod/nginx-aabbcc-xxyyzz

oc get deploy/nginx -o json
oc get deploy/nginx -o yaml

oc get rs/nginx-aabbcc -o json
oc get rs/nginx-aabbcc -o yaml

oc get pod/nginx-aabbcc-xxyyzz -o json
oc get rs/nginx-aabbcc-xxyyzz -o yaml
```

## Lab - Deploying nginx in declarative style
```
oc create deployment nginx --image=image-registry.openshift-image-registry.svc:5000/openshift/nginx:1.0 --replicas=3 --dry-run=client -o yaml > nginx-deploy.yml
oc create -f nginx-deploy.yml --save-config
oc get deploy,rs,po

# Update the nginx-deploy.yml replace replicas count from 3 to 5, save and apply
oc apply -f nginx-deploy.yml
oc get pods

# Delete the nginx deploy
oc delete -f nginx-deploy.yml
oc get deploy,rs,pods
```

## Lab - Finding the containers in a Pod
```
oc get pod nginx-7b58f48fbb-qr8kw -o jsonpath='{range .spec.containers[*]}{.name}{"\n"}{end}'
```

## Lab - Creating clusterip internal service in declarative style
Make sure you have your nginx deployment created
```
oc apply -f nginx-deploy.yml
```

Generate the nginx clusterip service declarative manifest yaml file
```
oc expose deploy/nginx --type=ClusterIP --port=8080 --dry-run=client -o yaml
# Generate the declarative clusterip internal service
oc expose deploy/nginx --type=ClusterIP --port=8080 --dry-run=client -o yaml > nginx-clusterip-svc.yml

#Create the clusterip internal service declaratively
oc apply -f nginx-clusterip-svc.yml

# Generate the declarative route manifest script
oc expose svc/nginx --dry-run=client -o yaml > nginx-route.yml
oc apply -f nginx-route.yml
oc get route

# Test the route
curl http://nginx-jegan.apps.ocp4.palmeto.org
```

## Lab - Generate nodeport external service declarative script and deploy it
```
oc apply -f nginx-deploy.yml

oc expose deploy/nginx --type=NodePort --port=8080 --dry-run=client -o yaml
oc expose deploy/nginx --type=NodePort --port=8080 --dry-run=client -o yaml > nginx-nodeport-svc.yml

# Make sure you have deleted the nginx clusterip service
oc delete -f nginx-clusterip-svc.yml

# Create the nodeport service declaratively
oc apply -f nginx-nodeport-svc.yml

# list the nodeport svc
oc get svc

# Test the nodeport external service
oc get nodes -o wide
curl http://<any-node-ip>:<node-port>
curl http://192.168.100.11:30829
```

Things to note about nodeport services
<pre>
- they are not user-friendly, assume if facebook says you need access facebook website like http://www.facebook.com:30829
- for each node-port service, a port like 30829 will be opened in every node in the openshift/kubernetes cluster, this leads to security issue.
- assume, you have about 100 microservices, for which you have created 100 nodeport services, which means you need to open up 100 ports in all the nodes, the more port we open up in firewall it is more vulnerable, hence it is not recommended
- in openshift the best alternate is creating a clusterip service and expose it via route for external access
</pre>

## Info - LoadBalancer service
<pre>
- this type of service is generally used in public cloud Openshift/Kubernetes setup
- when a LoadBalancer type of service is created in aws eks or azure aks, it will spin-up one external loadbalancer like aws elb/alb
- in case you don't prefer the in-built load-balancing supported by Kubernetes/Openshift kube-proxy you could consider using LoadBalancer service in public cloud environments
- in case you wish to use this loadbalancer service in on-prem openshift/kubernetes cluster, you should install Metallb operator and configure it in order to get support for LoadBalancer in a local Kubernetes/Openshift cluster like our training environment
</pre>

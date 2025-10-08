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

You can access the web page from your web browser
```
http://localhost:9999
```
<img width="1920" height="1168" alt="image" src="https://github.com/user-attachments/assets/3aae1e04-1e25-476e-9c31-cb14dbaac6dc" />
<img width="1920" height="1168" alt="image" src="https://github.com/user-attachments/assets/c0fc251a-06d2-4fcd-a0aa-fef5563bc89b" />


## Lab - Finding the containers in a Pod
```
oc get pod ovnkube-node-99dfk -n openshift-ovn-kubernetes -o jsonpath='{range .spec.containers[*]}{.name}{"\n"}{end}'
```

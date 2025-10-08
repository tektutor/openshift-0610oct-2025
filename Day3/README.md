# Day 3

## Info - Container Images imported in Server1 and Server2
```
oc get imagestreams -n openshift | grep nginx
oc get imagestream -n openshift | grep spring-ms
oc get is -n openshift | grep wordpress
oc get is -n openshift | grep mariadb
```

## Lab - Finding the containers in a Pod
```
oc get pod ovnkube-node-99dfk -n openshift-ovn-kubernetes -o jsonpath='{range .spec.containers[*]}{.name}{"\n"}{end}'
```

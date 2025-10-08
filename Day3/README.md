# Day 3

## Lab - Finding the containers in a Pod
```
oc get pod ovnkube-node-99dfk -n openshift-ovn-kubernetes -o jsonpath='{range .spec.containers[*]}{.name}{"\n"}{end}'
```

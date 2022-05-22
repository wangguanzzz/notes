## 56 autoscaling
horizonal pod autoscaler (HPA), a k8s facility that adjust replica of pod to matchi observed average CPU utilization
```
$ kubectl autoscale deployment wordpress --cpu-percent=50 --min=1 --max=10
```

example to generate load
```
$ kubectl run -i --tty load-generator --image=busybox --generator=run-pod/v1 /bin/sh (-i interactive --tty  => -it)
#hit enter
$ while true; do wget -q -O http://wordpress.default.svc.cluster.local; done

```
to view the status of HPA
```
kubectl get hpa
```

## 58 Auditing
k8s auditing provides a security-relevant chronological set of records documenting the sequence of activities that have affected the system
- audit policy
- audit storage ( logs or web hooks)

a audit-policy.yaml is started with K8S

### jq command  
```
cat log.json | jq .
```

## 62 high availability
* master
  - kube-apiserver
  - kube-controller-manager
  - kube-scheduler
* etcd
1. create reliable nodes that , together, will form an highly available master implementation in following steps
2. setting up a redundant, reliable data storage layer with clustered etcd
3. starting replicated, load balanced API servers (kube-api)
4. setting up master-elected kube-scheduler and kube-controller-manager daemons
5. multiple worker nodes
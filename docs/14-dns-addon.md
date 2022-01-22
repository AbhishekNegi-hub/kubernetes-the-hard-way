# Deploying the DNS Cluster Add-on

In this lab you will deploy the [DNS add-on](https://kubernetes.io/docs/concepts/services-networking/dns-pod-service/) which provides DNS based service discovery, backed by [CoreDNS](https://coredns.io/), to applications running inside the Kubernetes cluster.

## The DNS Cluster Add-on

Deploy the `coredns` cluster add-on:

```
kubectl apply -f https://raw.githubusercontent.com/AbhishekNegi-hub/kubernetes-the-hard-way/master/deployments/coredns.yaml
```

> output

```
serviceaccount/coredns created
clusterrole.rbac.authorization.k8s.io/system:coredns created
clusterrolebinding.rbac.authorization.k8s.io/system:coredns created
configmap/coredns created
deployment.extensions/coredns created
service/kube-dns created
```

List the pods created by the `kube-dns` deployment:

```
kubectl get pods -l k8s-app=kube-dns -n kube-system
```

> output

```
NAME                       READY   STATUS    RESTARTS   AGE
coredns-699f8ddd77-94qv9   1/1     Running   0          20s
coredns-699f8ddd77-gtcgb   1/1     Running   0          20s
```

Reference: https://kubernetes.io/docs/tasks/administer-cluster/coredns/#installing-coredns

## Verification

Create a `dnsutils` deployment:

```
kubectl apply -f https://k8s.io/examples/admin/dns/dnsutils.yaml
```

List the pod created by the `dnsutils` deployment:

```
kubectl get pods dnsutils
```

> output

```
NAME       READY   STATUS    RESTARTS   AGE
dnsutils   1/1     Running   0          55m
```

Execute a DNS lookup for the `kubernetes` service inside the `dnsutils` pod:

```
kubectl exec dnsutils -- nslookup kubernetes
```

> output

```
Server:    10.96.0.10
Address 1: 10.96.0.10 kube-dns.kube-system.svc.cluster.local

Name:      kubernetes
Address 1: 10.96.0.1 kubernetes.default.svc.cluster.local
```

Next: [Smoke Test](15-smoke-test.md)

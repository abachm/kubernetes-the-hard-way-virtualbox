# Provisioning Pod Network

Container Network Interface (CNI) is a standard interface for managing IP networks between containers across many nodes.

We chose to use CNI - [weave](https://www.weave.works/docs/net/latest/kubernetes/kube-addon/) as our networking option.


### Deploy Weave Network

Deploy weave network. Run only once on my local host. You will see a warning, but this is OK.

[//]: # (host:master-1)

On local linux box

```bash
kubectl apply -f "https://github.com/weaveworks/weave/releases/download/v2.8.1/weave-daemonset-k8s-1.11.yaml"
```

Weave uses POD CIDR of `10.32.0.0/12` by default.

## Verification

[//]: # (command:kubectl rollout status daemonset weave-net -n kube-system --timeout=90s)

List the registered Kubernetes nodes from the master node:

```bash
kubectl get pods -n kube-system
```

> output

```
NAME              READY   STATUS    RESTARTS      AGE
weave-net-7ptsp   2/2     Running   1 (74s ago)   81s
weave-net-b8dvj   2/2     Running   1 (74s ago)   81s
weave-net-vfwrd   2/2     Running   1 (73s ago)   81s
```

Once the Weave pods are fully running which might take up to 60 seconds, the nodes should be ready

```bash
kubectl get nodes
```

> Output

```
NAME       STATUS   ROLES    AGE   VERSION
worker-1   Ready    <none>   18h   v1.28.1
worker-2   Ready    <none>   33m   v1.28.1
worker-3   Ready    <none>   33m   v1.28.1
```

Reference: https://kubernetes.io/docs/tasks/administer-cluster/network-policy-provider/weave-network-policy/#install-the-weave-net-addon

Prev: [Configuring Kubectl](12-configuring-kubectl.md)</br>
Next: [Kube API Server to Kubelet Connectivity](14-kube-apiserver-to-kubelet.md)

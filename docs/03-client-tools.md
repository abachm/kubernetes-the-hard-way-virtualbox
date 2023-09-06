# Installing the Client Tools

First identify a system from where you will perform administrative tasks, such as creating certificates, kubeconfig files and distributing them to the different VMs.

My local system is a linux box, so I will be using this host for these administrative tasks.

## Access all VMs

As part of the vagrant provision process ssh-keys are generated for
the vagrant user on the VMs. We can use vagrant to ssh to a single VM,
but we will be using other secure shell operations. Therefor we will
extract the vagrant config into a ssh config file which can be used
for all commands.

Using Vagrant to ssh to a VM. (This can only be done from the vagrant folder.)

```bash
vagrant ssh <host>
```

Generate a ssh-config file for the setup (in the top level of the repo).

```bash
rm -f ssh-config
pushd vagrant
for H in master-1 master-2 loadbalancer worker-1 worker-2 worker-3; do vagrant ssh-config $H >> ../ssh-config; done
popd
```

Test using the new config to ssh to the `master-1` VM.

```bash
ssh -F ssh-config master-1
```

## Install kubectl

The [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl). command line utility is used to interact with the Kubernetes API Server. Download and install `kubectl` from the official release binaries:

Reference: [https://kubernetes.io/docs/tasks/tools/install-kubectl/](https://kubernetes.io/docs/tasks/tools/install-kubectl/)

### Linux

```bash
wget https://storage.googleapis.com/kubernetes-release/release/v1.28.1/bin/linux/amd64/kubectl
chmod +x kubectl
sudo mv kubectl /usr/local/bin/
```

### Verification

Verify `kubectl` version 1.28.1 or higher is installed:

```
kubectl version -o yaml
```

> output

```
clientVersion:
  buildDate: "2023-08-24T11:23:10Z"
  compiler: gc
  gitCommit: 8dc49c4b984b897d423aab4971090e1879eb4f23
  gitTreeState: clean
  gitVersion: v1.28.1
  goVersion: go1.20.7
  major: "1"
  minor: "28"
  platform: linux/amd64
kustomizeVersion: v5.0.4-0.20230601165947-6ce0bf390ce3

The connection to the server localhost:8080 was refused - did you specify the right host or port?
```

Don't worry about the error at the end as it is expected. We have not set anything up yet!

Prev: [Compute Resources](02-compute-resources.md)<br>
Next: [Certificate Authority](04-certificate-authority.md)

# Contiv installation for Kubernetes

This document details the setup instructions for Kubernetes version 1.4+ and higher for CentOS 7

Install kubernetes 1.4 or higher using http://kubernetes.io/docs/getting-started-guides/kubeadm/ and follow the instructions below.

### Install Contiv

* For ACI setups use contiv/contiv_aci.yaml instead of contiv/contiv.yaml.
* Replace all instances of NETMASTER_IP in contiv/contiv.yaml with the master IP
* Replace VLAN_IF with the data plane interface
* Optional: Replace the contiv version(v0.1-11-30-2016.20-08-20.UTC) with the desired release/test version.
* Optional ACI only steps:
  - Replace __APIC_xxx__ fields with their corresponding values.
  - APIC_EPG_BRIDGE_DOMAIN and APIC_CONTRACTS_UNRESTRICTED_MODE are optional fields.   
  - Password based authentication: When using password based authentication to ACI APIC_CERT_DN fields should be deleted.
  - Certificate based authentication: 
    - When using certificate based authentication to ACI APIC_PASSWORD can be empty. 
    - Copy the certificate to a file named aci.key. 
    - Create a secret using `kubectl create secret generic aci.key --from-file=<path name of aci.key file> -n kube-system`
 
* On the management node, run
```sh
kubectl apply -f contiv.yaml
```
* Get netctl from a Contiv release or local build. Contiv releases are available from https://github.com/contiv/netplugin/releases

### Using Contiv

1. Create the default network and EPG.
```sh
netctl net create -t default --subnet=20.1.1.0/24 default-net
netctl group create -t default default-net default-epg
```
See https://github.com/contiv/netplugin/tree/master/mgmtfn/k8splugin for some examples on how to use Contiv networking with your pods.

### Uninstall Contiv
```sh
kubectl delete -f contiv.yaml
```


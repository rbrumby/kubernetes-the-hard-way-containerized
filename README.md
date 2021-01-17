# kubernetes-the-hard-way-kubeadm-style
Following on from Kelsey Hightower's legendary [kubernetes-the-hard-way](https://github.com/kelseyhightower/kubernetes-the-hard-way) but in this version, we will create a cluster with self-hosted (containerized) control plane nodes.

If you have installed a cluster using kubeadm, you have probably noticed that the only things that are actually installed & run directly on the nodes host OS are the container runtime (Docker, containerd, etc) & the kubelet.

This repository is aimed at guiding you through similar steps to what kubeadm does in deploying control plane & worker nodes purely for the sake of learning.
If you want to install a cluster for real in the way kubeadm does it, I suggest installing it using... ...erm... ...kubeadm!

If however you want to get a deeper understanding of how the cluster is built - read on.

## How this guide is organized
All commands are embedded in the documents. There are no scripts to download & run - the point is to read & understand what you are executing.
Each document is numbered X.YY where X is the section & YY is the step within the section. The sections are organized as follows:

- Section 0 contains the one-off preparatory steps which can be run on any machine (not necessarily a machine which will form part of the cluster)
  - [0.00: Introduces the cluster topology that the guide will create](./0.00-introduction-and-server-topology.md)
  - [0.10: Creates a script to set environment variables throughout subsequent steps](./0.10-create-an-environment-script.md)
  - [0.20: Creates the certificates & encryption configuration for the cluster](./0.20-certificate-and-encryption-config.md)
  - [0.30: Creates a kubeconfig file for us to use as the cluster administrator](./0.30-create-admin-user-kubeconfig.md)

- Section 1 contains the steps needed to run ***any node*** (control plane or worker)
  - [1.10: Details which files created in section 0 are needed for which node type](./1.10-copy-certificates-to-node.md)
  - [1.20: Installs containerd, kubelet & their dependencies on the node](./1.20-install-kubelet-and-dependencies.md)
  - [1.30: Creates a bootstrap kubeconfig for the kubelet & starts the kubelet for the first time](./1.30-bootstrap-setup-and-start-kubelet.md)

- Section 2 installs etcd on control plane nodes - you really need to get this far on all control plane nodes before moving to section 3
  - [2.10: Deploys etcd as a pod](./2.10-start-etcd.md)

- Section 3 deploys the rest of the node-specific components needed for the control plane
  - [3.10: Deploys the API server as a pod](./3.10-start-api-server.md)
  - [3.20: Creates the cluster objects to allow the kubelet to bootstrap](./3.20-setup-kubelet-tls-bootstrapping.md)
  - [3.30: Deploys the controller manager as a pod](./3.30-configure-and-start-kube-controller-manager.md)
  - [3.40: Deploys the scheduler as a pod](./3.40-configure-and-start-scheduler.md)

- Section 4 sets up the cluster network & only needs to be done ***once for the entire cluster***
  - [4.10: Deploys kube-proxy & weave daemonsets & a CoreDNS deployment](./4.10-networking.md)

- If you are creating the first control plane node, follow sections 0 - 4
- If you are creating a subsequent control plane nodes, follow sections 1 - 3
- If you are creating a worker node, follow section 1

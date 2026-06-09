## Overcoming the real paradox of managing Kubernetes 😅

---

### Introduction

In the previous project, you explored Kubernetes using K3s and K3d — lightweight distributions designed for simplicity and speed. You learned how to bootstrap clusters, deploy workloads, expose services, and even automate deployments using GitOps with Argo CD.

Now it’s time to face the real thing.

This project introduces you to a **standard Kubernetes cluster** using **kubeadm**, closer to what you’ll encounter in production environments. You will understand how Kubernetes is bootstrapped manually, how networking really works, and how to operate a multi-node cluster.

This is where Kubernetes stops being “magic” and starts making sense.

---

## Stack

**Technologies used:**

* Linux (Ubuntu or Debian)
* kubeadm / kubelet / kubectl
* Container runtime (containerd)
* CNI (Calico or Flannel)
* NGINX Ingress Controller
* Helm
* GitHub (for configs)
* Argo CD (GitOps part)

---

## From K3s to “Real” Kubernetes

> K3s hides a lot of Kubernetes complexity for the sake of simplicity. While this is perfect for learning and edge deployments, production Kubernetes clusters rely on kubeadm, external CNIs, explicit TLS, and well-defined control plane components.

In this project, you will:

* Manually bootstrap a Kubernetes cluster using kubeadm
* Configure networking (CNI) yourself
* Join worker nodes to the cluster
* Deploy workloads using real manifests
* Expose applications with Ingress
* Add GitOps on top

This transition teaches you what Kubernetes *really* does under the hood.

---

## Mandatory Part

This project is divided into three parts and **must be done in order**:

* Part 1: Kubernetes cluster with kubeadm
* Part 2: Applications, Services, and Ingress
* Part 3: GitOps on a real cluster

---

## Part 1: Kubernetes Cluster with kubeadm

You must create **3 virtual machines** using Vagrant (or any local hypervisor).

### Requirements

* OS: Latest stable Ubuntu or Debian
* Resources (minimum):

  * 1 CPU
  * 1 GB RAM
  * 10 GB Disk
* Hostnames:

  * `<login>-CC-Master`
  * `<login>-CC-Worker1`
  * `<login>-CC-Worker2`
* Private IPs (eth1):

  * Master: `192.168.56.120`
  * Worker1: `192.168.56.121`
  * Worker2: `192.168.56.122`
* Passwordless SSH between machines

---

### Tasks

1. Install containerd on all nodes
2. Disable swap and configure kernel modules
3. Install kubeadm, kubelet, kubectl
4. Initialize the cluster on the master:

```bash
kubeadm init --pod-network-cidr=10.244.0.0/16
```

5. Configure kubectl for the user
6. Install a CNI (Calico or Flannel)
7. Join both workers using the kubeadm join command
8. Verify:

```bash
kubectl get nodes
```

---

## Part 2: Applications, Services, and Ingress

Now that you have a real Kubernetes cluster, you will deploy **three applications**.

### Requirements

* 3 Deployments (any language / image)
* 3 Services (ClusterIP or NodePort)
* 1 Ingress resource
* 1 Ingress Controller (NGINX)
* 1 namespace dedicated to these applications
* Readiness and liveness probes for every application
* CPU and memory requests/limits for every Pod
* At least one NetworkPolicy that blocks unwanted traffic
* One of the three applications packaged as a Helm chart

---

### Behavior

All apps must be accessible using host-based routing:

| Hostname       | App   |
| -------------- | ----- |
| app1.localhost | App 1 |
| app2.localhost | App 2 |
| app3.localhost | App 3 |

They must resolve to the master IP:
`192.168.56.120`

---

### Example Flow

1. Install Ingress Controller
2. Create Deployments + Services
3. Create an Ingress rule
4. Add `/etc/hosts` entries
5. Verify routing in the browser

### No Regression From Project 03

This project must not be a weaker repeat of Project 03. Moving from K3s to kubeadm should make Kubernetes clearer, not remove engineering hygiene.

You must keep:

* namespaces instead of deploying everything in `default`
* meaningful probes
* resource requests and limits
* basic NetworkPolicy isolation
* manifests tracked in Git
* a short explanation of what changed between K3s and kubeadm

Helm is mandatory for one application because the core curriculum expects students to understand chart authoring, values, and repeatable deployment templates.

---

## Part 3: GitOps on Real Kubernetes

This is where things get spicy 😈

---

### Tasks

1. Install Argo CD in namespace `argocd`
2. Create namespace `dev`
3. Push Kubernetes manifests to a public GitHub repo
4. Create an Argo CD Application that deploys:

   * One of your apps
   * Automatically from GitHub
5. Make a change in GitHub and verify auto-sync

---

### Goal

Your cluster must:

* Continuously deploy from GitHub
* Self-heal on drift
* Be observable via Argo CD UI

You must also explain what Argo CD is allowed to change and what it must not change.

---

## Deliverables

Your repository must include:

* Vagrantfile or VM setup notes for the 3-node cluster
* scripts or documented steps for installing containerd, kubeadm, kubelet, kubectl, and CNI
* Kubernetes manifests for all apps, services, ingress, namespace, resources, probes, and NetworkPolicies
* one Helm chart for one app
* Argo CD application manifest
* README explaining verification steps and failure cases

---

## Evaluation Focus

Be ready to explain:

* what kubeadm does during cluster bootstrap
* why swap must be disabled
* what CNI does and how Pod networking works
* how a worker joins the cluster
* how ingress reaches a service
* what your Helm values control
* how Argo CD detects and repairs drift
* which security rules protect the app namespace

Booting three VMs is not enough. A working cluster with weak manifests is not enough.

## Bonus Challenges (Optional but 🔥)

* Add TLS with cert-manager
* Add Horizontal Pod Autoscaler
* Install Prometheus + Grafana
* Add stricter egress NetworkPolicies

---

## Final Notes

This project teaches you:

* How Kubernetes *actually* boots
* Why CNIs matter
* How real clusters differ from K3s
* What production-grade setups look like
* Why GitOps is not optional anymore

###

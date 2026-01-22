## Who let the pods out ? WOO WOO WOOOO!

---

</p>
<p align="center">
<img src="https://github.com/buflallo/Club-cloud/blob/main/Beginner-Project03-Kuberenetes-k3s/imgs/k9s.png" width="500">
</p>

---

### Introduction :

This project aims to deepen your knowledge by making you use **K3s**, **K3d**, **Vagrant**, and real-world Kubernetes practices. It builds directly on the previous projects (Virtualization, Provisioning, and Containerization).

You will learn how to:

- Set up personal virtual machines with Vagrant and the distribution of your choice.
- Install and operate a small K3s cluster (server + worker) and use its Ingress.
- Transition from Docker Compose to Kubernetes for routing and scaling simple apps.
- Use K3d together with Argo CD to implement a basic GitOps workflow.
- Add first-class **observability** (metrics, dashboards, logs), **health checks & auto-healing**, and **security** (RBAC, NetworkPolicies) to your cluster.

These steps will get you started with Kubernetes not just as a toy, but as a small, realistic platform you can observe, debug, and secure.

This project is no longer a “minimal introduction” only: it is a challenging workshop designed to prepare you for real-world Kubernetes.

---

## Hardware Requirements

Kubernetes, K3d, Prometheus, and logging stacks consume resources. To keep things realistic:

- **Recommended laptop**: at least **4 vCPUs** and **16 GB RAM**.
- **Minimum for full experience** (including Prometheus + Grafana):
	- At least **2 vCPUs** and **8 GB RAM** available for your lab VMs and Docker/k3d.
	- Ability to allocate ~**2–3 GB RAM** to the K3d-based lab in Part 3.

If your laptop **cannot** reasonably meet these minimums:

- All **health checks, NetworkPolicies, RBAC, and centralized logging** remain **mandatory**.
- **Prometheus + Grafana** become **optional**. In that case, you must:
	- Clearly document your hardware limits in your README.
	- Describe how you would plug in Prometheus + Grafana on a larger machine (which metrics/dashboards, where in the manifests).

Evaluators may ask you to justify skipping Prometheus + Grafana based on your machine’s capacity.

---

## Stack 

</p>
<p align="center">
<img src="https://github.com/buflallo/Club-cloud/blob/main/Beginner-Project03-Kuberenetes-k3s/imgs/1_SHO_OgUDu_7_lH305VqYrw.png" width="500">
</p>

## From docker/docker compose to kubernetes

> Transitioning from Docker and Docker Compose to learning K3s involves moving from managing containerized applications on a single system to orchestrating containers in a lightweight Kubernetes environment. Docker and Docker Compose provide an accessible entry point to containerization, allowing users to build, run, and manage containers, and define multi-container applications through Compose files. However, as applications scale, more robust orchestration becomes necessary, and that’s where Kubernetes, specifically K3s, comes in. K3s is a minimal, resource-efficient Kubernetes distribution designed for simplicity, making it an excellent next step for those familiar with Docker. It offers essential Kubernetes features such as service discovery, scaling, and multi-host networking, enabling learners to manage containerized applications across clusters. This transition allows for hands-on experience with distributed container management and prepares users for larger Kubernetes deployments.

---


## Mandatory part

This project will consist of setting up several environments under specific rules.

It is divided into three parts you have to do in the following order:

• Part 1: K3s and Vagrant (cluster bootstrap)

• Part 2: K3s and three simple applications (Ingress, health checks, basic security)

• Part 3 - GitOps - K3d and Argo CD (GitOps, observability, logging, security)

---

## Part 1: K3s and Vagrant

To begin, you have to set up 2 machines.

Write your first Vagrantfile using the latest stable version of the distribution of your choice as your operating system. It is STRONGLY advised to allow only the bare minimum in terms of resources: 1 CPU, 512 MB of RAM (or 1024). 

The machines must be run using Vagrant.
Here are the expected specifications:

• The machine names must be the name of the student followed by -CC (e.g Abdessamad-CC). 

The hostname of the first machine must be followed by the capital letter S (like Server). 

The hostname of the second machine must be followed by SW (like ServerWorker).

Have a dedicated IP on the eth1 interface. The IP of the first machine (Server) will be 192.168.56.110, and the IP of the second machine (ServerWorker) will be 192.168.56.111.

Be able to connect with SSH on both machines with no password.

You will set up your Vagrantfile according to modern practices.

You must install K3s on both machines:
* In the first one (Server), it will be installed in controller mode.
* In the second one (ServerWorker), in agent mode.
* You will have to use kubectl (and therefore install it too).

---

## Part 2: K3s and three simple applications

You now understand the basics of K3s. Time to go further! 

To complete  this part, you will need only one virtual machine with the distribution of your choice (latest stable version) and K3s in server mode installed.

You will set up 3 web applications of your choice that will run in your K3s instance.

You will have to be able to access them depending on the HOST used when making a
request to the IP address 192.168.56.110. The name of this machine will be your name login followed by -CC (e.g. Abdessamad-CC).

Here is a small example diagram:

</p>
<p align="center">
<img src="https://github.com/buflallo/Club-cloud/blob/main/Beginner-Project03-Kuberenetes-k3s/imgs/k8s.png" width="500">
</p>

### New mandatory requirements for Part 2

In addition to the original goal (3 apps exposed via Ingress and HOST-based routing), you must now treat this K3s cluster as a small production-like environment:

- **Namespace isolation**
	- All 3 applications must run in a dedicated namespace (for example, `apps`), not in the `default` namespace.

- **Health checks & auto-healing**
	- Each of the 3 applications must define **readiness** and **liveness** probes.
	- Probes must be meaningful (checking a real HTTP endpoint or TCP port) and tuned to avoid flapping.
	- At least one application must have **3 replicas**; deleting one Pod must not cause downtime.
	- You must be able to simulate a failure (for example, breaking the liveness endpoint) and observe Kubernetes restarting the Pod.

- **Basic security with NetworkPolicies**
	- You must implement a **default deny** NetworkPolicy (at least for ingress) in the application namespace.
	- You must implement an allow policy that lets the Ingress controller (or a dedicated frontend namespace) reach your apps on HTTP/HTTPS.
	- You must demonstrate that traffic from a “random” Pod without the right labels cannot reach your services directly.

- **Resource hygiene & observability prep**
	- All application Pods must define **CPU and memory requests/limits**.
	- If you install `metrics-server` in this cluster, you should be able to run `kubectl top pods -n <your-namespace>` and see CPU/memory values.

These patterns (probes, namespaces, NetworkPolicies, resource requests/limits) are the foundation you will extend in Part 3 with GitOps, monitoring, and logging.

> **Tip**: Don’t rush to Part 3 until you can comfortably explain, for each of your Part 2 apps, what its probes do, which namespace it runs in, and how NetworkPolicies protect it.


## Part 3 : GITOPS k3d and argo CD

You now master a minimalist version of K3S! Time to set up everything you have just learnt (and much more!) but without Vagrant this time. To begin, install **K3d** on your virtual machine.

You will need Docker for K3d to work, and probably some other
software too. Thus, you have to write a script to install every
necessary package and tool.

First of all, you must understand the difference between K3S and K3D.

Once your configuration works as expected, you can start to create your first continuous integration! To do so, you have to set up a small infrastructure following the
logic illustrated by the diagram below:

</p>
<p align="center">
<img src="https://github.com/buflallo/Club-cloud/blob/main/Beginner-Project03-Kuberenetes-k3s/imgs/argocd.png" width="500">
</p>

You have to create at least two namespaces:

• The first one will be dedicated to Argo CD (for example, `argocd`).

• The second one will be named `dev` and will contain an application. This application will be automatically deployed by Argo CD using your online Github repository.

You will also create a `monitoring` namespace for observability (see below).

Yes, indeed. You will have to create a public repository on Github
where you will push your configuration files. A tiny example GitOps repository layout is provided in the sibling folder `Beginner-Project03-GitOps-Example/` to help you get started, but you are expected to adapt and understand it.

You are free to organize your own Git repository the way you like, as long as Argo CD manages all Kubernetes manifests for your dev app, monitoring, and logging.

#### How to approach Part 3 without getting lost

To avoid being overwhelmed, tackle Part 3 in this order:

1. **Bootstrap Argo CD**: install Argo CD in the `argocd` namespace and verify you can reach its UI.
2. **Create namespaces**: make sure `argocd`, `dev`, and `monitoring` exist.
3. **Wire GitOps for the dev app**: create a minimal Argo CD `Application` that deploys a simple app into `dev` (Deployment + Service + Ingress + probes), reusing your Part 2 experience.
4. **Add logging (Loki + Fluent Bit)**: deploy the logging stack in `monitoring`, then verify you can see `dev` logs in Loki/Grafana.
5. **Harden with RBAC and NetworkPolicies**: restrict Argo CD’s scope and lock down the `dev` namespace so only allowed traffic and components can reach it.
6. **(If hardware allows) Add Prometheus + Grafana metrics**: once everything else is stable, integrate metrics and dashboards.

The sibling folder `Beginner-Project03-GitOps-Example/` shows one possible layout and minimal manifests for these steps. Use it to understand the roles of each component, then design your own repository with your own app, images, and domain.

#### GitOps & Application management

- Your `dev` application must be deployed and updated **only** via Argo CD from a public Git repository.
- You must have at least one Argo CD `Application` for infra (namespaces, monitoring/logging, Argo Projects) and one for the `dev` app.
- Changing the app version (for example, image tag v1 → v2) must be done by changing Git, then letting Argo CD synchronize the cluster.

#### Observability: Prometheus + Grafana (hardware-dependent)

- If your laptop meets the **minimum hardware** requirements:
	- You must deploy **Prometheus + Grafana** (for example, in the `monitoring` namespace) to collect and visualize metrics.
	- At least one Grafana dashboard must show **per-pod CPU and memory usage** for the `dev` application.
	- You must configure at least one simple **Prometheus alert rule** (for example, on high CPU or frequent restarts).
- If your hardware does **not** meet the minimum:
	- You may skip installing Prometheus + Grafana, but you must document why, and explain how you would plug them in using your GitOps repo on a stronger machine.

#### Logging: Loki + Fluent Bit (blessed stack)

- Centralized logging is **mandatory** for everyone.
- The reference stack for this project is **Loki + Fluent Bit**:
	- Fluent Bit runs as a DaemonSet and collects container logs from your cluster.
	- Logs are enriched with Kubernetes metadata (namespace, pod, container, labels).
	- Logs are pushed to a **Loki** service in the `monitoring` namespace.
- You must be able to query logs for the `dev` namespace and your main app (for example, by `namespace="dev"` and `app="your-app"`) using Grafana or another Loki-compatible UI.
- Promtail is **not** allowed in this project; use Fluent Bit (or another supported collector if agreed with your instructors).

#### Health checks & auto-healing (GitOps-aware)

- The `dev` application managed by Argo CD must define readiness and liveness probes.
- You must be able to:
	- Deploy a **healthy** version (v1) that passes probes and serves traffic.
	- Deploy a **broken** version (for example, failing readiness or using a bad image) and observe how Argo CD and Kubernetes report degraded or failed health.
- Optional but recommended: configure a **Horizontal Pod Autoscaler (HPA)** for the `dev` app, versioned in Git and applied by Argo CD.

#### Security: RBAC and NetworkPolicies

- Argo CD must not run as unrestricted cluster-admin for everything.
- You must:
	- Restrict Argo CD’s privileges (for example, via AppProjects and/or dedicated service accounts) so it can manage only the namespaces it needs (`dev`, `monitoring`, `argocd`).
	- Secure the Argo CD UI with a non-default admin password and, ideally, a limited user/role that can view/sync the `dev` app but not reconfigure everything.
- You must enforce isolation around the `dev` namespace using **NetworkPolicies**:
	- Default deny for `dev`.
	- Explicit allows for Ingress traffic and observability/logging components.
	- Demonstrate that Pods in other namespaces cannot directly call Services in `dev`.

#### Bonus ideas (for the ambitious)

These are not required for passing, but will strongly increase your mastery:

- Add an HPA that actually scales on load and demonstrate it during evaluation.
- Improve secret management (for example, using sealed-secrets or SOPS) so sensitive data never appears in plain YAML in Git.

### Yayy... progress...

</p>
<p align="center">
<img src="https://github.com/buflallo/Club-cloud/blob/main/Beginner-Project03-Kuberenetes-k3s/imgs/k8s-meme.png" width="500">
</p>

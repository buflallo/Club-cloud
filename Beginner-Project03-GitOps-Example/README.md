# Beginner Project 03 – GitOps Example Repo

This folder contains a **tiny example GitOps repository** for the K3d + Argo CD part of Project 03.

- It is **not** a full solution.
- It shows a **minimal layout** and a few stub manifests to help you understand how to structure your own GitOps repo.
- You are expected to adapt it, extend it, or re-implement it, not blindly copy it.

High-level structure:

- `argo-apps/` – Argo CD `Application` resources for infra and dev workloads.
- `cluster/` – Cluster-wide or shared resources (namespaces, baseline RBAC, default NetworkPolicies).
- `infra/` – Infrastructure components managed by Argo CD (Argo config, monitoring, logging).
- `apps/dev/` – Dev application manifests (namespace, Deployment, Service, Ingress, HPA, NetworkPolicies, RoleBindings).

Read the comments inside each YAML file and adapt them to your own cluster, names, and domain.

## How to use this example (and what is forbidden)

Use this repo as a **learning map**, not as your project hand-in.

Recommended way to work:

- Open each folder and answer for yourself: “What problem does this manifest solve?”
- Trace how Argo CD Applications in `argo-apps/` point to the `infra/` and `apps/dev/` paths.
- Map each requirement from the main Project 03 README (probes, NetworkPolicies, RBAC, logging) to the relevant example file.
- Start your own GitOps repo and copy only small pieces you fully understand, adapting names, images, domains, and namespaces.

Forbidden shortcuts:

- You must **not** submit this repository as-is (or with only cosmetic changes) as your own work.
- You must **not** keep placeholder values (like example repo URLs, usernames, or passwords) in your final project.
- You must **not** skip understanding what a manifest does just because it “works” when applied.

If you feel lost:

- Start from the `apps/dev/` manifests and make sure you can explain Deployment, Service, Ingress, and NetworkPolicy in your own words.
- Then move to `infra/monitoring-logging/` to understand how Fluent Bit and Loki connect, and how Grafana discovers Loki.
- Finally, study the `argo-apps/` and `infra/argocd/` manifests to see how Argo CD ties everything together.

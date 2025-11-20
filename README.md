# platform-gitops

This repository defines the entire Kubernetes platform as code, managed through GitOps.
It contains cluster bootstrap configuration, platform services, security, policy, networking, and observability.
ArgoCD watches this repository and continuously reconciles the cluster state.

This repo manages:

- ArgoCD installation and App-of-Apps pattern
- Ingress (Nginx or Traefik)
- ExternalDNS
- cert-manager (TLS automation)
- Cluster Autoscaler & VPA
- Prometheus Operator
- Grafana dashboards and configuration
- Loki, Promtail, Tempo (observability)
- External Secrets Operator
- Gatekeeper policies (OPA)
- NetworkPolicies and PodSecurity configurations

---

## Structure

```
platform-gitops/
├── bootstrap/
│   ├── argocd/
│   └── root-application.yaml
├── clusters/
│   ├── dev/
│   ├── staging/
│   └── prod/
└── apps/
    ├── observability/
    ├── security/
    ├── networking/
    └── core/
```

---

## Responsibilities
- Bootstrap ArgoCD using an App-of-Apps pattern.
- Configure all cluster-wide services.
- Define observability stack as code.
- Apply security and policy controls.
- Manage cluster networking components.
- Operate all add-ons declaratively.

---

## GitOps Flow

1. Infra repo provisions cluster.
2. ArgoCD is installed via `bootstrap/argocd/`.
3. ArgoCD syncs `root-application.yaml`.
4. All platform services are deployed from `apps/`.
5. Each folder represents a service deployed declaratively with Kustomize overlays.

---

## Sync Manually
```
kubectl apply -f bootstrap/root-application.yaml
```

---

## Principles

- No kubectl apply from humans.
- All cluster configuration is managed via Git.
- Multi-environment support via Kustomize overlays.
- Cluster always converges to Git state.

---

## Purpose

This repo aligns with Certified Argo Project Associate (CAPA) competencies, including:
- AppProjects
- RBAC
- ApplicationSets
- Sync waves & hooks
- Progressive delivery with Rollouts

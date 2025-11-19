# platform-gitops

GitOps repository containing the full Kubernetes platform stack.  
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
  argo/
    install/
    apps/
  platform/
    ingress/
    cert-manager/
    external-dns/
    metrics-server/
    autoscaling/
    observability/
    security/
  secrets/
  ci/
```

---

## GitOps Flow

1. Terraform deploys the cluster and initial ArgoCD manifests.
2. ArgoCD syncs the `argo/apps/app-of-apps.yaml`.
3. App-of-Apps deploys all platform components.
4. Each folder represents a service deployed declaratively with Kustomize overlays.

---

## Principles

- No kubectl apply from humans.
- All cluster configuration is managed via Git.
- Multi-environment support via Kustomize overlays.
- Cluster always converges to Git state.

---

## Purpose

This repo aligns with Certified ArgoCD Associate (CAA) competencies, including:
- AppProjects
- RBAC
- ApplicationSets
- Sync waves & hooks
- Progressive delivery with Rollouts

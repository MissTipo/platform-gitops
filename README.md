# Platform GitOps

This repository defines the full Kubernetes platform stack using GitOps, managed by ArgoCD via Helm charts. It includes all cluster-level services, security, networking, and observability, deployed declaratively and environment-aware using Kustomize overlays.

## Overview

* **ArgoCD Installation:** Helm-based, with CRDs installed automatically.
* **Bootstrap:** App-of-Apps pattern to deploy all platform components.
* **Environments:** Dev, Staging, Prod, with environment-specific overlays.
* **Platform Services:** Ingress, cert-manager, ExternalDNS, metrics, monitoring, logging, security policies, cluster autoscaling.

## Repository Structure

```
platform-gitops/
├── README.md
├── bootstrap/
│   ├── kustomization.yaml
│   └── argocd-root-application.yaml
├── argo/
│   ├── install/
│   │   ├── base/
│   │   │   ├── Chart.yaml
│   │   │   ├── values.yaml
│   │   │   └── kustomization.yaml
│   │   └── overlays/
│   │       ├── dev/
│   │       │   ├── values-dev.yaml
│   │       │   └── kustomization.yaml
│   │       ├── staging/
│   │       │   ├── values-staging.yaml
│   │       │   └── kustomization.yaml
│   │       └── prod/
│   │           ├── values-prod.yaml
│   │           └── kustomization.yaml
│   └── apps/
│       ├── app-of-apps-helm.yaml
│       └── projects/
│           ├── platform.yaml
│           └── security.yaml
├── platform/
│   ├── ingress/
│   ├── cert-manager/
│   ├── external-dns/
│   ├── metrics-server/
│   ├── prometheus/
│   ├── grafana/
│   ├── loki/
│   ├── tempo/
│   ├── external-secrets/
│   ├── gatekeeper/
│   └── cluster-autoscaler/
├── secrets/
│   └── external-secrets/
└── ci/
    └── validate-manifests.yml
```

## Getting Started

1. **Add the Argo Helm repo:**

```bash
helm repo add argo https://argoproj.github.io/argo-helm
helm repo update
```

2. **Create the ArgoCD namespace:**

```bash
kubectl create namespace argocd
```

3. **Install ArgoCD via Helm with CRDs:**

```bash
helm install argocd argo/argo-cd --namespace argocd --set crds.install=true
```

4. **Bootstrap the platform stack via App-of-Apps:**

```bash
kubectl apply -k bootstrap/
```

5. **Access ArgoCD UI locally:**

```bash
kubectl port-forward service/argocd-server -n argocd 8080:443
```

* Username: `admin`
* Password: Retrieve with:

```bash
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```

* Delete the initial secret after changing the password:

```bash
kubectl -n argocd delete secret argocd-initial-admin-secret
```

## Principles

* No manual `kubectl apply` for platform services.
* All cluster configuration managed via Git.
* Environment-specific overlays for dev, staging, prod.
* Cluster always converges to Git state.
* Helm is used for bootstrapping ArgoCD; all other services deployed via GitOps.

## Supported Components

* ArgoCD (App-of-Apps)
* Nginx Ingress Controller
* cert-manager (TLS automation)
* ExternalDNS
* Cluster Autoscaler & VPA
* Metrics-server, Prometheus, Grafana, Loki, Tempo
* External Secrets Operator
* Gatekeeper (OPA policies)
* NetworkPolicies and PodSecurity configurations

## Notes

* Currently using Helm-based ArgoCD installation.
* Ingress is configured per environment using Kustomize overlays.
* Observability and security components are all managed declaratively.
* Multi-environment support allows testing and deployment isolation for dev, staging, and prod.


# GitOps Lab – Argo CD + Kustomize + Kubernetes

This repository demonstrates a production-aligned GitOps workflow using:

* Kubernetes
* Argo CD
* Kustomize
* Declarative Application manifests

It showcases environment overlays, automated synchronization, drift detection, and self-healing using Argo CD.

---

## Architecture Overview

```
Git Repository
      ↓
Argo CD watches repository
      ↓
Kustomize renders manifests
      ↓
Kubernetes cluster
```

Argo CD continuously reconciles the desired state stored in Git with the live cluster state.

---

## Repository Structure

```
gitops-lab/
├── base/
│   ├── deployment.yaml
│   ├── service.yaml
│   └── kustomization.yaml
│
├── overlays/
│   └── dev/
│       └── kustomization.yaml
│
└── argocd/
    └── application.yaml
```

---

## Components

### 1. Base Manifests (`base/`)

Contains reusable Kubernetes resources:

* `Deployment`
* `Service`
* `kustomization.yaml`

These define the core application configuration.

---

### 2. Environment Overlay (`overlays/dev/`)

Implements environment-specific customization using Kustomize:

* Replica overrides
* Name prefixing
* Environment labels

This allows multiple environments (dev, stage, prod) using the same base configuration.

---

### 3. Argo CD Application (`argocd/application.yaml`)

Defines how Argo CD:

* Connects to the repository
* Tracks a specific path (`overlays/dev`)
* Deploys to the cluster
* Enables automated sync
* Enables prune and self-heal

---

## Key GitOps Features

### Automated Sync

Argo CD automatically applies changes when new commits are pushed.

### Pruning

Resources removed from Git are automatically deleted from the cluster.

### Self-Heal

If someone manually changes a resource in the cluster, Argo CD restores it to the desired state.

---

## Kustomize Strategy

This repository follows a standard GitOps pattern:

* **Base layer** → reusable application manifests
* **Overlay layer** → environment-specific modifications

This ensures:

* Clear separation of concerns
* Reusability
* Scalability for multi-environment deployments

---

## How It Works

1. Developer updates manifests in Git.
2. Changes are pushed to the repository.
3. Argo CD detects the new revision.
4. Argo CD runs Kustomize internally.
5. Argo CD compares desired vs live state.
6. Differences are reconciled automatically.

---

## Extending This Lab

This repository can be extended with:

* Multiple environments (stage, prod)
* App-of-Apps pattern
* Sealed Secrets
* Argo Rollouts
* Image Updater
* CI integration

---

## Technologies Used

* Kubernetes
* Argo CD
* Helm
* Kustomize
* GitOps Principles

---

## License

This project is intended for learning and demonstration purposes.

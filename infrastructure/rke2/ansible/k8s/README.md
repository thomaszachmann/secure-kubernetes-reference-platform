# ArgoCD

ArgoCD wird mittels ArgoCD-Operator installiert. Dieser Operator wird mittels des Operator Lifecycle Managers installiert.

Die Ordnerstruktur ist wie folgt:

- `initial` - Software, die initial installiert werden muss, damit ArgoCD läuft
- `cluster-configuration` - Anwendungen, die mittels ArgoCD-Applications installiert werden sollen

## Installation OLM (Operator Lifecycle Manager)

Upstream-Doku: https://argocd-operator.readthedocs.io/en/latest/install/olm/

```bash
kubectl create -f initial/olm/crds.yaml
kubectl create -f initial/olm/olm.yaml
```
## Installation ArgoCD-Operator

```bash
kubectl create -f initial/argocd-operator/argocd.ns.yaml
kubectl create -f initial/argocd-operator/argocd-operator.catalogsource.yaml
kubectl create -f initial/argocd-operator/argocd-operator.operatorgroup.yaml
kubectl create -f initial/argocd-operator/argocd-operator.subscription.yaml
```

## Installation ArgoCD

Gitlab Access Token im Secret `initial/argocd/argocd-repo.secret.yaml` als `password` einfügen.

```bash
kubectl create -f initial/argocd/argocd.yaml
kubectl create -f initial/argocd/argocd-repo.secret.yaml
```

## Installation ArgoCD-Applications

Wir nutzen das [App of Apps](https://argo-cd.readthedocs.io/en/stable/operator-manual/cluster-bootstrapping/)-Pattern von ArgoCD:

Nach der Installation von ArgoCD wird eine ArgoCD-Application "App of Apps" angelegt. Diese Application installiert alle anderen Applications.

```bash
kubectl create -f initial/app-of-apps.yaml
```

## Installation weiterer Anwendungen

Um eine neue Anwendung zu installieren, muss im Ordner `cluster-configuration` eine neue ArgoCD-Application angelegt und in das Repository gepushed werden. ArgoCD liest die Konfiguration dann automatisch ein und wendet diese an.

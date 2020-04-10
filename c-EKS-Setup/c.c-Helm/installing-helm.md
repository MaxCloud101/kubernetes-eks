# Installation of Helm package manager

## Install latest version of Helm

```bash
cd 
mkdir helm && cd helm
curl https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3 | bash
cd -
```

## Add official stable Helm repository

```bash
helm repo add stable https://kubernetes-charts.storage.googleapis.com/
```

## Check/Update

```bash
helm version
helm repo list
helm repo update

helm search repo
```

## Test installation of a chart

```bash
helm install stable/redis

helm ls
helm uninstall <<name-of-release-from-previous-output>>
```

# setup

## prerequisites 🗒️

- a k8s cluster: v1.27+
- kubectl: v1.27+
- helm

## getting started 🚀

```sh
# install service monitor CRD
kubectl apply -f https://raw.githubusercontent.com/prometheus-operator/prometheus-operator/main/example/prometheus-operator-crd/monitoring.coreos.com_servicemonitors.yaml

# add required helm repos
helm repo add camunda https://helm.camunda.io
helm repo add elastic http://helm.elastic.co
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo add konghq https://charts.konghq.com
helm repo add codecentric https://codecentric.github.io/helm-charts

# setup helm dependencies
helm dep up && helm dep build

# install/upgrade the release
helm upgrade --install phee .
```

## clean up 🧹

```sh
# delete the release
helm del phee

# delete all pvcs (storage) after all pods are cleared
kubectl delete pvc --all
```

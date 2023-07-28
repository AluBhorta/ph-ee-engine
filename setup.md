# setup

## prerequisites 🗒️

- a k8s cluster
- kubectl
- helm

## getting started 🚀

```sh
# install service monitor CRD
kubectl apply -f https://raw.githubusercontent.com/prometheus-operator/prometheus-operator/main/example/prometheus-operator-crd/monitoring.coreos.com_servicemonitors.yaml

# setup helm dependencies
helm dep up && helm dep build

# install the chart
helm install phee .
```

## clean up 🧹

```sh
# delete the release
helm del phee

# delete all pvcs (storage) after all pods are cleared
kubectl delete pvc --all
```

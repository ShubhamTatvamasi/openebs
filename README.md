# openebs

Create OpenEBS namespace:
```bash
kubectl create ns openebs
```

Install openebs with chart name as openebs:
```bash
helm repo add openebs https://openebs.github.io/charts
helm repo update
helm install --namespace openebs openebs openebs/openebs
```


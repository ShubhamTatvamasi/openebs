# openebs

Install openebs with chart name as openebs:
```bash
helm repo add openebs https://openebs.github.io/charts
helm repo update

helm install openebs openebs/openebs \
  --create-namespace \
  --namespace openebs
```


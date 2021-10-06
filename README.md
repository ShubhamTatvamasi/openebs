# openebs

https://openebs.io/docs/user-guides/localpv-hostpath \
https://openebs.io/docs/user-guides/localpv-hostpath#prerequisites

If you are using the Rancher RKE cluster, you must configure kubelet service with extra_binds for BasePath. If your BasePath is the default directory /var/openebs/local, then extra_binds section should have the following details:
```yaml
services:
  kubelet:
    extra_binds:
      - /var/openebs/local:/var/openebs/local
```

Install openebs with chart name as openebs:
```bash
helm repo add openebs https://openebs.github.io/charts
helm repo update

helm install openebs openebs/openebs \
  --create-namespace \
  --namespace openebs
```

set default storage class:
```bash
kubectl patch storageclass openebs-hostpath \
  --patch='{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'
```


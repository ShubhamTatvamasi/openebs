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

### Replicated Volumes

Install iSCSI on all hosts:
```bash
sudo apt install open-iscsi
sudo systemctl enable --now iscsid
modprobe iscsi_tcp
echo iscsi_tcp >/etc/modules-load.d/iscsi-tcp.conf
```

Verify iSCSI Status:
```bash
sudo systemctl status iscsid
```

Install Jiva Operator:
```bash
kubectl apply -f https://openebs.github.io/charts/jiva-operator.yaml
```

Create replication Jiva volume policy:
```yaml
kubectl create -f - << EOF
apiVersion: openebs.io/v1alpha1
kind: JivaVolumePolicy
metadata:
  name: replication-jivavolumepolicy
  namespace: openebs
spec:
  replicaSC: openebs-hostpath
  target:
    replicationFactor: 1
EOF
```

Create a Storage Class to dynamically provision volumes by specifying above policy:
```yaml
kubectl create -f - << EOF
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: openebs-jiva-csi-sc
provisioner: jiva.csi.openebs.io
allowVolumeExpansion: true
parameters:
  cas-type: "jiva"
  policy: "replication-jivavolumepolicy"
EOF
```

Set `openebs-jiva-csi-sc` as default storage class:
```bash
kubectl patch storageclass openebs-jiva-csi-sc \
  --patch='{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'
```

https://github.com/openebs/jiva-operator/blob/develop/docs/quickstart.md



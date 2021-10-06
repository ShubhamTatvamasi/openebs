# openebs

https://openebs.io/docs/user-guides/localpv-hostpath

Verify iSCSI initiator is installed
```bash
sudo apt install -y open-iscsi
sudo systemctl enable --now iscsid
```

Install openebs with chart name as openebs:
```bash
helm repo add openebs https://openebs.github.io/charts
helm repo update

helm install openebs openebs/openebs \
  --create-namespace \
  --namespace openebs
```


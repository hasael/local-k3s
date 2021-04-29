
## Set up prometheus and grafana in arm

From inside your raspberry pi master

```
apt-get update
apt-get install -y build-essential golang git
git clone https://github.com/carlosedp/cluster-monitoring
```


Configure vars.jsonnet
- Set `k3s.enabled` to `true`.
- Change your K3s master node IP(your VM or host IP) on `k3s.master_ip` parameter.
- Edit `suffixDomain` to have your node IP with the `.nip.io` suffix or your cluster URL. This will be your ingress URL suffix. Pick one of the workers
- Set _traefikExporter_ `enabled` parameter to `true` to collect Traefik metrics and deploy dashboard.
- Set armexporter to true

Run 
```
make vendor
make
kubectl apply -f manifests/setup/
kubectl apply -f manifests/
```

Check created pods with

```bash
kubectl get po -n monitoring
```

---
#arm #k8s #k8s 
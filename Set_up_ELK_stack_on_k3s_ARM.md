# Set up ELK stack on k3s ARM
Set up ELK stack using helm package manager for k8s
### Install Helm
- Download your [desired version](https://github.com/helm/helm/releases)
- Unpack it `tar -zxvf`
- Find the `helm` binary in the unpacked directory, and move it to its desired destination (`mv linux-amd64/helm /usr/local/bin/helm`))

### Clone helm charts
From the master raspberry pi, download helm chart github [here](https://github.com/elastic/helm-charts)
Inside of each folder `elasticsearch`,`filebeat`,`kibana`etc are contained chart yaml files and instructions.

### Install ElasticSearch
Scale down replicas and resources on `/elasticsearch/values.yaml`
```
helm install elasticsearch
```

### Install Kibana
Scale down replicas and resources on `/kibana/values.yaml`
```
helm install kibana
```

To access kibana port forward to localhost
```
kubectl port-forward deployment/kibana-kibana 5601
```

### Install Filebeat
At the moment of this writing, there is no official docker image for Filebeat in ARM platform.
Currently not an official one [kasaoden/filebeat](https://hub.docker.com/r/kasaoden/filebeat). You can edit the image to use inside `filebeat/values.yaml` file, and scale the resources down as in previous steps.

### Kibana management
Log in kibana (using port forward) and from `Management` -> `Create a new index pattern` with the `filebeat-*` pattern.

Now all stdout of your pods are searchable

---

Sources: [ELK stack install](https://itnext.io/deploy-elastic-stack-on-kubernetes-1-15-using-helm-v3-9105653c7c8), [Helm Install](https://helm.sh/docs/intro/install/) 

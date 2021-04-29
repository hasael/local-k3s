# Local K8s
A local k8s cluster is handy for personal playground and study purposes. It is also useful to abstract infrastructure as a code removing the need to install it locally or through docker.
I ran it using a cluster of raspberry pis running k3s.

## Setting up the cluster
- Setup the raspberries following the [[Raspberry Pi setup]]
- Install k3s on the cluster [[Install k3s through Ansible|using Ansible]]
- [[Set up prometheus and grafana in arm]] to have monitoring
- [[Set up ELK stack on k3s ARM]] for logging
- [[K8s auto update script]] to update the K8S infrastructure on a CI/CD pipeline

## Use case in Haskell
To have a running haskell pod, I used the [Haskell on raspberry pi| base Haskell image](https://github.com/hasael/aarch64-haskell-base) that uses [Nix](https://nixos.org/).

---
Sources: [Jeff Geerling YT video](https://www.youtube.com/watch?v=N4bfNefjBSw) , [Pi Dramble](https://www.pidramble.com/wiki)

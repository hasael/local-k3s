# Local K8s
A local k8s cluster is handy for personal playground and study purposes. It is also useful to abstract infrastructure as a code removing the need to install it locally or through docker.
I ran it using a cluster of raspberry pis running k3s. For the hardware setup I followed the [Pi Dramble](https://www.pidramble.com/wiki) guide.

## Setting up the cluster
- Setup the raspberries following the [Raspberry Pi setup](Raspberry_pi_setup.md)
- Install k3s on the cluster [using Ansible](Install_k3s_ansible.md)
- [Set up prometheus and grafana in arm](Set_up_prometheus_and_grafana_in_arm.md) to have monitoring
- [Set up ELK stack on k3s ARM](Set_up_ELK_stack_on_k3s_ARM.md) for logging
- [K8s auto update script](K8s_auto_update_script) to update the K8S infrastructure on a CI/CD pipeline

## Use case in Haskell
To have a running haskell pod, I used the [base Haskell image](https://github.com/hasael/aarch64-haskell-base) that uses [Nix](https://nixos.org/).

---
Sources: [Jeff Geerling YT video](https://www.youtube.com/watch?v=N4bfNefjBSw)

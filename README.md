# cilium_gatewayAPI_k3s_vagrant_libvirt_ansible

Vagrant-libvirt setup that creates a VM with [k3s](https://k3s.io) and
[Cilium](https://cilium.io) as the CNI.

This setup does not have a ingress controller running, instead it is using the
[GatewayAPI specification](https://gateway-api.sigs.k8s.io/). It automatically
deploys everything so the nginx deployment is reachable from the outside.

This setups disables the kube-proxy component and let's Cilium take over this
role, as this is a prerequisite for using the GatewayAPI. Details can be found
[in the
docs](https://docs.cilium.io/en/stable/network/kubernetes/kubeproxy-free/#kubeproxy-free).

Default OS is openSUSE Leap 15.6, but that can be changed in the Vagrantfile.
Please be aware, that this might break the Ansible provisioning.

## Vagrant

1. You need `vagrant`, obviously. And `git`. And Ansible...
1. Fetch the box, per default this is `opensuse/Leap-15.5.x86_64`, using
   `vagrant box add opensuse/Leap-15.5.x86_64`.
1. Make sure the git submodules are fully working by issuing
   `git submodule init && git submodule update`
1. Run `vagrant up`
1. Run `kubectl --kubeconfig ansible/k3s-kubeconfig get nodes` and you should
   see your server.
1. Open the URL that Ansible printed in the end, it looks something like this:

   ```
   http://nginx.192.0.2.13.sslip.io
   ```

   (where `192.0.2.13` is the VM's IP address)

1. You should see the Nginx welcome page. Party!

## Cleaning up

The VMs can be torn down after playing around using `vagrant destroy`. This will
also remove the kubeconfig file `ansible/k3s-kubeconfig`.

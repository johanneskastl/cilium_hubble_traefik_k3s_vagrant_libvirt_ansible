# cilium_hubble_traefik_k3s_vagrant_libvirt_ansible

Vagrant-libvirt setup that creates a VM with [k3s](https://k3s.io) and
[Cilium](https://cilium.io) as the CNI. Traefik is still being used as the
default ingress controller, as in standard k3s.

In addition to Cilium as a CNI, the hubble observability tool is being installed
and its UI is exposed via an ingress (without authentication!).

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
1. Open the Nginx URL that Ansible shows near the end of the provisioning, it
   looks something like this:

   ```
   http://nginx.192.0.2.13.sslip.io
   ```

   (where `192.0.2.13` is the VM's IP address)

1. You should see the Nginx welcome page.
1. Open the Hubble URL that Ansible printed out at the end of the provisioning.
   It looks something like this:

   ```
   http://hubble.192.0.2.13.sslip.io
   ```

   (where `192.0.2.13` is the VM's IP address)
1. Open the `default` namespace and you should see the traffic graph with
   Traefik (in `kube-system` namespace) sending traffic to the Nginx service.
1. Hit the Nginx URL a few times and you should see your requests show up in the
   bottom of the Hubble UI.
1. Party!

## Cleaning up

The VMs can be torn down after playing around using `vagrant destroy`. This will
also remove the kubeconfig file `ansible/k3s-kubeconfig`.

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
1. Open the URL that Ansible printed in the end, it looks something like this:

   ```
   http://nginx.192.0.2.13.sslip.io
   ```

   (where `192.0.2.13` is the VM's IP address)

1. You should see the Nginx welcome page. Party!

## Cleaning up

When tearing down the machine, the kubeconfig and token files that was download
does not get deleted unfortunately. To not cause problems the next time you
start, just run `rm ansible/k3s-kubeconfig ansible/k3s-token` and all is well.

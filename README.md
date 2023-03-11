# A CKA lab for kubeadm with KVM/libvirt

This is an essential, quick&dirty (but complete) local lab to practice some `kubeadm` and `etcdctl` commands. More painful than minikube, but also more fun!

It has been built on a Fedora 37 + KVM and it's built on top of two components:

- `virt-lightning`, a simple but very effective way of spinning KVM VMs; it's **tons** faster than Vagrant and it has very little requirements; it also has an embedded function to produce an Ansible inventory. Head over to [the `virt-lightning` README](./virt-lightning/README.virt-lightning.mdown) to learn more.

- `ansible` to install locally all dependencies and calling `kubeadm` (for both starting and configuring the master and joining the workers). This is executed from the host, and could/should have been put in form of roles. A few of these could/should have been `cloud-init` configs, but I focused on customizing rapidly rather than building efficiently. More on that [in the README in the ansible dir](./ansible/README.ansible.mdown)

## Running commands

Since I'm lazy and my shell history can only span a few thousand lines, I resorted to a few [Taskfiles](https://taskfile.dev) to call the usual commands:

```zsh
❯ task
task: Available tasks for this project:
* cluster-access:                 Prints an export command to eval. Run "eval `task cluster-access`".
* cluster-info:                   Test access to the API Server
* local-setup:                    Install and configure the local workstation
* redo:                           Tear down and rebuild all lab
* ansible:playbook-init:          Prepare the OS
* ansible:playbook-kubeadm:       Bootstrap Kubernetes with kubeadm
* virt-lightning:create:          Create the lab machines
* virt-lightning:destroy:         Destroy the whole bunch of machines
* virt-lightning:start:           Start all VMs
```

## Requirements

At the very minimum, you'll need:

- A fairly decent host (16 GB RAM and a quad-core CPU will do)
- [Taskfiles](https://taskfile.dev) and Kubectl binary in path. If you have `asdf` you can benefit from the local `.tool-versions` file.
- And of course, `libvirt` and the KVM implementation for your battle station, with a permissive `sudoers`

Everything else can be set up by calling `task local-setup`.

ansible-galaxy collection install ansible.posix
ansible-galaxy collection install community.general

kubeadm config images pull
kubeadm init --ignore-preflight-errors=NumCPU,Mem

Static pods at /etc/kubernetes/manifests
[kubelet-start] Writing kubelet environment file with flags to file "/var/lib/kubelet/kubeadm-flags.env"
[kubelet-start] Writing kubelet configuration to file "/var/lib/kubelet/config.yaml"
/etc/kubernetes/admin.conf


kubeadm join 192.168.123.5:6443  --ignore-preflight-errors=NumCPU,Mem --token kh6as7.cceg8ja94h62f0ai \
	--discovery-token-ca-cert-hash sha256:456af8849e5d25763216adafccf630c0439a2ef52c7898f6b68eee64941de67b
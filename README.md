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

curl https://raw.githubusercontent.com/projectcalico/calico/v3.24.5/manifests/calico.yaml -O



/etc/polkit-1/rules.d/10.virt.rules:

polkit.addRule(function(action, subject) {
    if (action.id == "org.libvirt.unix.manage"
            && subject.local
            && subject.active
            && subject.isInGroup("libvirt")) {
        return polkit.Result.YES;
    }
});

# sed -i 's/initialDelaySeconds: [0-9]\+/initialDelaySeconds: 180/' /etc/kubernetes/manifests/kube-apiserver.yaml

mkdir /tmp/kustom

cat > /tmp/kustom/kustomization.yaml <<EOF
patchesJson6902:
- target:
    version: v1
    kind: Pod
    name: kube-apiserver
    namespace: kube-system
  path: patch.yaml
EOF

cat > /tmp/kustom/kube-apiserver0+json.yaml <<EOF
- op: replace
  path: /spec/containers/0/livenessProbe/initialDelaySeconds
  value: 180
- op: replace
  path: /spec/containers/0/livenessProbe/timeoutSeconds
  value: 30
EOF

sudo kubeadm init --config config.yaml -k /tmp/kustom

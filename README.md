## Setting up Kubernetes Cluster
* swapoff -a
* setenforce 0
* Disable SE linux
vi /etc/selinux/config
cat <<EOF > /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
EOF
* sudo yum install -y kubelet kubeadm kubectl
* systemctl start kubelet && systemctl enable kubelet
* cat <<EOF> /etc/sysctl.d/k8s.conf
> net.bridge.bridge-nf-call-ip6tables = 1
> net.bridge.bridge-nf-call-iptables = 1
> EOF

* sysctl --system
vim kube-config.yml

kubeadm join 172.31.24.187:6443 --token rkt825.mlq6btpvlbcyp7ue \
    --discovery-token-ca-cert-hash sha256:d82d486f535c727a655cd4dc5209f2791553c83d08fba41f5c67f7f11eb767f3 
edi vim /etc/kubernetes/manifests/kube-controller-manager.yaml 

systemctl restart kubelet
kubectl apply -f https://docs.projectcalico.org/v3.14/manifests/calico.yaml

https://phoenixnap.com/kb/how-to-install-kubernetes-on-centos

## Setting up Prometheus
use script install_prometheus.sh
## Install node exporter in worker nodes
use script install_node_exporter.sh
## Install grafana
https://www.fosslinux.com/8328/how-to-install-and-configure-grafana-on-centos-7.htm
Create script for installing grafana
Create data source
Prometheus
## Recording rules
cd /etc/prometheus


## Alertmanager
https://medium.com/devops-dudes/prometheus-alerting-with-alertmanager-e1bbba8e6a8e

https://github.com/linuxacademy/content-kubernetes-prometheus-env
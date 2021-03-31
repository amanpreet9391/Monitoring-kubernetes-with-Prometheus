# Application Monitoring with Prometheus
<img width="692" alt="Screenshot 2021-04-01 at 3 49 12 AM" src="https://user-images.githubusercontent.com/25201552/113219859-d5d69f80-929f-11eb-894a-3dae7d938b06.png">

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
> net.bridge.bridge-nf-call-ip6tables = 1</br>
> net.bridge.bridge-nf-call-iptables = 1</br>
> EOF
* sysctl --system
* Copy paste this token to worker nodes -> kubeadm join token`generated at master node`</br>
* systemctl restart kubelet
<img width="1205" alt="Screenshot 2021-03-30 at 4 59 42 PM" src="https://user-images.githubusercontent.com/25201552/113219913-f0a91400-929f-11eb-9d37-726f43b3507b.png">

## Setting up Prometheus
use script install_prometheus.sh
<img width="1403" alt="Screenshot 2021-03-31 at 3 06 48 AM" src="https://user-images.githubusercontent.com/25201552/113219979-0f0f0f80-92a0-11eb-917b-7087fe3eb259.png">
</br>
## Install node exporter in worker nodes
use script install_node_exporter.sh
## Install grafana
use script install_grafana.sh
Create data source -> Prometheus
<img width="1427" alt="Screenshot 2021-03-31 at 3 29 31 AM" src="https://user-images.githubusercontent.com/25201552/113220045-28b05700-92a0-11eb-8939-e13ef4a46299.png">
</br>
## Recording rules
Recording rules allows us to precompute frequently needed or computationally expensive expressions and save their result as a new set of time series.</br>
create a `prometheus_rules.yml` containing the necessary rule statements and then have Prometheus load the file via the `rule_files` field in the prometheus configuration i.e., `prometheus.yml`.</br>
<img width="1409" alt="Screenshot 2021-03-31 at 4 16 20 AM" src="https://user-images.githubusercontent.com/25201552/113219804-bb042b00-929f-11eb-8533-b4e3b24e7f5d.png">
</br>



## Alertmanager
The AlertManager uses a configuration file named `alertmanager.yml`</br>
we create the AlertManager systemd service</br>
Then reload the daemon and start the alertmanager service</br>

This is how the Alerts will be visible on Prometheus Dashboard.</br>
<img width="1424" alt="Screenshot 2021-03-31 at 4 56 20 AM" src="https://user-images.githubusercontent.com/25201552/113220159-50072400-92a0-11eb-9bd6-9d5a34f577e8.png">
</br>
This is how alert will come on alertmanager dashboard</br>
<img width="1415" alt="Screenshot 2021-03-31 at 4 56 35 AM" src="https://user-images.githubusercontent.com/25201552/113220387-b9873280-92a0-11eb-9d06-b486e1c4aa85.png">
</br>
We can configure the notification part i.e. where we want to get alert notification, Slack, Email, Pagerduty etc. In this case, alert notification will come on Slack `alerts` channel.</br>
<img width="1436" alt="Screenshot 2021-03-31 at 4 56 04 AM" src="https://user-images.githubusercontent.com/25201552/113220449-d28fe380-92a0-11eb-9d97-f2fd193275a2.png">
</br>
After the problem causing alert is being resolved.</br>
<img width="1419" alt="Screenshot 2021-03-31 at 5 03 46 AM" src="https://user-images.githubusercontent.com/25201552/113220573-0a972680-92a1-11eb-8279-1e9149412055.png">
</br>

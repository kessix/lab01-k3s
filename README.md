# Cluster

172.1.0.10 manager.k3scluster.local
172.1.0.11 worker01.k3scluster.local
172.1.0.12 worker02.k3scluster.local

### ----------------

## Instalando manager 
curl -sfL https://get.k3s.io | sh -

### Alterar ip do server em /etc/rancher/k3s/k3s.yaml
### criando link 
ln -s /etc/rancher/k3s/k3s.yaml ~/.kube/config
### matando os processos do k3s
k3s-killall.sh
### executando novamente o daemon do k3s 
k3s server --node-external-ip=172.1.0.10 2>%1 &
### verificando se está ok e com ip correto
kubectl cluster-info
kubectl get node

## Adiconando agents no cluster
curl -sfL https://get.k3s.io | K3S_URL=https://172.1.0.10:6443 K3S_TOKEN=K103ba5c7608aac82ac966b616cb9f1dfab6d7d66b44396adf6719667e6c0be6215::server:b9698a48251dfc4dfbdb7e19f9ad9bce sh -

export K3S_URL=manager.k3scluster.local
export K3S_TOKEN=K103ba5c7608aac82ac966b616cb9f1dfab6d
k3s-killall.sh

k3s agent --server https://$K3S_URL:6443 --token ${K3S_TOKEN}

### ----------------

Ajuste centos/8 mininal que instalei com o vagrant

cd /etc/yum.repos.d && sed -i 's/mirrorlist/###mirrorlist/g' /etc/yum.repos.d/CentOS-* && sed -i 's|###baseurl=http://mirror.centos.org|baseurl=http://vault.centos.org|g' /etc/yum.repos.d/CentOS-*

echo '### Ajust VAR PATH' >> /etc/bashrc && echo PATH="$PATH:/usr/local/bin" >> /etc/bashrc

Obs.:
desabilitei o SELinux em todos no manager e nos nodes

### ----------------

erro incluir worker no cluster
curl -sfL https://get.k3s.io | K3S_URL=https://172.1.0.10:6443 K3S_TOKEN=K10ff2729b899da11807d407abfc4d4c9cd9c2bf5cb0d3908ed4022c29896c5ddbc::server:5f60b98250c4262439d0ae0542101931 sh -

The connection to the server localhost:8080 was refused - did you specify the right host or port?

[root@manager ~]### kubectl cluster-info
Kubernetes control plane is running at https://127.0.0.1:6443
CoreDNS is running at https://127.0.0.1:6443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy
Metrics-server is running at https://127.0.0.1:6443/api/v1/namespaces/kube-system/services/https:metrics-server:https/proxy


Obs.: 
A doc já dizia:
A kubeconfig file will be written to /etc/rancher/k3s/k3s.yaml and the kubectl installed by K3s will automatically use it

==> server: https://127.0.0.1:6443 ------> alterar para o ip do servidor

fonte: https://github.com/k3s-io/k3s/issues/3862
server

    Install the k3s on my server
    edit /etc/rancher/k3s/k3s.yaml and change to my server IP address,
    Copy /etc/rancher/k3s/k3s.yaml on my server machine itself to ~/.kube/config



[blogpost](https://devopscube.com/kubernetes-cluster-vagrant/)


fork repo

git clone git@github.com:pabloqpacin/vagrant-kubeadm-kubernetes.git ~/repos/vagrant-kubeadm-kubernetes

## config vbox anfitrión

```bash
sudo mkdir -p /etc/vbox/
echo "* 0.0.0.0/0 ::/0" | sudo tee -a /etc/vbox/networks.conf

vup
vssh controlplane
```

```bash
vagrant@controlplane:~$ ip -brief a
lo               UNKNOWN        127.0.0.1/8 ::1/128
eth0             UP             10.0.2.15/24 metric 100 fe80::a00:27ff:fec8:9864/64
eth1             UP             10.0.0.10/24 fe80::a00:27ff:fed0:c61e/64
cni0             UP             10.85.0.1/16 fe80::b43d:62ff:fea0:8a50/64
veth41e5e25e@if2 UP             fe80::c4df:a4ff:fe02:bbc/64
veth20debf15@if2 UP             fe80::cc22:76ff:fee2:cd85/64
veth16b58740@if2 UP             fe80::8da:b9ff:fed7:b5ac/64
```

```bash
apt install grc
echo -e "\nalias kc='grc kubectl'" >> /root/.bashrc
bash

root@controlplane:~# kc get nodes -o wide
# NAME           STATUS   ROLES           AGE     VERSION   INTERNAL-IP   EXTERNAL-IP   OS-IMAGE             KERNEL-VERSION       CONTAINER-RUNTIME
# controlplane   Ready    control-plane   5m29s   v1.29.0   10.0.0.10     <none>        Ubuntu 22.04.4 LTS   5.15.0-116-generic   cri-o://1.31.0
# node01         Ready    worker          4m12s   v1.29.0   10.0.0.11     <none>        Ubuntu 22.04.4 LTS   5.15.0-116-generic   cri-o://1.31.0
# node02         Ready    worker          3m      v1.29.0   10.0.0.12     <none>        Ubuntu 22.04.4 LTS   5.15.0-116-generic   cri-o://1.31.0

root@controlplane:~# kc -n kube-system get pods
# NAME                                       READY   STATUS                      RESTARTS   AGE
# calico-kube-controllers-74d5f9d7bb-h2lsq   1/1     Running                     0          5m40s
# calico-node-9k4p4                          0/1     Init:CreateContainerError   2          4m40s
# calico-node-9m4vg                          0/1     Init:CreateContainerError   0          3m28s
# calico-node-w9f8m                          0/1     Init:CreateContainerError   0          5m40s
# coredns-76f75df574-kgb8d                   1/1     Running                     0          5m40s
# coredns-76f75df574-qlhrn                   1/1     Running                     0          5m40s
# etcd-controlplane                          1/1     Running                     0          5m53s
# kube-apiserver-controlplane                1/1     Running                     0          5m53s
# kube-controller-manager-controlplane       1/1     Running                     0          5m53s
# kube-proxy-29b7d                           1/1     Running                     0          4m40s
# kube-proxy-485fc                           1/1     Running                     0          3m28s
# kube-proxy-jscdv                           1/1     Running                     0          5m40s
# kube-scheduler-controlplane                1/1     Running                     0          5m53s
# metrics-server-d4dc9c4f-qfsg4              1/1     Running                     0          5m40s

kubectl -n kube-system describe pod calico-foo
    # pivot_error
    # ...

```


<!-- 
## vagrantfile

- networking
  - yo nat: 10.0.2.15 for each vm
  - yo hostonly: 10.0.0.0/24
  - yo kubeadm pod-network-cidr: 10.0.0.0/16
  - yo calico ipPools: 10.0.0.0/16
  - vkk nat: 10.0.2.15 for each vm
  - vkk hostonly: 10.0.0.0/24
  - vkk kubeadm pod-network-cidr: 10.0.0.0/16
  - vkk calico ipPools: 10.0.0.0/16
- releases
  - yo k8s: 1.29
  - vkk k8s: ...


## scripts
 -->

# Домашнее задание к занятию "12.5 Сетевые решения CNI"

## Задание 1: установить в кластер CNI плагин Calico

Для проверки других сетевых решений стоит поставить отличный от Flannel плагин — например, Calico. Требования: 
* установка производится через ansible/kubespray;
* после применения следует настроить политику доступа к hello-world извне. Инструкции [kubernetes.io](https://kubernetes.io/docs/concepts/services-networking/network-policies/), [Calico](https://docs.projectcalico.org/about/about-network-policy)

Создаем hello-node
```
vagrant@vagrant:~/netology-12-kubernetes-05-cni$ kubectl create deployment hello-node --image=registry.k8s.io/echoserver:1.4
deployment.apps/hello-node created
vagrant@vagrant:~/netology-12-kubernetes-05-cni$ kubectl get deployments
NAME         READY   UP-TO-DATE   AVAILABLE   AGE
hello-node   1/1     1            1           7s
vagrant@vagrant:~/netology-12-kubernetes-05-cni$ kubectl get pods
NAME                          READY   STATUS    RESTARTS   AGE
hello-node-7c7c59b7cb-dds7v   1/1     Running   0          14s
vagrant@vagrant:~/netology-12-kubernetes-05-cni$
```

Применяем политику

```
vagrant@vagrant:~/netology-12-kubernetes-05-cni$ kubectl apply -f network-policy/hello-policy-allow.yaml
networkpolicy.networking.k8s.io/hello created
vagrant@vagrant:~/netology-12-kubernetes-05-cni$ kubectl get networkpolicies
NAME    POD-SELECTOR   AGE
hello   app=hello      2m49s
vagrant@vagrant:~/netology-12-kubernetes-05-cni$
```


## Задание 2: изучить, что запущено по умолчанию

Самый простой способ — проверить командой calicoctl get <type>. Для проверки стоит получить список нод, ipPool и profile.
Требования: 
* установить утилиту calicoctl;
* получить 3 вышеописанных типа в консоли.


Собственно используем calicoctl
```
vagrant@vagrant:~/netology-12-kubernetes-05-cni$ ./calicoctl get nodes --allow-version-mismatch
NAME
cp1
node1
node2
node3
node4

vagrant@vagrant:~/netology-12-kubernetes-05-cni$ ./calicoctl get ipPool --allow-version-mismatch
NAME           CIDR             SELECTOR
default-pool   10.233.64.0/18   all()

vagrant@vagrant:~/netology-12-kubernetes-05-cni$ ./calicoctl get profile --allow-version-mismatch
NAME
projectcalico-default-allow
kns.default
kns.kube-node-lease
kns.kube-public
kns.kube-system
ksa.default.default
ksa.kube-node-lease.default
ksa.kube-public.default
ksa.kube-system.attachdetach-controller
ksa.kube-system.bootstrap-signer
ksa.kube-system.calico-node
ksa.kube-system.certificate-controller
ksa.kube-system.clusterrole-aggregation-controller
ksa.kube-system.coredns
ksa.kube-system.cronjob-controller
ksa.kube-system.daemon-set-controller
ksa.kube-system.default
ksa.kube-system.deployment-controller
ksa.kube-system.disruption-controller
ksa.kube-system.dns-autoscaler
ksa.kube-system.endpoint-controller
ksa.kube-system.endpointslice-controller
ksa.kube-system.endpointslicemirroring-controller
ksa.kube-system.ephemeral-volume-controller
ksa.kube-system.expand-controller
ksa.kube-system.generic-garbage-collector
ksa.kube-system.horizontal-pod-autoscaler
ksa.kube-system.job-controller
ksa.kube-system.kube-proxy
ksa.kube-system.namespace-controller
ksa.kube-system.node-controller
ksa.kube-system.nodelocaldns
ksa.kube-system.persistent-volume-binder
ksa.kube-system.pod-garbage-collector
ksa.kube-system.pv-protection-controller
ksa.kube-system.pvc-protection-controller
ksa.kube-system.replicaset-controller
ksa.kube-system.replication-controller
ksa.kube-system.resourcequota-controller
ksa.kube-system.root-ca-cert-publisher
ksa.kube-system.service-account-controller
ksa.kube-system.service-controller
ksa.kube-system.statefulset-controller
ksa.kube-system.token-cleaner
ksa.kube-system.ttl-after-finished-controller
ksa.kube-system.ttl-controller

vagrant@vagrant:~/netology-12-kubernetes-05-cni$
```


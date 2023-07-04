# ETCD in Kubernetes

`ETCD` 라는 **Key-Value 데이터 저장소**는 클러스터에 관한 정보를 저장한다. 예를들어 Nodes, Pods, Configs, Secrets, Accounts, Roles, Bindings, Others 와 같은 것들을 말이다.

`kubectl` 명령어를 실행할 때마다 얻게 되는 모든 정보는 `ETCD` Server에서 얻게된다.

Node 추가, Pod 배포, ReplicaSet 생성 등 모든 클러스터에 변화를 주는 행위는 ECTD Server에 업데이트가 된다.

> ETCD Server에 업데이트가 되어야만 특정 행위가 완료된 것으로 간주된다.

클러스터를 어떻게 설정하느냐에 따라서 ECTD는 다양하게 배포된다. 직접 배포하는 방법이랑 kubeadm 도구를 이용한 배포 방법이 존재한다.

**직접 배포**하는 방법은 다음과 같이 직접 다운로드해서 배포한다.

```shell
wget -q -https-only \
  "https://github.com/coreos/etcd/releases/download/v3.3.9/etcd-v3.3.9-linux-amd64.tar.gz"
```

kubeadm을 이용해서 클러스터를 설정하면 kubeadm이 ETCD Server를 배포한다. 다음과 같이 etcd-master의 이름이 붙어있는 Pod으로 배포한 것을 볼 수 있다.

```shell
kubectl get pods -n kube-system
```

**쿠버네티스가 저장한 모든 키를 열거**하려면 다음과 같은 명령어를 사용하면 된다.

```shell
kubectl exec etcd-master –n kube-system etcdctl get / --prefix –keys-only
```

고가용성 환경에서는 클러스터에는 여러 개의 마스터 노드가 존재할 것이다. 이 때 ETCD Server도 여러 개가 존재하게 된다.
워커노드에 존재하는 kubelet은 **Pod을 노드에서 실행**하거나, Kubernetes 클러스터에 **node를 등록**하는 역할을 수행한다.

Kubelet은 Node에서 Pod을 실행시키라는 명령이 내려오면 Container Runtime Engine에게 요청하여 필요한 이미지를 pulling 하고 컨테이너를 실행하라고 한다.

그런다음 Kubelet은 **Pod의 상태를 지속적으로 모니터링하고 Pod의 상태를 kube-apiserver에 보고**한다.
# Installing kubelet

Kubelet을 설치하는 방법은 다음과 같다.

```shell
wget https://storage.googleapis.com/kubernetes-release/release/v1.13.0/bin/linux/amd64/kubelet
```

**kubeadm으로 클러스터를 구성하더라도 kubelet은 kubeadm이 설치해주지 않는다. 따라서 항상 설치를 수동으로 해줘야한다.**

이에 따라서 kubelet 옵션도 또한 실행중인 프로세스를 보는 명령어로 봐야한다.

```shell
ps -aux | grep kubelet
```

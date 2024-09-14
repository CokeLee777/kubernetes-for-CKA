# Scheduler

Kube Scheduler는 Node의 Pod에 대한 스케줄링을 담당한다.

Kube Scheduler는 **오직 어떤 Pod이 어느 Node에 들어갈지만 결정**한다. 즉, Pod을 Node에 두는 역할을 하는 것이 아니다. 그 책임은 kubelet이 담당하고 있다.

Kube Scheduler가 특정 Pod에 알맞은 Node를 찾는 과정은 다음과 같다.

1. Filter Nodes
  -  Kube Scheduler는 A Pod에 맞지 않는 노드를 걸러낸다.
  -  예를 들어 몇몇 Node의 CPU와 메모리 리소스가 A Pod이 필요로하는 리소스보다 부족한 상황이면 거른다는 것이다.
2. Rank Nodes
   - Scheduler는 남은 Node들에게 가장 적합한 순서대로 우선순위를 부여한다.
   - 예를 들어 Scheduler는 Pod를 특정 Node에 설치한 후 Node에 얼마만큼의 리소스 여유가 있는지 계산한다. -> **리소스가 더 여유로운 Node를 우선순위로 두게된다.**
# Installing kube-scheduler

Kube Scheduler를 직접 설치하는 방법은 다음과 같다.

```shell
wget https://storage.googleapis.com/kubernetes-release/release/v1.13.0/bin/linux/amd64/kube-scheduler
```

kubeadm으로 클러스터를 구성했다면 이미 kube admin이 Kube Scheduler를 static pod으로 배포를 하기 때문에 따로 설치가 필요없다.

따라서 Kube Scheduler의 스펙은 단순히 다음 경로에서 볼 수 있다.

```shell
cat /etc/kubernetes/manifests/kube-scheduler.yaml
```

kubeadm으로 클러스터를 구성하지 않았다면 다음과 같은 경로에서 옵션을 확인할 수 있다.

```shell
cat /etc/systemd/system/kube-scheduler.service
```

또한 실행중인 프로세스의 정보를 확인함으로서 옵션을 확인할 수 있다.

```shell
ps -aux | grep kube-scheduler
```


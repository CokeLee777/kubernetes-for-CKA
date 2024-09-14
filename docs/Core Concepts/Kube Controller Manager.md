# Controller Manager

Controller Manager는 쿠버네티스의 다양한 컨트롤러를 관리한다. Controller는 마치 사무실이나 한 부서와 비슷한 맥락이다. 즉, 각자 맡은 책임이 존재한다는 것이다.

모든 Controller들은 이 Kube-Controller Manager로 패키지화 되어있다. 따라서 Kube-Controller Manager를 설치하면 다양한 Controller들이 설치된다.

## Controller

컨트롤러는 시스템 내 다양한 구성 요소의 상태를 지속적으로 모니터링하고 시스템 전체를 잘 동작하도록 제어한다.

**Node Controller**

Node Controller는 kube-apiserver를 통해서 **Node들의 상태를 모니터링**하고 응용 프로그램이 계속 실행되도록 필요한 행동을 한다.

또한 Node Controller는 **5초마다 Node들의 상태를 확인**한다.

Node의 Health Check가 Fail 되면 Node Controller는 Node의 상태를 UNREACHABLE 상태로 표시하는데, UNREACHABLE 상태를 표시하기 전에 40초 정도를 기다린 후 그 후에도 Health Check가 Fail이면 UNREACHABLE로 상태를 표시한다.

UNREACHABLE 상태가 된 후 5분동안 Node가 다시 정상상태가 될 때까지 기다린다. 만약 5분 안에 Node가 정상상태가 되지 않는다면 Node Controller는 해당 Node에 할당된 Pod들을 제거하고 다른 정상상태의 Node로 제거된 Pod들을 띄운다.(단, Pod 들이 ReplicaSet에 포함된 경우에만)

**Replication Controller**

Replication Controller는 **ReplicaSet의 상태를 모니터링**하고 원하는 수의 Pod이 Set내에서 항상 사용 가능하도록 제어한다.

Pod 하나가 다운되면 다른 Pod이 띄워지는 것처럼 말이다.

이 두개의 컨트롤러 이외에도 각자 맡은 책임이 있는 여러 개의 컨트롤러들이 존재한다. Deployment-Controller, Namespace-Controller, Service-Account-Controller 등등 말이다.
# Installing kube-controller-manager

Kube Controller Manager를 설치하는 방법은 다음과 같다.

```shell
wget https://storage.googleapis.com/kubernetes-release/release/v1.13.0/bin/linux/amd64/kube-controller-manager
```

kubeadm으로 클러스터를 구성했다면 이미 kube admin이 Kube Controller Manager를 static pod으로 배포를 하기 때문에 따로 설치가 필요없다.

따라서 Kube Controller Manager의 스펙은 단순히 다음 경로에서 볼 수 있다.

```shell
cat /etc/kubernetes/manifests/kube-controller-manager.yaml
```

kubeadm으로 클러스터를 구성하지 않았다면 다음과 같은 경로에서 옵션을 확인할 수 있다.

```shell
cat /etc/systemd/system/kube-controller-manager.service
```

또한 실행중인 프로세스의 정보를 확인함으로서 옵션을 확인할 수 있다.

```shell
ps -aux | grep kube-controller-manager
```

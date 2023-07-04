# kube-apiserver

kube-apiserver는 쿠버네티스의 주요 구성요소이다.

kubectl 명령어를 실행하면 쿠버네티스 내부에서 발생하는 과정에 대해서 설명하면 다음과 같다.

[예시 1: Node들 모두 조회]

1. `kubectl get nodes` 명령어를 실행하면 kubectl utility가 kube-apiserver에 도달하게 된다.
2. kube-apiserver가 먼저 사용자의 요청을 인증하고, 유효성을 확인한다.
3. 그런 다음에 ETCD CLUSTER에서 사용자가 원하는 데이터를 회수해서 요청한 정보를 응답한다.

[예시 2: Pod 생성]

1. Pod을 생성하는 명령어를 실행하면 kubectl utility가 kube-apiserver에 도달하게 된다.
2. kube-apiserver가 먼저 사용자의 요청을 인증하고, 유효성을 확인한다.
3. 그런 다음에 Pod을 생성해서 ETCD CLUSTER에 데이터를 업데이트하고 Pod이 생성되었다고 사용자에게 응답한다.
4. 스케줄러는 지속적으로 apiserver를 모니터링하다가 Node가 할당되지 않은 새로운 Pod이 있다는 것을 알아채고 올바른 Node를 식별해서 새로운 Pod를 할당하고 kube-apiserver와 통신하게 된다.
5. kube-apiserver는 ETCD CLUSTER에 정보를 업데이트 한다.
6. kube-apiserver는 해당 정보를 적절한 Worker Node의 kubelet에게 전달한다.
7. kubelet은 Worker Node에 Pod을 생성하고 Container Runtime Engine에게 지시해서 Application Image를 배포한다.
8. 모든것이 완료되면 kubelet은 status를 kube-apiserver로 다시 업데이트하고, kube-apiserver는 바뀐 데이터들을 ETCD CLUSTER에 업데이트한다.

kube-apiserver를 직접 설치하는 방법은 다음과 같다.

```shell
wget https://storage.googleapis.com/kubernetes-release/release/v1.13.0/bin/linux/amd64/kube-apiserver
```

kubeadm을 사용하면 kube-apiserver-master라는 이름의 Pod으로 추가적으로 설치된다. 다음 명령어로 확인할 수 있다.

```shell
kubectl get pods -n kube-system
```
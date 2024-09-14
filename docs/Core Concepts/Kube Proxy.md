
쿠버네티스 **클러스터 안에서는 모든 Pod들은 서로 통신이 가능**하다. 이 통신은 **Pod Networking Solution을 쿠버네티스 클러스터에 배포함으로써 이루어질 수 있다.**

Pod Network는 내부 가상 네트워크로 모든 Pod이 연결되는 클러스터 내 모든 Node에 걸쳐있다. 이 네트워크를 통해서 서로 통신이 가능한 것이다.

이 네트워크를 배포하는데 사용 가능한 솔루션은 많다. **kube-proxy가 대표적으로 이를 해결**해준다.

kube-proxy는 쿠버네티스 클러스터의 **각 노드에서 실행되는 프로세스**이다. 이 kube-proxy의 역할은 **새로운 서비스를 찾아주는 역할**을 한다. **새로운 서비스가 생성될 때마다 각 노드에 적절한 규칙을 만들어서 그 서비스로 트래픽을 전달**한다.

위에서 얘기한 적절한 규칙중 하나는 IP Table 규칙을 만드는 것이다. 클러스터의 각 노드에 IP Table 규칙을 만들어서 서비스의 IP로 향하는 트래픽을 향하게 한다.
# Installing kube-proxy

Kube Proxy를 설치하는 방법은 다음과 같다.

```shell
wget https://storage.googleapis.com/kubernetes-release/release/v1.13.0/bin/linux/amd64/kube-proxy
```

kubeadm을 사용하면 kube-proxy라는 이름의 daemonsets으로 추가적으로 설치된다. 다음 명령어로 확인할 수 있다.

```shell
kubectl get pods -n kube-system
```

```shell
kubectl get daemonset -n kube-system
```

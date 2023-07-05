# Controller Manager

Controller Manager는 쿠버네티스의 다양한 컨트롤러를 관리한다. Controller는 마치 사무실이나 한 부서와 비슷한 맥락이다. 즉, 각자 맡은 책임이 존재한다는 것이다.

모든 Controller들은 이 Kube-Controller Manager로 패키지화 되어있다. 따라서 Kube-Controller Manager를 설치하면 다양한 Controller들이 설치된다.

## Controller

컨트롤러는 시스템 내 다양한 구성 요소의 상태를 지속적으로 모니터링하고 시스템 전체를 잘 동작하도록 제어한다.

**Node Controller**

Node Controller는 kube-apiserver를 통해서 Node들의 상태를 모니터링하고 응용 프로그램이 계속 실행되도록 필요한 행동을 한다.

또한 Node Controller는 5초마다 Node들의 상태를 확인한다.

Node가 다운되면 UNREACHABLE 상태가 표시되고 40초 후에야 신호가 잡힌다. Node가 UNREACHABLE 상태가 된 후에 다시 정상 동작할 때까지 5분이 걸린다. 그렇지 않다면 해당 Node에 할당된 Pod을 제거하고 ReplicaSet의 일부 Pod을 대신 띄우게 된다.

**Replication Controller**

Replication Controller는 ReplicaSet의 상태를 모니터링하고 원하는 수의 Pod이 Set내에서 항상 사용 가능하도록 제어한다.

Pod 하나가 다운되면 다른 Pod이 띄워지는 것처럼 말이다.

이 두개의 컨트롤러 이외에도 각자 맡은 책임이 있는 여러 개의 컨트롤러들이 존재한다.
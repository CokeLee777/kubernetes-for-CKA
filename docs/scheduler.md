# Scheduler

쿠버네티스 Scheduler는 Node의 Pod 에 대한 일정 관리를 담당한다.

Scheduler는 오직 어떤 Pod이 어느 Node에 들어갈지만 결정한다. 즉, Pod을 Node에 두는 역할을 하는 것이 아니다. 그 책임은 kubelet이 담당하고 있다.

Scheduler가 특정 Pod에 알맞은 Node를 찾는 과정은 다음과 같다.

1. Filter Nodes
  -  Scheduler는 A Pod에 맞지 않는 노드를 걸러낸다.
  -  예를 들어 몇몇 Node의 CPU와 메모리 리소스가 A Pod이 필요로하는 리소스보다 부족한 상황이면 거른다는 것이다.
2. Rank Nodes
   - Scheduler는 남은 Node들에게 가장 적합한 순서대로 우선순위를 부여한다.
   - 예를 들어 Scheduler는 Pod를 특정 Node에 설치한 후 Node에 얼마만큼의 리소스 여유가 있는지 계산한다. -> 리소스가 더 여유로운 Node를 우선순위로 두게된다.

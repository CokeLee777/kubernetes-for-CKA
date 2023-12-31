# Pods

Pod는 애플리케이션의 단일 인스턴스이다. 쿠버네티스에서 만들 수 있는 가장 작은 단위이다.

즉, Pod이 있고 그 안에 실행중인 컨테이너가 존재한다. 보통 Pod은 실행중인 컨테이너와 1:1 관계를 형성하고 있다.

애플리케이션 사용자가 증가하면서 트래픽이 많아지면 쿠버네티스는 새로운 Pod을 생성하고 그 안에 새로운 컨테이너를 띄우게 된다.

하지만 하나의 노드가 현재의 사용자 수를 감당하지 못할 만큼 증가해서 용량이 부족하면 클러스터에 새로운 노드를 만들고 추가 Pod을 배포한다.

## Multi-Container Pods

보통 Pod과 container는 1:1 관계를 형성하고 있는데, 하나의 Pod에는 여러 개의 컨테이너가 존재할 수는 있다. 하지만 실행중인 동일한 애플리케이션이 여러 개 존재하지는 않는다.

앞에서 말한 것처럼 응용 프로그램 규모를 늘리고 싶다면 Pod을 추가적으로 만들어야 한다.

하지만 예외적으로 애플리케이션에게 도움을 주는 Helper container가 필요할 수도 있다. 이 경우에는 같은 Pod에 컨테이너를 두 개 가질 수 있다. 단, 이 경우에 두 컨테이너 모두 생명주기가 같다.(한 컨테이너가 생성되면 도우미 컨테이너도 생성되고, 한 컨테이너가 다운되면 도우미 컨테이너도 다운된다.)

두 컨테이너는 서로 직접 통신이 가능하다. localhost로 통신이 가능하다는 것이다. 이는 왜냐하면 같은 네트워크 공간을 공유하기 떄문이다. 또한 같은 저장소를 가지고 있다.

여기서 중요한 점은 Multi-Container Pod은 예외적인 상황이라는 점이다. 보통 Pod과 Container는 1:1로 매핑된다.

## Deployment

- 새로운 Pod을 생성해서 Docker Container를 배포하는 명령어

```shell
kubectl run nginx --image nginx
```

- 기존의 Pod을 삭제하는 명령어

```shell
kubectl delete pod <Pod 이름>
```

- Pod 목록 조회 명령어

```shell
kubectl get pods
```

- Pod 목록 조회(좀 더 많은 정보) 명령어

```shell
kubectl get pods -o wide
```

- 특정 Pod 상세정보 조회 명령어

```shell
kubectl describe pods <Pod 이름>
```
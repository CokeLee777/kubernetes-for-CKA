# Replication Controller

만약 어떤 이유로 앱이 다운되고 사용자가 앱에 접속할 수 없게 되었다고 가정하자.

사용자가 앱에 대한 액세스를 잃지 않도록 하나 이상의 인스턴스의 실행을 동시에 수행하고자 한다.

이 때, 등장한게 Replication Controller이다. Replication Controller는 여러 개의 인스턴스에서 동일한 Pod이 실행되게끔 도와준다. 즉, 고가용성을 제공한다.

Pod을 하나만 사용할 계획이라고 하더라도 복제 컨트롤러를 사용할 수 있다.

-> Pod이 하나라고 해도 Replication Controller는 기존의 Pod이 다운되었을 때 자동으로 새로운 Pod을 불러온다.

또한 Replication Controller는 부하 분산과 오토 스케일링을 위해서도 사용된다.

## Replication Controller & Replica Set

둘 다 용도는 같지만 아예 같지는 않다. Replication Controller는 오래된 기술로 점차 Replica Set으로 대체되고 있다.

Replica Set은 복제를 설정하는 새로운 권장 방식이다.

## Replication Controller in YAML

```yml
# 복제 컨트롤러는 쿠버네티스 버전 1에서 제공
apiVersion: v1
kind: ReplicationController
metadata:
  name: myapp-rc
  labels:
    app: myapp
    type: front-end
# Replication Controller 사양
spec:
  template:
    metadata:
      name: myapp-pod
      labels:
        app: myapp
        type: front-end
    # Pod 사양
    spec:
      containers:
        - name: nginx-controller
          image: nginx
  # 복제본 개수
  replicas: 3
```

## Replica Set in YAML

```yml
# Replica Set의 버전
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: myapp-replicaset
  labels:
    app: myapp
    type: front-end
spec:
  template:
    metadata:
      name: myapp-pod
      labels:
        app: myapp
        type: front-end
    spec:
      containers:
        - name: nginx-container
          image: nginx
  replicas: 3
  # Replica Set은 선택자가 필수
  selector:
    # Pod에 있는 레이블과 일치시킨다
    matchLabels:
      type: front-end
```

## Command

**복제 컨트롤러 정보 조회 명령어**

```shell
kubectl get replicationcontroller
```

**복제 세트 정보 조회 명령어**

```shell
kubectl get replicaset
```

## Replica Set - Labels and Selectors

그렇다면 왜 Replica Set에 Label과 Selector를 붙일까?

Replica Set의 역할은 Pod들을 모니터링하는 것이다. 하나라도 문제가 생기면 새 Pod을 배포하는 것처럼 말이다.

그렇다면 Replica Set은 어느 부분을 모니터링할지 어떻게 알까?

클러스터에는 서로 다른 응용 프로그램을 실행하는 다른 부분이 수백 개 있을 수 있다. 이 때 Pod에 Label을 붙이는게 유용할 것이다.

이렇게 label이 붙은 Pod과 선택자의 matchLabels를 일치시켜서 Replica Set이 어느 Pod을 모니터링할지 알 수 있는 것이다.

## Scale

**파일은 그대로 두고 스케일 업, 다운 하는 명령어**

```shell
kubectl scale --replicas=<업데이트할 복제본의 개수> -f <파일명>
```
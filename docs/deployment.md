# Deployment

우리 애플리케이션을 공개적으로 배포하는 것을 의미한다.

바로 yml 파일 형식부터 알아보면 다음과 같이 replica set과 거의 유사한 형태를 띈다.

```yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp-deployment
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
  selector:
    matchLabels:
      type: front-end
```

**배포 파일 생성 명령어**

```shell
kubectl create -f deployment-definition.yml
```

**배포 파일 생성 명령어(변화의 원인 기록)**

```shell
kubectl create -f deployment-definition.yml --record
```

**배포 조회 명령어**

```shell
kubectl get deployments
```

**클러스터에 생성된 모든 개체(deployment, replicaset, pods 등) 조회 명령어**

```shell
kubectl get all
```

replica set은 deployment과 큰 차이가 없다. deployment가 쿠버네티스 오브젝트를 deployment라고 만든것 뿐이다.

## Deployment - Updates and Rollback

배포를 처음 생성할 때 첫 배포를 수행하고, 새로운 롤아웃은 배포를 수정한다. 추후에도 새로운 롤 아웃이 트리거되면 새 배포 Revision이 생성된다. 즉, Revision 2가 되는 것이다.

이것은 배포에 일어난 변화를 추적할 수 있도록 해주고, 필요하다면 배포의 이전 버전으로 되돌릴 수 있도록 해준다.

**롤아웃 상태를 조회하는 명령어**

```shell
kubectl rollout status deployment/myapp-deployment
```

**롤아웃의 Revision과 이전 기록을 조회하는 명령어**

```shell
kubectl rollout history deployment/myapp-deployment
```

## Deployment Strategy

배포 전략에는 두 가지 유형이 존재한다.

**Recreate Strategy(재생성 전략)**

배포된 웹 응용 프로그램 인스턴스 복제본이 5개라고 가정하자. 이것들을 새로운 버전으로 업그레이드하는 한 가지 방법은 이것들을 모두 제거하고 응용 프로그램 인스턴스의 새로운 버전을 만드는 것이다.

하지만 이러한 방법의 문제는 구 버전이 다운되고 새 버전이 업되기 전 기간동안 응용 프로그램은 다운되고 사용자 접근이 불가능해진다.

이러한 전략은 재생성(Recreate) 전략이라고 부르고, 기본 배포 전략은 아니다.

**Rolling Update Strategy(롤링 업데이트 전략)**

두 번째 전략은 한꺼번에 다 없애지 않는 전략이다. 그 대신에 구 버전을 하나씩 내리고 새 버전을 하나씩 올리는 것이다.

이렇게 하면 앱이 다운되지 않고 업그레이드도 수월하게 이뤄질 것이다. 배포를 생성할 때 전략을 따로 지정하지 않는다면 이 전략을 기본으로 사용하게 된다.

## kubectl apply

파일을 사용해서 업데이트를 한다면 보통 apply 명령어를 사용해서 다음과 같이 쓴다.

```shell
kubectl apply -f deployment-definition.yml
```

하지만 다음과 같이 도커 이미지만 바꿀 수 있는 방법도 존재한다.

```shell
kubectl set image deployment/myapp-deployment nginx-container=nginx:1.9.1
```

### Upgrades

새 배포가 생성되면 예를들어 복제본 5개를 배포한다고 할 때 먼저 자동으로 ReplicaSet을 생성한다. 이에 따라서 복제본 개수에 부합하는 Pod 개수가 생성되게 된다.

앱을 업그레이드 할 때도 마찬가지로 기존의 ReplicaSet 이외에 새로운 ReplicaSet을 생성하게 된다. 그런다음 업그레이드가 진행될 때 이전 ReplicaSet의 Pod 들을 하나씩 다운시키고, 새로 생긴 ReplicaSet 에다가 새로운 Pod을 생성하게 된다.

### Rollback

앱을 업그레이드헀는데 뭔가 잘못됨을 알게 될때 발생하는 이벤트이다. 업그레이드할 때 사용하는 새 버전 빌드에 문제가 생기는 상황인 것이다.

이런 상황이 발생하면 쿠버네티스 배치는 이전 Revision으로 되돌려 업그레이드를 취소하게 한다.

수동으로 롤백하는 방법은 다음과 같은 명령어를 입력하면 된다.

```shell
kubectl rollout undo deployment/myapp-deployment
```
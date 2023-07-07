# Node Affinity

Node Selector 속성의 목적은 Pod이 특정 노드에 호스팅될 수 있도록 하는 것이다.

하지만 Node Selector의 속성만으로는 복잡한 기준의 용량을 가진 Pod을 스케줄링하기가 어렵다는 단점이 존재한다.

Node Affinity는 이러한 단점을 극복하고 복잡한 기준에 따라서 Pod을 스케줄링 할 수 있는 고급 기능을 제공한다.

다음 Node Affinity 속성을 사용한 yml파일을 보자.

```yml
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
spec:
  containers:
    - name: data-processor
      image: data-processor
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
          - matchExpressions:
            - key: size
              operator: In
              values:
                - Large
                - Medium
```

operator에는 In, NotIn, Exists 등 다양한 것들이 존재한다.

Node Affinity 타입에는 두 가지 유형이 존재한다.

1. requiredDuringSchedulingIgnoredDuringExecution
2. preferredDuringSchedulingIgnoredDuringExecution

**DuringScheduling**

Pod이 존재하지 않다가 처음으로 만들어지는 상태를 말한다. Pod이 생성되면 지정된 선호도 규칙에 따라 Pod을 올바른 Node에 배치하는게 고려된다.

**required**

필수 유형을 선택하면 스케줄러는 지정된 선호도 규칙과 함께 Pod을 Node에 놓도록 지시한다. 일치하는 Node를 찾지 못하면 Pod은 붕 뜨게 된다.

**preferred**

선호 모드로 설정하면 일치하는 Node가 없을 경우 스케줄러는 Node 선호도 규칙을 무시하고 해당 Pod을 모든 가능한 Node에 배치한다.

즉, 조건을 만족하는 Node를 찾고 없다면 배치가 가능한 다른 Node에 스케줄링한다.

**IgnoredDuringExecution**

Pod이 실행되고 환경이 변하면서 Node의 친화성에 영향을 미친다. Node의 label 변경처럼 말이다. 예를들어 관리자가 size=Large 라벨을 Node에서 제거했다고 가정해보자. 그렇다면 Node에서 실행중인 Pod들은 어떻게 되는 것일까?

Ignore옵션이 앞에 붙으면 Pod은 계속 실행되고 Node 선호도는 이미 스케줄링이 되면 영향을 미치지 않는다.

**RequiredExecution**

새로운 옵션으로는 Required도 있는데 이는 반대의 상황이 되어버린다. 즉, 이미 스케줄링된 Pod도 떨어져 나가게 된다.
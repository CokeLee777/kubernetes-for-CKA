# Node Selectors

쿠버네티스 클러스터에는 최소 1개부터 시작해서 여러 개의 워커노드가 존재할 수 있다.

또한 워커노드마다 정해진 용량이 존재하고 용량이 작은것부터 큰것까지 다양하게 존재할 수 있다. 따라서 Pod들을 효율적으로 배치하기 위한 방법이 필요한데, 이 때 Node Selector를 사용하면 된다.

Node Selector를 Pod에 정의하면 Pod에 정의한 Node Selector에 해당하는 Node에 배치되게 된다.

다음은 Pod을 정의한 yml 파일에 Node Selector를 정의하는 방법이다.

```yml
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
spec:
  containers:
    - name: data-processor
      image: data-processor
  nodeSelector:
    size: Large
```

위 Node Selector의 Key: Value는 특정 Node에 붙어있는 Label이다.

특정 Node에 Label을 붙이는 방법은 다음과 같다.

```shell
kubectl label node <Node 이름> <label-key>=<label-value>
```

**예시 1**

```shell
kubectl label node node01 size=Large
```
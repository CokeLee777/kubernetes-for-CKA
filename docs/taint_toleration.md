# Taint & Toleration

- taint: Node마다 설정 가능하다. 설정한 Node에는 Pod이 스케줄되지 않는다.
- toleration: taint를 무시할 수 있다.

주로 노드를 지정된 역할만 하게할 때 사용한다.

예를 들어 gpu가 있는 노드에는 다른 Pod들은 올라가지 않고 gpu를 쓰는 Pod들만 올라가게 하는 등의 상황에 사용할 수 있다.

taint에 사용할 수 있는 옵션에는 3가지가 존재한다.

1. NoSchedule: toleration이 없으면 Pod이 스케줄되지 않는다. **기존에 실행되던 Pod에는 적용되지 않는다.**
2. PreferNoSchedule: toleration이 없으면 Pod을 스케줄링 안하려고 하지만 필수는 아니다. 클러스터내의 자원이 부족하거나 하면 taint가 걸려있는 Node에서도 Pod이 스케줄링될 수 있다.
3. NoExecute: toleration이 없으면 Pod이 스케줄되지 않으며 **기존에 실행되던 Pod도 toleration이 없으면 종료시킨다.**

**특정 Toleration만 허용하는 Taint effect를 Node에 지정**

```shell
kubectl taint nodes <Node 이름> <Label의 Key>=<Label의 Value>:<taint-effect>
```

**특정 Node의 특정 taint 제거**

```shell
kubectl taint nodes <Node 이름> <Label의 Key>=<Label의 Value>:<taint-effect>-
```

**toleration을 Pod에 적용 - yml**

모든 속성은 항상 쌍따옴표로 지정해야 한다!

```yml
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
spec:
  containers:
    - name: nginx-container
      image: nginx
  tolerations:
    - key: app
      operator: "Equal"
      value: "blue"
      effect: "NoSchedule"
```
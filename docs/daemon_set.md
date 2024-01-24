# DaemonSet

데몬셋은 ReplicaSet과 Deployment와 같이 복제본의 집합인데, 다만 이와 다른점은 복제본들이 각각 다른 노드에 배치되도록 하게 해준다.

즉, 모든 노드에서 데몬셋에 해당하는 Pod이 하나씩 생성된다. 노드가 죽더라도 해당 노드에서 실행중인 Pod이 다른 Node로 옮겨가지 않고, 제거된 Node와 함께 Pod이 단순히 제거된다.

## DaemonSet의 스펙

```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
    name: monitoring-daemon
spec:
    selector:
        matchLabels:
            app: monitoring-agent
    template:
        metadata:
            labels:
                app: monitoring-agent
        spec:
            containers:
                - name: monitoring-agent
                  image: monitoring-agent
```
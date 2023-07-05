# Namespaces

쿠버네티스 클러스터 내의 논리적인 분리 단위이다.

왜 namespace가 필요할까? 쿠버네티스 오브젝트에는 Pod 이외에 다양한 리소스들이 있다. 그렇기 때문에 비슷한 이름의 개체들이 수 없이 많이 생길 것이고, 쿠버네티스 환경 운영자와 사용자는 관리와 사용 측면에서 어려움을 겪게 될 것이다.

그래서 쿠버네티스에서는 namespace라는 것을 제공한다. 다시 말하면 namespace는 쿠버네티스 클러스터 내의 논리적인 분리 단위이다. 클러스터 내의 오브젝트들을 namespace를 통해 논리적으로 분리시키는 것이다.

물리적으로 분리하는 것은 아니므로 클러스터 자체가 다운되면 모든 namespace가 타격을 입는다.

다음은 namespace를 yml 파일로 정의한 예시이다.

```yml
apiVersion: v1
kind: Namespace
metadata:
  name: dev
```

또한 커맨드에서 직접 namespace를 만들수도 있다.

```shell
kubectl create namespace dev
```

리소스들을 얻을 때 네임스페이스별로 조회가 가능하다.

```shell
kubectl get pods --namespace=dev
```

네임스페이스를 따로 입력하지 않으면 default namespace를 조회한다.

```shell
kubectl get pods
```

모든 네임스페이스에 있는 모든 Pod을 나열할 수도 있다.

```shell
kubectl get pods --all-namespace=true
```

## Resource Quota

네임스페이스별로 리소스 한도를 정의할 수도 있다.

Resource Quota를 정의한 파일 예시는 다음과 같다.

```yml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: compute-quota
  namespace: dev
spec:
  hard:
    pods: "10"
    requests.cpu: "4"
    requests.memory: 5Gi
    limits.cpu: "10"
    limits.memory: 10Gi
```
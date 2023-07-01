# YAML in Kubernetes

쿠버네티스 정의 파일은 다음과 같이 항상 4개의 최상위 필드를 포함한다.

```yml
apiVersion:
kind:
metadata:

spec:
```

이 4개의 속성들은 루트 레벨 속성들이고, 쿠버네티스 정의에 있어서 반드시 필요하다.

**apiVersion**

- 쿠버네티스 API 버전 정보

```yml
apiVersion: v1
```

**kind**

- 만드려는 객체의 유형
- Pod, Deployment, Service 등이 존재

```yml
kind: Pod
```

**metadata**

- label 같은 개체에 대한 데이터를 의미
- yml 문법의 Dictionary 형태
- label 하위의 키-값 쌍은 커스텀하게 지정 가능(그 이외의 메타데이터는 불가능)

```yml
metadata:
  name: myapp-pod
  labels:
    app: myapp
    type: front-end
```

**spec**

- 만들 개체의 사양을 의미
- 추가 정보를 제공

```yml
spec:
  containers:
    - name: nginx-container
      image: nginx
```

## Run YAML in kubernetes file

**yml 파일 기반으로 실행**

```shell
kubectl create -f pod-definition.yml
```

**yml 파일 기반으로 종료**

```shell
kubectl delete -f pod-definition.yml
```

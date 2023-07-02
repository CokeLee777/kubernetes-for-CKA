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

**배포 조회 명령어**

```shell
kubectl get deployments
```

**클러스터에 생성된 모든 개체(deployment, replicaset, pods 등) 조회 명령어**

```shell
kubectl get all
```

replica set은 deployment과 큰 차이가 없다. deployment가 쿠버네티스 오브젝트를 deployment라고 만든것 뿐이다.


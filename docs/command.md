# kubectl Command

**Pod 생성 명령어**

```shell
kubectl run <Pod 이름> --image=<image 이름>
```

**특정 Pod을 Service를 이용해서 배포**

```shell
kubectl expose pod <Pod 이름> --port=<Service Port> --name=<Service 이름>
```

**Deployment 생성**

```shell
kubectl create deployment <Deployment 이름> --image=<image 이름> --replicas=<복제본 개수>
```

**Pod을 생성하면서 동시에 같은 이름의 Service 생성 후 배포**

```shell
kubectl run <Pod 이름> --image=<image 이름> --port=<port> --expose=true
```
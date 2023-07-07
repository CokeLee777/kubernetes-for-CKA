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

**실제로 리소스를 제어하지 않으면서 제어 가능한지 확인**

```shell
kubectl run nginx --image=nginx --dry-run=client
```

**실제로 리소스를 제어하지 않으면서 제어 가능한지 확인하고 생성된 리소스의 yaml 정보 조회**

```shell
kubectl run nginx --image=nginx --dry-run=client -o yaml
```

**실제로 리소스를 제어하지 않으면서 제어 가능한지 확인하고 생성된 리소스의 yaml 파일 생성**

```shell
kubectl run nginx --image=nginx --dry-run=client -o yaml > example.yaml
```
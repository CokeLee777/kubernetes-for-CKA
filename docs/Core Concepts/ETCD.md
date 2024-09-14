# Installing ETCD

ETCD 설치는 다음과 같이 ETCD 바이너리 파일을 다운로드 받고 실행하면 끝이다.

1. 바이너리 파일 다운로드

```shell
curl -L https://github.com/etcd-io/etcd/releases/download/v3.3.11/etcd-v3.3.11-linux-amd64.tar.gz -o etcd-v3.3.11-linux-amd64.tar.gz
```

2. 압축풀기

```shell
tar xzvf etcd-v3.3.11-linux-amd64.tar.gz
```

3. ETCD 서버 실행

```shell
./etcd
```

ETCD 서버를 실행하면 기본적으로 ETCD 서버는 **2379** 포트를 사용하여 실행한다.
# Operate ETCD

- 데이터 세팅

```shell
./etcdctl set key1 value1
```

- 데이터 조회
```shell
./etcdctl get key1
```
# ETCD Versions

ETCD 버전은 다음과 같이 업데이트 되어왔다.

- v0.1[2013.08]
- v0.5[2014.12]
- v2.0[2015.02]
- v3.1[2017.01]
- CNCF에 의해 관리[2018.11]

가장 큰 변화는 v2.0과 v3.0 사이에 일어났다. v2.0은 가장 많이 사용된 버전이고, v3.0은 가장 많은 변화가 일어난 버전이다. API 버전 또한 v2.0과 v3.0 사이에 변경되었다. 즉, ETCDCTL 명령어가 많이 변화되었다.

여기서 주의해야할 점은 ETCD 서버와 ETCD API 버전이 다를 수 있다는 것이다. 기본적으로 ETCDCTL의 API 버전은 2이다.

```shell
./etcdctl --version
```

위 명령어를 치면 etcdctl 의 버전과 API 버전이 나오는데, 두 개의 버전이 하나는 2 또 다른 하나는 3 일수도 있다는 것이다. etcdctl 명령어를 치기 전에 버전을 확인하고 내가 원하는 버전이 맞는지 확인 후 실행하는 것을 추천한다.

만약 현재 etcdctl 버전이 3이고 API 버전이 2 인데, API 버전을 3으로 명령어를 활용하고 싶다면 다음과 같이 ENV를 줘서 처리할 수 있다.

```shell
ETCDCTL_API=3 ./etcdctl version
```

하지만 위와 같이 하면 명령어를 칠 때마다 ENV를 명시해야 한다는 단점이 존재한다. 따라서 다음과 같이 export 명령어를 사용하여 환경변수를 지정하여 해당 세션동안 API 버전을 유지하고 명령어를 사용할 수 있다.

```shell
export ETCDCTL_API=3 ./etcdctl version
```
# ETCD in Kubernetes

`ETCD` 라는 **Key-Value 데이터 저장소**는 클러스터에 관한 정보를 저장한다. 예를들어 Nodes, Pods, Configs, Secrets, Accounts, Roles, Bindings, Others 와 같은 것들을 말이다.

`kubectl` 명령어를 실행할 때마다 얻게 되는 모든 정보는 `ETCD` Server에서 얻게된다.

Node 추가, Pod 배포, ReplicaSet 생성 등 모든 **클러스터에 변화를 주는 행위는 ECTD Server에 업데이트가 된다.**

> ETCD Server에 업데이트가 되어야만 특정 행위가 완료된 것으로 간주된다.

클러스터를 어떻게 설정하느냐에 따라서 ECTD는 다양하게 배포된다. 직접 배포하는 방법이랑 kubeadm 도구를 이용한 배포 방법이 존재한다.

**직접 배포**하는 방법은 다음과 같이 직접 다운로드해서 배포한다.

```shell
wget -q -https-only \
  "https://github.com/coreos/etcd/releases/download/v3.3.9/etcd-v3.3.9-linux-amd64.tar.gz"
```

kubeadm을 이용해서 클러스터를 설정하면 kubeadm이 ETCD Server를 배포한다. 다음과 같이 etcd-master의 이름이 붙어있는 Pod으로 배포한 것을 볼 수 있다.

```shell
kubectl get pods -n kube-system
```

**쿠버네티스가 저장한 모든 키를 열거**하려면 다음과 같은 명령어를 사용하면 된다.

```shell
kubectl exec etcd-master –n kube-system etcdctl get / --prefix –keys-only
```

고가용성 환경에서는 클러스터에는 여러 개의 마스터 노드가 존재할 것이다. 이 때 ETCD Server도 여러 개가 존재하게 된다.
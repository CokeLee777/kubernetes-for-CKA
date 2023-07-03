# Services

쿠버네티스 서비스는 애플리케이션 안팎의 다양한 구성요소간의 통신을 가능하도록 해준다.

또한 애플리케이션을 다른 애플리케이션 또는 사용자와 연결하는 데 도와준다.

예를들어 백엔드와 프론트엔드 간의 통신을 돕고, 외부 데이터 소스와의 연결을 설정하는데 도와준다.

## Services Types

**NodePort**

서비스가 노드의 포트에 내부 포트를 액세스할 수 있도록 해준다.

Node의 Port와 Pod의 Port를 매핑함으로써 이뤄지게 된다.

매핑이 이루어지는 원리는 TargetPort라고 불리는 Pod의 Port와 Service의 Port가 매핑이 된다. 그리고 Service의 Port와 NodePort라고 불리는 Node의 Portrk 매핑이 되고, 이 NodePort는 외부 웹 서버에 액세스할 때 사용된다.

아래의 yml 파일에서 ports 속성을 보면 targetPort는 Pod의 Port이고, port는 Service의 Port, nodePort는 Node의 Port이다.

```yml
apiVersion: v1
kind: Service
metadata:
  name: myapp-service
spec:
  type: NodePort
  ports:
    - targetPort: 80
      port: 80
      nodePort: 30008
  selector:
    app: myapp
    type: front-end
```

**서비스 생성 명령어**

```shell
kubectl create -f service-definition.yml
```

**서비스 조회 명령어**

```shell
kubectl get services
```

**ClusterIP**

이 경우 서비스는 클러스터 내부에 가상 IP를 만들어 Node 간의 통신을 가능하게끔 해준다.

즉, 내부 통신을 위한 서비스이다.

그렇다면 각각의 Node 자체의 IP를 쓰면 되지 않냐고 의문을 제기할 수도 있다. 하지만 Replica Set을 적용하거나 Node가 다운되었다가 다시 업되면 IP가 계속 바뀌게 된다. 또한 복제본들은 각각 다른 IP주소를 가지고있기 때문에 한쪽으로만 트래픽이 쏠리게 된다.

ClusterIP는 이러한 문제점을 해결하기 위해서 같은 Pod들을 하나의 인터페이스로 묶어주고, 요청은 무작위로 한 Pod으로 전달되게끔 해준다. 또한 ClusterIP 서비스의 이름으로 통신이 가능하게끔 해주어 IP를 몰라도 상관없게 된다.

```yml
apiVersion: v1
kind: Service
metadata:
  name: back-end
spec:
  type: ClusterIP
  ports:
    - targetPort: 80
      port: 80
  selector:
    app: myapp
    type: back-end
```

**LoadBalancer**

이 유형은 부하 분산기로 지원되는 클라우드 공급자에서 우리의 응용 프로그램을 위해 부하 분산기를 프로비전한다.

이 서비스는 특정한 클라우드 플랫폼에서만 지원되므로 주의하도록 하자.

```yml
apiVersion: v1
kind: Service
metadata:
  name: myapp-service
spec:
  type: LoadBalancer
  ports:
    - targetPort: 80
      port: 80
      nodePort: 30008
```
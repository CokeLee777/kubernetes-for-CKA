쿠버네티스는 사용자 계정을 직접 관리하지 않는다.
대신에 세부정보, 인증서가 있는 파일, LDAP 같은 타사 ID 서비스와 같은 외부 소스에 의해 관리된다.
따라서 쿠버네티스에서 사용자를 생성하거나 사용자 목록을 조회할 수 없다.

하지만 서비스 계정(Service Accounts)는 쿠버네티스가 관리할 수 있다.
아래와 같이 쿠버네티스 API를 이용하여 서비스 계정(Service Accounts)을 생성하고 관리할 수 있다.
```shell
kubectl create serviceaccount sa1
```

```shell
kubectl get serviceaccount
```
## Accounts

- 모든 사용자 액세스는 kube-apiserver에 의해 관리된다.
- kubectl을 사용하든 curl 명령어로 직접 명령을 내리든 상관없이 모든 요청은 kube-apiserver로 간다.
- kube-apiserver는 요청을 처리하기 전에 사용자를 인증하는 과정을 먼저 처리한다.
## Auth Mechanisms

- 그렇다면 kube-apiserver는 어떻게 인증을 할까?
- 인증 메커니즘은 다음과 같이 다양하다.
	- Username, Password 목록들이 존재하는 정적 파일
		- csv 파일에 Username, Password를 저장하여 사용자 정보의 데이터소스로 사용이 가능하다.
		- 파일은 세 개의 열이 존재하는데 각각 Password, Username, User ID 로 구성된다.
		- 이렇게 데이터 소스를 구성한 다음 kube-apiserver의 옵션으로 다음과 같이 부여하면 된다.
			- --basic-auth-file=user-details.csv
			- kube-apiserver를 정적 pod으로 기동했고 kubeadm을 사용한다면 '/etc/kubernetes/manifests/kube-apiserver.yaml' 파일내의 command에 옵션을 부여하면 된다.
			- 이렇게 옵션을 붙이고 kube-apiserver를 재시작하면 적용된다.
	- Username, Token 목록들이 존재하는 정적 파일
		- 토큰 방식도 위와 동일하게 Password 대신에 토큰을 넣으면 된다.
		- 그렇게 구성한 다음 kube-apiserver 옵션으로 다음과 같이 부여하면 된다.
			- --token-auth-file=user-token-details.csv
	- Certificates 
	- Identity Services
### 주의사항

- Basic Authentication mechanism은 추천하지 않는 인증 방식이다.
- RBAC Authorization 방식을 새로운 사용자에게 부여하는것을 추천한다.
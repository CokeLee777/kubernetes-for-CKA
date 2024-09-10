# Secure Kubernetes

- kube-apiserver는 쿠버네티스내의 모든 동작의 중심에 위치한다.
- kubectl 혹은 kube-apiserver에 직접 액세스하여 클러스터내의 거의 모든 작업을 수행할 수 있다.
- 따라서 kube-apiserver 자체에 대한 액세스를 제어하여 1차적으로 보안을 강화할 수 있다.
## Authentication

누가 액세스가 가능한가?
액세스할 수 있는 방법은 다음과 같이 다양하다.

- Username, Password(아이디, 비밀번호)
- Username, Token(아이디, 토큰)
- Certificates(인증서)
- External Authentication providers(외부 인증 공급자) - LDAP
- Service Accounts(서비스 계정)
## Authorization

인증받은 사용자는 뭘 할 수 있는가?
다음과 같은 인가 방식이 존재한다.

- RBAC(Role Based Access Control) Authorization(역할기반 액세스 제어 인가)
- ABAC(Attribute Based Access Control) Authorization(행동기반 액세스 제어 인가)
- Node Authorization(노드 인가)
- Webhook Mode(웹훅)
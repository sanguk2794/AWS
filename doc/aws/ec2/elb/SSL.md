## SSL/TLS
### 1. SSL/TLS - Basics
- An SSL Certificate allows traffic between your clients and your load balancer to be encrypted in transit (in-flight encryption)  
→ `SSL, Secure Sockets Layer` 인증서를 이용하면 클라이언트와 로드 밸런서 사이에서 트래픽이 이동하는 동안 암호화된다. 이를 전송 중 암호화라고 한다.
암호화된 요청은 송신자와 수신자 측에서만 복호화할 수 있다.

- SSL refers to Secure Sockets Layer, used to encrypt connections  
→ `SSL`은 연결을 암호화하는데 사용화한다.

- TLS refers to Transport Layer Security, which is a newer version  
→ `TLS, Transport Layer Security` 새로운 버전의 `SSL`이다.

- Nowadays, TLS certificates are mainly used, but people still refer as SSL  
→ 최근에는 `TLS` 인증서가 주로 사용되는데, 여전히 `SSL`이라고 부른다.

- Public SSL certificates are issued by Certificate Authorities (CA)  
→ 공공 인증서는 `CA, Certificate Authorities`라 불리는 인증기관에서 발급한다.

- CA → Comodo, Symantec, GoDaddy, GlobalSign, Digicert, Letsencrypt, etc…

- SSL certificates have an expiration date (you set) and must be renewed  
→ `SSL` 인증서에는 만료 날짜가 있어서 주기적으로 갱신해 인증 상태를 유지해야만 한다.

### 2. Load Balancer - SSL Certificates
- `SSL`은 로드 밸런서에서 다음과 같은 순서로 동작한다.
~~~
1. 사용자가 HTTPS를 통해 접속한다. HTTP에 S가 붙은 것은 SSL 인증서를 써서 암호화했기에 안전하다는 뜻을 내포한다.
2. 인터넷을 통해 로드 밸런서에 접속하면 로드 밸런서에서는 내부적으로 SSL Termination을 수행한다.
3. 백엔드에서는 HTTP로 EC2 인스턴스와 통신한다. 하지만 VPC로 이동하는 트래픽은 프라이빗 네트워크를 쓰기 때문에 안전하게 보호된다.
~~~

![image](https://user-images.githubusercontent.com/97398071/233822532-4c14bdfa-be8b-4c28-a0ff-2993bde8f98f.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- The load balancer uses an X.509 certificate (SSL/TLS server certificate)  
→ 로드 밸런서는 `X.509` 인증서를 사용하는데, 이걸 `SSL` 또는 `TLS` 서버 인증서라고 부른다.

- You can manage certificates using ACM (AWS Certificate Manager)  
→ `AWS`에서는 이 인증서들을 관리할 수 있는 `ACM, AWS Certificate Manager`이 있다.

- You can create upload your own certificates alternatively  
→ 원할 경우 가지고 있는 인증서를 `ACM`에 업로드할 수 있다.

- HTTPS listener:
→ `HTTP` 리스너를 구성할 때에는 반드시 `HTTPS` 리스너를 설정해야 한다.
~~~
- You must specify a default certificate  
→ 기본 인증서를 지정해주어야 한다. 

- You can add an optional list of certs to support multiple domains  
→ 다중 도메인을 지원하기 위해 다른 인증서를 추가할 수도 있다.

- Clients can use SNI (Server Name Indication) to specify the hostname they reach  
→ 클라이언트는 `SNI, Server Name Indication`라는 것을 사용해 접속할 호스트의 이름을 알릴 수 있다.

- Ability to specify a security policy to support older versions of SSL / TLS (legacy clients)  
→ 원하는 대로 보안 정책을 지정할 수 있다. 구 버전의 `SSL`과 `TLS`, 즉 레거시 클라이언트를 지원할 수도 있다.
~~~
 
### 3. SSL – Server Name Indication (SNI)
- SNI solves the problem of loading multiple SSL certificates onto one web server (to serve multiple websites)  
→ `SNI`는 여러 개의 `SSL` 인증서를 하나의 웹 서버에 로드해 하나의 웹 서버가 여러 개의 웹 사이트를 지원할 수 있게 해 준다.

- It’s a “newer” protocol, and requires the client to indicate the hostname of the target server in the initial SSL handshake  
→ `SNI`는 확장된 프로토콜로 클라이언트가 대상 서버의 호스트 이름을 지정하도록 한다. 이는 최초의 `SSL handshake` 단계이다.

- The server will then find the correct certificate, or return the default one  
→ 확장된 프로토콜로 새로 추가된 기능이기 때문에 모든 클라이언트가 지원하지는 않는다.
 
- Only works for ALB & NLB (newer generation), CloudFront, Does not work for CLB (older gen)  
→ `ALB`, `NLB`, `CloudFront`에서만 동작한다. `CLB`에서는 동작하지 않는다. 
그렇기에 어떤 로드 밸런서에 `SSL` 인증서가 여러개라면 `ALB`와 `NLB` 중 하나이다.

#### 4. Elastic Load Balancers – SSL Certificates
- `ELB`의 종류별로 `SSL` 지원이 다르다.
- Classic Load Balancer (v1)  
→ 지원하기는 하지만, `SSL` 인증서를 하나만 둘 수 있다.
~~~
- Support only one SSL certificate
- Must use multiple CLB for multiple hostname with multiple SSL certificates
~~~

- Application Load Balancer, Network Load Balancer (v2)  
→ 여러 개의 `SSL` 인증서를 두고 리스너를 여러 개 지원할 수 있다. 인증서를 여러 개 지원하기 위해서는 `SNI`를 사용해야 한다.
~~~
- Supports multiple listeners with multiple SSL certificates
- Uses Server Name Indication (SNI) to make it work
~~~

- `EC2` → `Load Balancing` → `Load Balancers` → `ALB` or `NLB` 인스턴스  클릭 → `Listeners` → `Add Listener`  
→ `ALB`의 경우 프로토콜을 `HTTPS`로, 포트번호를 `HTTPS`의 기본값인 443로 설정한다. `NLB`의 경우 프로토콜을 `TLS`로, 포트번호를 `TLS`의 기본값인 443로 설정한다.
해당 설정 값과 일치하는 요청이 왔을 경우 요청은 특정 `Target group`으로 전달된다.

- `Secure listener settings` 설정에서 `SSL` 보안 정책을 설정할 수 있는데, 인증서가 어떤 방식을 쓰게 할 것인지 정하는 것이다.
필요에 따라 구 버전의 `SSL`이나 `TLS` 등과 호환되게 할 수 있다.

- 인증서를 `ACM`에서 가져오거나, 개인키, 인증서, 인증서 체인을 입력해 외부에서 가져올 수 있다. 외부에서 가져올 경우 인증서가 `ACM`으로 직접 들어간다.

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---

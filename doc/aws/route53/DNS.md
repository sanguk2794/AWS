## DNS
### 1. What is DNS?
- Domain Name System which translates the human friendly hostnames into the machine IP addresses  
→ `DNS`는 사람들에게 친숙한 호스트 이름을 대상 서버 `IP` 주소로 번역해 준다.
~~~
www.google.com => 172.217.18.36
~~~

- DNS is the backbone of the Internet  
→ `DNS`는 인터넷의 중추로 `URL`과 호스트 이름을 `IP`로 변환하는 방법이다.

- DNS uses hierarchical naming structure  
→ `DNS`에는 계층적 이름 구조가 있다. 이를 `도메인 네임 스페이스, Domain name space`라고 한다.

### 2. DNS Terminologies
- Domain Registrar: Amazon Route 53, GoDaddy, …  
→ 도메인 레지스트라는 도메인 이름을 등록하는 곳이다. 아마존 `Route 53`, `GoDaddy` 등 여러 레지스트라가 존재한다.

- DNS Records: A, AAAA, CNAME, NS, …
- Zone File: contains DNS records  
→ 모든 `DNS` 레코드를 포함한다. 호스트 이름과 `IP` 또는 주소를 일치시키는 방법이다.

- Name Server: resolves DNS queries (Authoritative or Non-Authoritative)  
→ 이름 서버는 `DNS` 쿼리를 실제로 해결하는 서버이다. 

- Top Level Domain (TLD): .com, .us, .in, .gov, .org, …
- Second Level Domain (SLD): amazon.com, google.com, …

![image](https://user-images.githubusercontent.com/97398071/234050072-be3832e0-c8ce-4af7-807c-cf5951b18e96.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

### 3. How DNS Works
- `IPv4 Address = 9.10.11.12`인 웹 서버가 있다고 가정한다. 이를 도메인 이름 `example.com`을 이용해서 접근하고자 한다. 이를 위해서는 `example.com`라는 도메인 이름을 `DNS`용 서버에 등록해야 한다.  

- 클라이언트는 웹 서버에 접속하기 위해 로컬 `DNS` 서버에 `example.com`를 아는지 물어본다. 이 로컬 `DNS` 서버는 보통 회사에 의해 할당되고 관리된다.

- 로컬 서버가 이 도메인을 본 적이 없다면 `ICANN`에 의해 관리되는 `DNS` 서버의 루트에 이 도메인을 물어본다. 이 루트 `DNS` 서버는 해당 서버룰 모르기에 근접한 도메인인 `.com`의 주소를 반환한다.

- 이를 받은 로컬 `DNS` 도메인은 `TLD DNS` 서버에 쿼리의 답을 요청한다. 이는 `IANA`에 의해 관리되는 또 다른 도메인이다.

- 다음 질문을 할 서버는 서브도메인 `DNS` 서버이다. 이는 `Route 53` 등 도메인 이름 레지스트라에 의해 관리되는 서버이다. 이 `DNS` 서버는 `example.com`이 무엇인지 알고 있고, 그 결과값을 로컬 `DNS` 서버에 반환한다.

- 이렇게 `DNS` 서버에 반복적으로 물어보며 가장 구체적인 것을 찾아낸다. 찾아낸 값은 로컬 `DNS` 서버에서 캐시한다.

![image](https://user-images.githubusercontent.com/97398071/234052721-32d01ff4-1875-4b66-bf4c-55c82f92b4ae.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

---
#### ▶ Reference
- [DNS (Domain Name System)란?](https://peemangit.tistory.com/52)
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---
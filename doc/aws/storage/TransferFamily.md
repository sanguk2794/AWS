## AWS Transfer Family
### 1. AWS Transfer Family
- A fully-managed service for file transfers into and out of Amazon S3 or Amazon EFS using the FTP protocol  
→ `Amazon S3` 또는 `EFS` 안팎으로 데이터를 전송할 때 `S3 APi`나 `Amazon EFS`를 사용하지 않고 `FTP` 프로토콜을 통해 전송하고 싶은 경우 사용된다.

- Supported Protocols  
→ `FTPS`, `SFTP`는 전송 중 암호화가 이루어진다.
~~~
- AWS Transfer for FTP (File Transfer Protocol (FTP))
- AWS Transfer for FTPS (File Transfer Protocol over SSL (FTPS))
- AWS Transfer for SFTP (Secure File Transfer Protocol (SFTP))
~~~

- Managed infrastructure, Scalable, Reliable, Highly Available (multi-AZ)  
→ 전송 제품군은 완전 관리형 인프라이며 확장성과 안정성, 가용성이 뛰어나다.

- Pay per provisioned endpoint per hour + data transfers in GB  
→ 시간당 프로비저닝된 엔드 포인트 비용 + 전송된 용량 당 비용

- Store and manage users’ credentials within the service  
→ 서비스 내에서 사용자 자격 증명을 저장, 및 관리할 수 있다.

- Integrate with existing authentication systems (Microsoft Active Directory, LDAP, Okta, Amazon Cognito, custom)  
→ `Microsoft Active Directory`, `LDAP`, `Okta`, `Amazon Cognito`, `custo` 등 기존의 인증 시스템과 통합할 수 있다.

- Usage: sharing files, public datasets, CRM, ERP, …  
→ `S3`나 `EFS`의 `FTP` 인터페이스를 추가하기 위해 사용한다.

![image](https://user-images.githubusercontent.com/97398071/235355158-407429aa-de91-4cff-ae95-44d1549caa36.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

- 다음 중 `AWS` 전송 제품군이 지원하지 않는 프로토콜은 무엇인가요?  
→ 전송 층 보안(TLS)

- 한 머신 러닝 업체가 `S3` 버킷에 호스팅된 데이터 세트를 기반으로 연구를 진행하고 있습니다. 업체는 다른 사람들이 연구에 활용할 수 있도록 데이터 세트 중 일부를 공개하기로 결정했지만, S3 버킷 자체를 퍼블릭으로 설정하는 건 원치 않습니다. 또한 데이트 세트는 `FTP` 프로토콜로 접근할 수 있어야 합니다. 최소한의 노력으로 이 요구 사항을 충족하려면 어떻게 해야 합니까?  
→ `AWS Transfer Family`를 사용

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---

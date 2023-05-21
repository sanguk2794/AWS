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

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---

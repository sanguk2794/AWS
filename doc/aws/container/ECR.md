## Amazon ECR
### 1. Amazon ECR
- ECR = Elastic Container Registry  
→ `ECR`은 `Elastic Container Registry`의 약자이다.

- Store and manage Docker images on AWS  
→ `AWS`에 도커 이미지를 저장하고 관리하는 데 사용된다.

- Private and Public repository (Amazon ECR Public Gallery https://gallery.ecr.aws)  
→ `ECR`에는 두 가지 옵션이 있다. 특정 계정들에 한해 이미지를 공개하도록 프라이빗으로 게시하거나, `Amazon ECR Public Gallery`에 퍼블릭으로 게시할 수 있다.

- Fully integrated with ECS, backed by Amazon S3  
→ `ECR`은 `Amazon ECS`와 완전히 통합되어 있고, 이미지는 백그라운드에서 `Amazon S3`에 저장된다.

- Access is controlled through IAM (permission errors => policy)  
→ `ECR` 저장소에 여러 도커 이미지가 있다. 이 이미지를 `ECS` 클러스터의 `EC2` 인스턴스에 이미지를 끌어오기 위해서는 `EC2` 인스턴스에 `IAM` 역할을 지정해야 한다. `ECR`에 대한 모든 접근은 `IAM`이 보호하고 있다. `ECR`에 권한 에러가 생긴다면 정책을 살펴봐야 한다.

- Supports image vulnerability scanning, versioning, image tags, image lifecycle, …  
→ `Amazon ECR`은 단순히 저장하는 레포지토리에서 그치지 않는다. 이미지의 취약점 스캐닝, 버저닝 태그 및 수명 주기 설정을 지원한다.

- 인스턴스를 끌어오는데 성공한다면 컨테이너가 시작된다.  

![image](https://user-images.githubusercontent.com/97398071/235710052-45d35282-ead8-4400-ab81-69416bb3cd63.png)

출처 → [AWS Certified Solutions Architect Slides v10](https://courses.datacumulus.com/downloads/certified-solutions-architect-pn9/)

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---
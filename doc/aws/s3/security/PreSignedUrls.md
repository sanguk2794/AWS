## Pre-Signed URLs
### 1. Amazon S3 – Pre-Signed URLs
• Generate pre-signed URLs using the S3 Console, AWS CLI or SDK  
→ `미리 서명된 URL`은 `S3` 콘솔, `CLI`, `SDK`를 사용하여 생성할 수 있는 `URL`이다. 

• URL Expiration  
→ `URL`에는 만료 기간이 있다. 
~~~
• S3 Console – 1 min up to 720 mins (12 hours)
→ S3 콘솔을 사용하면 최대 12시간이다.

• AWS CLI – configure expiration with --expires-in parameter in seconds (default 3600 secs, max. 604800 secs ~ 168 hours)
→ CLI를 사용하면 최대 168시간이다.
~~~

• 객체 선택 → `Object actions` → `Share with a presigned URL` 작업을 통해 `미리 서명된 URL`을 생성할 수 있다.

• Users given a pre-signed URL inherit the permissions of the user that generated the URL for GET / PUT  
→ `미리 서명된 URL`은 `URL`을 받는 사용자와 생성한 사용자의 `GET / PUT` 권한을 상속한다.

• Examples:  
→ AWS 외부의 사용자에게 한 파일에 대한 권한을 부여해야 할 경우
~~~
• Allow only logged-in users to download a premium video from your S3 bucket
→ 인증받은 사용자만 동영상을 다운로드받을 수 있게 허용한다.

• Allow an ever-changing list of users to download files by generating URLs dynamically
→ 다운로드 가능한 사용자 목록이 계속 변하는 경우에는 `URL`을 동적으로 생성해서 다운로드 할 수 있게 한다.

• Allow temporarily a user to upload a file to a precise location in your S3 bucket
→ 또는 일시적으로 사용자가 S3 버킷의 특정한 위치에 파일을 업로드하도록 허용할 수 있다.
~~~

• 증가하는 연합 사용자들(Federated Users)에게 임시 `URL`을 제공해 해당 사용자들이 `S3` 버킷의 특정 위치로 파일을 업로드할 수 있게 하려고 합니다. 어떤 기능을 사용해야 합니까?  
→ `미리 서명된 URL`

### 2. Pre-Signed URLs Scenario
• AWS 외부의 사용자에게 한 파일에 대한 권한을 부여할 때, 보안 문제를 해결하기 위해 버킷을 `private`로 생성한다.

• 버킷 소유자는 해당 파일의 `미리 서명된 URL`을 생성한다.

• 그러면 `S3` 버킷이 `URL`을 제공하고 해당 `URL`은 미리 서명된다.

• `URL`이 자격 증명을 이어받아 해당 파일에 액세스할 수 있는 권한을 부여한다.

• `미리 생성된 URL`을 사용자에게 보내면 사용자는 해당 `URL`을 통해 `S3` 버킷의 파일에 액세스할 수 있다. 만료 기간이 끝나기 전까지는 자유롭게 해당 파일에 접근이 가능하다.

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---

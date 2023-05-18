## MFA Delete
### 1. Amazon S3 – MFA Delete
- MFA (Multi-Factor Authentication) – force users to generate a code on a device (usually a mobile phone or hardware) before doing important operations on S3  
→ `Amazon S3`에서 중요한 작업을 수행하기 전 `MFA` 인증을 요구할 수 있다.

- MFA will be required to:
~~~
- Permanently delete an object version
→ 객체 버전을 영구적으로 삭제할 때 필요하다. 영구 삭제에 대한 보호 설정이다.

- Suspend Versioning on the bucket
→ 버킷에서 버저닝을 중단할 때 필요하다.
~~~

- MFA won’t be required to:
~~~
- Enable Versioning
→ 버저닝을 활성화할 때에는 필요하지 않다.

- List deleted versions
→ 삭제한 버전을 나열하는 작업에는 필요하지 않다.
~~~

- To use MFA Delete, Versioning must be enabled on the bucket  
→ `MFA Delete`를 사용하려면 먼저 버킷에서 버저닝을 활성화해야 한다. 버저닝과 관련되어 있기 때문이다.

- Only the bucket owner (root account) can enable/disable MFA Delete  
→ 버킷 소유자, 즉 루트 계정만이 `MFA Delete`를 활성화하거나 비활성화할 수 있다.

- `SDK` 혹은 `CLI`를 통해서만 설정 가능하다. `AWS` 콘솔에서는 설정이 불가능하다.
~~~ shell
# generate root access keys
aws configure --profile root-mfa-delete-demo

# enable mfa delete
aws s3api put-bucket-versioning --bucket [your bucket name] --versioning-configuration Status=Enabled,MFADelete=Enabled --mfa "[arn-of-mfa-device] [mfa-code]" --profile [your profile name]

# disable mfa delete
aws s3api put-bucket-versioning --bucket [your bucket name] --versioning-configuration Status=Enabled,MFADelete=Disabled --mfa "[arn-of-mfa-device] [mfa-code]" --profile [your profile name]
~~~

---
#### ▶ Reference
- [Ultimate AWS Certified Solutions Architect Associate SAA-C03](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/)
---
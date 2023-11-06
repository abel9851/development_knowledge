ACL(Access Contorl List)와 관련이 깊은 듯하다.
현재 AWS가 추천하는 것은 ownership은 사용하고 ACLs은 disabled하는 것이다.
s3 object ownership은 bucket에 추가된 objects의 ownership을 컨트롤하는 설정이다.

owner enforced setting을 사용하면, ACLs은 disabled되고 기존의 object와 새롭게 추가되는 object를 소유하게 된다.

ACLs를 enabled하는 것은 AWS가 추천하지 않는 방식이기 때문에 작성하지 않는다.

결과적으로, objects에 대한 제어는 policy에 기반하게 된다.
IAM policies, s3 bucket polcies, VPC endpoint polcies, Organizations SCPs 등 말이다.

bucket owner enforced setting를 설정하는 것은, s3 versioning을 사용하고 있을 때에 새로운 version을 추가하거나 하지는 않는다.

Reference
https://docs.aws.amazon.com/AmazonS3/latest/userguide/about-object-ownership.html
https://docs.aws.amazon.com/AmazonS3/latest/userguide/access-control-overview.html
https://docs.aws.amazon.com/AmazonS3/latest/userguide/configuring-block-public-access-account.html

Link
[[Managing access with ACLs]]

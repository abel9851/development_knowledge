
ACLs은 리소스 기반 옵션이다. 버킷과 오브젝트의 access를 관리한다.
특히 다른 account가 현재 내 account의 버킷과 오브젝트를 read 및 write를 하고 싶을 때 사용했었는데
(어카운트 내의 IAM user를 위한 설정이 아니다.)
현재로서는 ownership을 통해 관리하는게 주류이다.(AWS 공식문서에서도 추천하고 있다.)
ownership을 설정하고 ACLs는 disabled하는게 default다.



Reference
https://docs.aws.amazon.com/AmazonS3/latest/userguide/acls.html
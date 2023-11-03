ACL 자체의 기능은 access를 허용하는 것만 하나보다.
ACL이 부여된 bucket이나 object는 public access가 되는 것 같다.

1. block public access permissions to buckets and objects granted through new access control lists
   새롭게 추가된 버킷과 또는 오브젝트에 적용되는 public access 권한을 차단한다. 그리고 존재하는 버킷과 오브젝트에 대해서 새로운 public access ACL의 생성을 막는다. 단 이 설정은 ACL를 사용하는 s3 리소스에 public access를허용하는 기존의 권하는 변경하지 않는다.
2. block public access to bukets and objects granted through any access control lists
   버킷과 오브젝트에 대해 public access를 부여하는 모든 ACL를 차단한다.
3. block public access to buckets and objects granted through new public bucket or access point policies
   버킷과 오브젝트에 대해 public access를 부여하는 새로운 버킷 및 acccess point policies를 차단한다.
   단 이 설정은 s3 리소스에 public access를 허용하는 기존의 policy는 변경하지 않는다.
   
4. block public and cress-account access to buckets and objects through any public bucket or access point policies

정리하자면, 4개의 설정은, public access 권한을 막고, 모든 ACL(기존포함)을 차단하고, 버킷 및 오브젝트에 대해 public access를 부여하는 새로운 버킷과 access point policies를 차단하고, 버킷과 오브젝트에 대해 public access를 부여하는 policies를 가진 access points와 버킷에 대한 cross account, public을 무시하도록 하는 설정이다.


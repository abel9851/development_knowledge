resource로 vpc를 만드는 것 외에도 공식모듈을 사용해서 만드는 게 가능하다.

```tf

module "vpc" {
source = "terraform-aws-modules/vpc/aws"
version = "5.1.2"
}

```

서브넷 이름은 같아도 된다. subnet-3292g0edede48aed38 / test-prod-private처럼 이름 앞에 서브넷 번호는 유니크하다.

- enable_dns_hostnames
  기본값은 false다.
  vpc가 public ip addresses가 있는 인스턴스들에게 public DNS hostnames를 할당하는 옵션이다. 
  public DNS hostname은 사람이 읽을 수 있는 네트워크 리소스의 이름이다.
  이것이 있으면 vpc 안에 인스턴스는 public DNS hostname을 통해 이터넷과 통신할 수 있고 인터넷에서도 이러한 DNS 호스트 네임을 통해 인스턴스에 접근할 수 있다. (private ip address와 private DNS hostnames(vpc 안에서 사용하는)는 이 옵션과 무관하게 부여된다.) 

  ex. - Private DNS Hostname:`ip-172-31-0-1.ec2.internal` ,Public DNS Hostname (만약 퍼블릭 IP가 있다면): `ec2-54-204-255-255.compute-1.amazonaws.com`
  
- enable_dns_support
  기본값은 true다.
  이 속성이 활성화되면, Amazon의 DNS 서버가 VPC의 DNS 이름 해석을 지원하게 된다.
  VPC 내의 인스턴스가 퍼블릭 DNS 이름을 사용하여 다른 AWS 서비스와 통신할 수 있다.
  이 설정이 true여야 enable_dns_hostnames 옵션을 true로 설정할 수 있다.
  
  

- enable_nat_gateway
- single_nat_gateway
- one_nat_gateway

위의 3개의 옵션은, 상관관계가 있으므로 설정할 때 공식문서의 nat gateway scenario를 보자.
실제 테스트해본 바로는,
single nat gateway일 경우, public subnets 블록에 있는 첫번째 public subnet에 할당된다. 
10.0.1.0/24에 있을 거다. aws console에 확인하자.
확인해보니까 정말로 public subnet 한곳에 nat gateway가 생성되어 있었으므로 one_nat_gateway_per_az는 false로 해야했다.


Reference
https://registry.terraform.io/modules/terraform-aws-modules/vpc/aws/latest
https://registry.terraform.io/modules/terraform-aws-modules/vpc/aws/latest#nat-gateway-scenarios
https://docs.aws.amazon.com/vpc/latest/userguide/vpc-dns.html#vpc-dns-hostnames

링크
[[NAT gateway는 private subnet 개수별로 필요한가]]
[[aws NAT gateway가 eip를 갖아야하는 이유]]

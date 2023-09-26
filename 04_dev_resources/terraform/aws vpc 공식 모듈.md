resource로 vpc를 만드는 것 외에도 공식모듈을 사용해서 만드는 게 가능하다.

```hcl

module "vpc" {
source = "terraform-aws-modules/vpc/aws"
version = "5.1.2"
}

```

Reference
https://registry.terraform.io/modules/terraform-aws-modules/vpc/aws/latest
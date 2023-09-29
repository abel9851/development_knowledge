
```
terraform {
    required_version = "~> 1.2"

    required_providers {
        aws = {
            source = "hashicorp/aws"
            version = "~> 4"
        }
    }
}


provider "aws" {
    region = local.region
    
    default_tags {
        tags = {
            environment = var.env
            product = local.product
        }
    }
}

```

terraform 블록은 사용할 terraform cli의 버전과 프로바이더의 출처와 버전을 제어한다.

required_provider는 프로바이더의 출처와 버전을 제어한 블록이다.

provider 블록은 사용할 provider의 구체적인 설정을 정의한다.

```

provider "docker" {
	registry_auth {
		address = "${data.aws_caller_identity.aws_account_info.account_id}.dkr.ecr.${local.region}.amazonaws.com"
		username = data.aws_ecr_authorization_token.token.user_name
		password =  data.aws_ecr_authorization_token.token.password
	}
}

data "aws_ecr_authorization_token" "token" {}


```

terraform에서 도커를 사용하기 위해 설정한 프로바이더.
registry_auth에는 도커 프로바이더의 인증정보를 설정한다.

인증정보로 사용된 ecr의 token은 autorization token, proxy endpoint, token, expiration date, user name, password를 ECR repository에서 얻을 수 있도록 해준다.

user_name, password는 token으로부터 decode된 value다.



Reference
https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/ecr_authorization_token
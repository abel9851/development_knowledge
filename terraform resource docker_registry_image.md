레지스트리 안에서 이미지의 생명주기를 관리한다.
레지스트리에 이미지를 등록하거나 삭제하는 리소스다.

aws ecr과 같이 사용할 경우, 레지스트리 안에 레포지토리를 미리 만들어놔야한다.

- bing 검색 결과
terraform의 resource인 Resource (docker_registry_image)와 provider에서 aws의 ecr을 지정했을 경우, ecr안에 있는 repository는 docker_registry_image에서 지정한 name에 repository주소를 확인해서 자동으로 등록하는거야? provider "docker" { registry_auth { address = "${data.aws_caller_identity.aws_account_info.account_id}.dkr.ecr.${local.region}.amazonaws.com" username = data.aws_ecr_authorization_token.token.user_name password = data.aws_ecr_authorization_token.token.password } }


[Terraform의 `docker_registry_image` 리소스를 사용하면 Docker 이미지를 빌드하고 레지스트리에 푸시할 수 있습니다](https://registry.terraform.io/providers/kreuzwerker/docker/latest/docs/resources/registry_image)[1](https://registry.terraform.io/providers/kreuzwerker/docker/latest/docs/resources/registry_image). [그러나 이 리소스는 이미지를 빌드하고 레지스트리에 푸시하는 것만 관리하며, ECR 내의 리포지토리를 자동으로 생성하거나 관리하지는 않습니다](https://registry.terraform.io/providers/kreuzwerker/docker/latest/docs/resources/registry_image)[1](https://registry.terraform.io/providers/kreuzwerker/docker/latest/docs/resources/registry_image).

[ECR 리포지토리를 생성하려면 별도로 `aws_ecr_repository` 리소스를 사용해야 합니다](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/ecr_repository)[2](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/ecr_repository). [이 리소스를 사용하면 ECR 내에 리포지토리를 생성하고 관리할 수 있습니다](https://registry.terraform.io/providers/kreuzwerker/docker/latest/docs/resources/registry_image)[2](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/ecr_repository).

따라서, `docker_registry_image`에서 지정한 이름으로 ECR 내의 리포지토리가 자동으로 등록되지는 않습니다. [먼저 `aws_ecr_repository`를 사용하여 리포지토리를 생성한 후, 해당 리포지토리의 URL을 `docker_registry_image`의 이름으로 사용해야 합니다](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/ecr_repository)[2](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/ecr_repository)[1](https://registry.terraform.io/providers/kreuzwerker/docker/latest/docs/resources/registry_image).

아래는 `aws_ecr_repository`와 `docker_registry_image`를 함께 사용하는 예입니다:

```hcl
resource "aws_ecr_repository" "repository" {
  name = "my-repository"
}

resource "docker_registry_image" "image" {
  name = "${aws_ecr_repository.repository.repository_url}:my-tag"
  build {
    context = "${path.module}/context"
  }
}
```

이 예제에서는 먼저 ECR 리포지토리를 생성하고, 그 다음에 Docker 이미지를 빌드하여 생성된 리포지토리에 푸시합니다. 이렇게 하면 `docker_registry_image`에서 지정한 이름이 ECR 리포지토리의 URL과 일치하게 됩니다.


Reference
https://registry.terraform.io/providers/kreuzwerker/docker/latest/docs/resources/registry_image
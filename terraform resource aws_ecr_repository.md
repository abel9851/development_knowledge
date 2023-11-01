elastic container registry 안에 repository를 생성한다.

- 옵션
	- force_delete
		- true라면, 컨테이너 이미지가 있더라도 repository를 삭제한다.
	- image_tag_mutability
		- repository 안에 이미 같은 태그를 가진 이미지가 있는 상황에서 이미지를 push할 경우, 오류가 반환된다. 기본 설정은 `"MUTABLE"` 이다.
	- image_scanning_configuration
		- 이미지의 취약점을 식별하는 스캔을 실시한다.
		- scan_on_push(필수)
			- 이미지가 push된 후, scan할지 안할지를 설정한다.

```terraform
resource "aws_ecr_repository" "foo" {
  name                 = "bar"
  image_tag_mutability = "MUTABLE"

  image_scanning_configuration {
    scan_on_push = true
  }
}
```

reference
https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/ecr_repository
https://docs.aws.amazon.com/ja_jp/AmazonECR/latest/userguide/image-tag-mutability.html
https://docs.aws.amazon.com/AmazonECR/latest/userguide/image-scanning.html



link
[[terraform resource docker_registry_image]]
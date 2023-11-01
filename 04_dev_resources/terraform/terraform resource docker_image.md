지정된 host에 docker registry로부터 image를 pull하는 리소스다.
`pull_triggers` 필드를 업데이트하는 data.docker_registry_image의 데이터와 함께 사용하지 않는 한 자동적으로 새로운 레이어를 pull하지는 않는다.

```terraform

data "docker_registry_image" "ubuntu" {
  name = "ubuntu:precise"
}

resource "docker_image" "ubuntu" {
  name          = data.docker_registry_image.ubuntu.name
  pull_triggers = [data.docker_registry_image.ubuntu.sha256_digest]
}
```

build 옵션을 사용하면 docker registry로부터 pull한 이미지를 사용해서 build를 할 수 있다.(Dockerfile도 지정가능)

```terraform
resource "docker_image" "zoo" {
  name = "zoo"
  build {
    context = "."
    tag     = ["zoo:develop"]
    build_arg = {
      foo : "zoo"
    }
    label = {
      author : "zoo"
    }
  }
}
```

Reference
https://registry.terraform.io/providers/kreuzwerker/docker/latest/docs/resources/image

link
[[terraform resource docker_image]]
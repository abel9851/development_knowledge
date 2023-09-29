도커 이미지를 도커 레지스트리에서 지정된 도커 호스트로 가져오는 리소스다.

이 리소스는 `pull_triggers` 필드를 업데이트하기 위해 도커 레지스트리 이미지 데이터 소스를 사용하지 않는한, 자동적으로 새로운 레이어를 pull하지는 않는다.


Reference
https://registry.terraform.io/providers/kreuzwerker/docker/latest/docs/resources/image
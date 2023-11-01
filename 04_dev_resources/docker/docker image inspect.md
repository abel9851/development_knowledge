
`docker image inspect -f '{{.Id}} nignx'`처럼 포맷을 지정하면 아래와 같이 결과가 나온다.
계층 형식으로 되어 있어 하위 정보 조회 시 .상위[.하위] 방식으로 조회. 

```bash
sha256:593aee2afb642798b83a85306d2625fd7f089c0a1242c7e75a237846d80aa2a0
```

`docker image inspect --format="{{ .ContainerConfig.Env }}" httpd`
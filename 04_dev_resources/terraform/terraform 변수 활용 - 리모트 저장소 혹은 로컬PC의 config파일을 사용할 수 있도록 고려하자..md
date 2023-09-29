중요한 정보가 들어있는 config파일을 S3에 저장하는 경우가 있는데 로컬 PC에서도 config 파일을 사용할 수 있도록 설계하는 게 좋다고 생각한다.
AWS의 가용성을 생각하면 굳이 로컬 PC에서 config 파일을 준비해서 할 필요가 있겠냐만은
만일의 대비하는 것과, 로컬PC에서 s3 bucket을 열람할 필요없이 config파일을 확인 할 수 있다는 이점이 있다고 생각한다.

```
variable "is_read_from_s3" {
	description = "whether config file is read from s3 or local pc"
	default = true
	type = boolean
}
```
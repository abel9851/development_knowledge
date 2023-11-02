ACM(AWS certificate Manager)으로부터 certificate의 arn을 도메인 별로 가져와서 사용할 수 있다.

```terraform
# Find a certificate that is issued
data "aws_acm_certificate" "issued" {
  domain   = "tf.example.com"
  statuses = ["ISSUED"]
}

# Find a certificate issued by (not imported into) ACM
data "aws_acm_certificate" "amazon_issued" {
  domain      = "tf.example.com"
  types       = ["AMAZON_ISSUED"]
  most_recent = true
}

# Find a RSA 4096 bit certificate
data "aws_acm_certificate" "rsa_4096" {
  domain    = "tf.example.com"
  key_types = ["RSA_4096"]
}
```

### 사용한 파라미터
- domain
	- 조회 할 인증서의 도메인이다. 없다면 에러를 반환한다.
- types
	- 반환된 목록을 필터링하기 위한 유형 목록이다. 실제 사용한것은 `AMAZON_ISSUED`다. 즉 amazon에서 발급했다는 뜻이다.
- most_recent
	- true라면 이전 기준에 일치하는 certificate를 `NotBefore` field로 정렬하여 가장 최근의 것을 리턴한다. false일 경우, 그리고 여러개의 certificate가 있다면, 에러를 리턴한다.
- provider

Reference
https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/acm_certificate
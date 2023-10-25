```

resource "aws_route53_record" "records" {
  for_each = var.test
  
  name    = join("", [each.value, var.name]) # name은 이미 variables.tf에서 지정됬다는 전제
  zone_id = var.route53_zone_id # route53_zone_id도 마찬가지.
  type    = "A"

  depends_on = [
    aws_cloudfront_distribution.cloudfront_instance
  ]
}


```

위의 코드에서 name에 도메인을 전부 지정해야할 것 같지만
zone_id로 hosted zone을 지정했으므로 name에는 도메인을 지정해줄 필요가 없다.

hosted_zone의 이름이 test라면, name에는 test를 빼고 작성하면 된다.
name이 예를 들어 apple이라면 apple.test로 test라는 hosted zone에 레코드가 생성된다.
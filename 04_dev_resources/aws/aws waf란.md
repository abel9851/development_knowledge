application load balancer, amazon api gateway api, cloudfront distribution을 향한 web request를
모니터링할 수 있게 해준다.

내가 정의한 condition으로 위의 resource들을  웹공격(web exploits [[why is mean web exploit to web attack]] )으로부터 보호할 수 있다.

장점으로는,
1. 웹 공격으로부터 빠르게 protection 가능
2. 배포가 쉽다.(cloudfront, application load balancer 등에 추가적인 설정 없이 바로 배포가능)
3. 트래픽을 가시화해서 문제가 있는 request들을 속아낼 수 있다.
4. managed rule을 통해서 시간 절약 가능. managed rule은 aws측에서 관리하고 업데이트 해준다.
   쉽게 application을 보호할 수 있는, aws측에서 준비해주는 waf의 부품이라고 생각하면 된다.
   managed rule 중에는 추가적으로 비용이 드는 rule도 있다.

waf에서 관건은 2개.
1. 적절한 rule을 설정하는 것
2. rule, condition에 matching된 request들을 logging할 것(ex. datadog에 aws kinesis firehose를 사용해서 스트리밍 데이터를 보낸다던가.)

사내에서 waf는, datadog와의 연계가 중요하다.

Reference
https://us-east-1.console.aws.amazon.com/wafv2/homev2/start?region=us-east-1
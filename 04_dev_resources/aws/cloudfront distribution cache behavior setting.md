
cache behvior setting을 통해 내 웹사이트의 파일에 대하여 pattern을 설정하고, 여러 기능을 사용할 수 있다.
기본적으로 유저로부터의 모든 request는 origin으로 전달되는데 이를 제어할 수 있다.

- pattern을  `*.jpg` 로 설정하면 내가 distribution에 지정한 origin으로부터의 file중에 jpg 파일만 대상으로 기능을 적용할 수 있다.
- 유저로부터의 request는 반드시 HTTPS여야 한다라든가
- 지정된 query string만 허용된다든가
- origin을 여러개 사용하고 있을 경우, 특정 origin에만 전달할 수 있게 한다던가
- signed URLS만 특정 파일에 접근할 수 있다던지
- origin에서 file에 설정한 `Cache-Control` header의 값과는 상관없이 cloudfront cache에 얼마나 머물게 할지, 최소 시간을 정한다든지


이러한 cache설정은 origin이 2개 있다면, origin만큼의 cache설정이 존재해야한다. origin이 2개고 cache 설정이 1개라면, 나머지 origin은 사용되지 않는다.
그리고 새롭게 추가한 cache설정은 맨마지막에 적용된다. 이는 aws console에서 확인할 수 있다.
cloudfront api를 사용할 경우, `DistributionConfig` 에 순서가 지정되어 있으므로 파일을 통해 확인하면 된다.
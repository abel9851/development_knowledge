
cloudfront는 동적 , 정적 컨텐츠를 빠르게 배포하게 해주는 웹서비스다.
개발자가 설정한 origin server(S3, Http server 등)로부터 content를 edge location(전세계에 있는 AWS의 data center)에 보존한 후, user의 request가 오면 edge location에 request를 라우팅하여 content를 제공한다.(content는 일반적으로 web page, image, media file등이다.)
공식문서에서는 lowest latency(time delay)를 자랑한다고 한다.

cloudfront를 사용하지 않을 경우, 인터넷에 연결된 여러 상호 네트워크의 집합을 거쳐서 유저에게 content가 전달되지만 aws는 aws backbone network를 별로도 사용해서 빠르게 cotent를 제공한다.

실제 사용방법은 아래와 같다.
1. origin server 설정(나같은 경우는 s3를 주로 이용한다.)
2. distribution 설정 - cloudfront의 entity인 distribution을 설정하는데 여기에는 어떤 origin server에서 content를 가져올지, 모든 리퀘스트를 logging할지, distribution이 생성된 뒤, 바로 배포를 할지, cotent가 edge location에 cache되는 기간은 어떻게 할지(default는 24t시간) 등등 상세설정을 할 수 있다.
3. cloudfronts,s distribution에 도메인을 할당한다. 대체 도메인을 커스터마이징 할 수도 있다. ex. `d111111abcdef8.cloudfront.net` 이 도메인은 단순히 이름뿐만이 아니라 배포되는 컨텐츠를 요청하는 창구인, 정말로 도메인을 뜻한다.
   이 부분은 보통 route53이나 다른 도메인 서비스을 이용해서 원하는 도메인을 설정한다.
5. cloudfront는 내가 설정한 distribution configuration을 모든 edge location 또는 points of presence(POPS-collections of servers in geographically)에 보낸다. (저곳들에 content의 복사본을 cache한다.)

Reference
https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/Introduction.html

Link
[[CreateDistribution]]
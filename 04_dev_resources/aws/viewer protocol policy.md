viewer란 request를 보내는 클라이언트, 일반적으로 브라우저를 뜻한다.
viewer protocol policy는 cloudfront edge location 안에 있는 컨텐츠에 접속하기 위해 어떤 procotocol policy를 사용하기를 원하는지를 설정하는 옵션이다.

viewer라는 개념은 viewer request(client로부터의 요청), origin request(cloudfront가 origin server에 보내는 요청), origin response(origin server가 cloudfront에 보내는 응답), viewer response(client에게 보내는 응답)에서도 쓰이는 용어니까 기억해두자.

- HTTP and HTTPS: 두개 다 사용
- Redirect HTTP to HTTPS: 2개 다 사용가능하지만 자동적으로 HTTP requests는 HTTPS requests로 리다이렉트된다.
- HTTPS Only: viewer는 HTTPS로만 contents에 접근 가능


Reference
https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/distribution-web-values-specify.html#DownloadDistValuesCacheBehavior
https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/viewers-reports.html
https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/lambda-cloudfront-trigger-events.html
https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/using-https-viewers-to-cloudfront.html
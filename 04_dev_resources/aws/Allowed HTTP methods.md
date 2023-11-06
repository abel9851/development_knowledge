cloudfront가 origin server로 전달하고 처리해주길 원하는 HTTP methods를 지정한다.
현재 사용하고 있는 인프라 아키텍처에서는 GET, HEAD만 사용하고 있다.
처음에는 웹애플리케이션에서 GET, POST, PUT, DELETE를 사용하고 있으니 cloudfront도 그렇게 해야하나 싶었다.
하지만 현재 사용하고 있는 vue3의 static file이나 bundling을 s3에 업로드한 다음에 유저가 get으로
front의 애플리케이션을 얻은 다음, POST, PUT, DELETE 등은 내부적으로 백엔드 서버에 전달되도록 되어 있기 때문에 cloudfront를 거치지 않는다.

- GET, HEAD
- GET, HEAD, OPTIONS
- GET, HEAD, OPTIONS, PUT, POST, PATCH, DELETE


Reference
https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/distribution-web-values-specify.html#DownloadDistValuesCacheBehavior
https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_AllowedMethods.html
https://dev.classmethod.jp/articles/amazon-cloudfront-post-pass-through/
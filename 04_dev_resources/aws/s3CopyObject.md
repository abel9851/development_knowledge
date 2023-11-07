destination bucket에는 list, put 권한이, source에서list, get권한이 필요하다.

CopyObject 자체는 s3:Getobject, Putboject action만 추가하면 사용 가능 하다.

Reference
https://repost.aws/ja/knowledge-center/s3-troubleshoot-copy-between-buckets
https://repost.aws/knowledge-center/s3-troubleshoot-copy-between-buckets
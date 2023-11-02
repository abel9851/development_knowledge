
`aws_iam_policy`를 사용해서  json format의 aws iam policy를 생성하는 리소스다.
즉, awsdml iam policy를 생성하는 것이 아니다.
`file` interpolation function을 사용해서 json file을 읽어도 되고 직접 리소스 안에 policy를 작성해도 된다.

Reference
https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/iam_policy_document

Link
[[terraform data aws_iam_policy_document와 resource aws_iam_policy 비교]]
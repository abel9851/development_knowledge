
bing 출처

Data Source: aws_iam_policy_document와 비슷한 resource도 있어?

lassmethod.jp/articles/writing-iam-policy-with-terraform/)[1](https://dev.classmethod.jp/articles/writing-iam-policy-with-terraform/)[`aws_iam_policy_document`는 데이터 소스로, IAM 정책 문서를 JSON 형식으로 생성하는 데 사용됩니다](https://dev.classmethod.jp/articles/writing-iam-policy-with-terraform/)[1](https://dev.classmethod.jp/articles/writing-iam-policy-with-terraform/). [이 데이터 소스는 Terraform 구성 내에서만 존재하며, 실제 AWS 리소스를 생성하지 않습니다](https://stackoverflow.com/questions/75665520/terraform-aws-iam-why-must-policy-documents-be-data)[2](https://stackoverflow.com/questions/75665520/terraform-aws-iam-why-must-policy-documents-be-data).

[다른 한편으로, `aws_iam_policy`와 같은 Terraform 리소스는 실제 AWS 리소스를 생성하거나 관리합니다](https://dev.classmethod.jp/articles/writing-iam-policy-with-terraform/)[1](https://dev.classmethod.jp/articles/writing-iam-policy-with-terraform/). [예를 들어, `aws_iam_policy` 리소스는 AWS에서 관리 정책을 생성합니다](https://stackoverflow.com/questions/44628380/terraform-assume-role-policy-similar-but-slightly-different-than-standard-ia)[3](https://stackoverflow.com/questions/44628380/terraform-assume-role-policy-similar-but-slightly-different-than-standard-ia).

따라서, `aws_iam_policy_document`와 같은 기능을 하는 Terraform 리소스는 없습니다. [그러나 `aws_iam_policy_document` 데이터 소스를 사용하여 생성된 IAM 정책 문서는 `aws_iam_policy`와 같은 리소스에서 사용될 수 있습니다](https://dev.classmethod.jp/articles/writing-iam-policy-with-terraform/)[1](https://dev.classmethod.jp/articles/writing-iam-policy-with-terraform/).

`aws_iam_policy`와 `aws_iam_policy_document`는 Terraform의 AWS IAM 관련 리소스로, 두 리소스는 서로 다른 목적과 동작 방식을 가지고 있습니다.

1. [**aws_iam_policy**](https://stackoverflow.com/questions/66510222/difference-between-aws-iam-policy-and-aws-iam-role-policy)[1](https://stackoverflow.com/questions/66510222/difference-between-aws-iam-policy-and-aws-iam-role-policy):
    
    - [이 리소스는 **관리 정책(managed policy)**을 생성합니다](https://stackoverflow.com/questions/66510222/difference-between-aws-iam-policy-and-aws-iam-role-policy)[1](https://stackoverflow.com/questions/66510222/difference-between-aws-iam-policy-and-aws-iam-role-policy).
    - [관리 정책은 재사용이 가능하며, 여러 IAM 엔티티(사용자, 그룹, 역할)에 연결할 수 있습니다](https://stackoverflow.com/questions/66510222/difference-between-aws-iam-policy-and-aws-iam-role-policy)[1](https://stackoverflow.com/questions/66510222/difference-between-aws-iam-policy-and-aws-iam-role-policy).
    - [이 리소스를 사용하여 생성된 정책은 AWS Management Console, AWS CLI, AWS API를 통해 확인하고 관리할 수 있습니다](https://stackoverflow.com/questions/66510222/difference-between-aws-iam-policy-and-aws-iam-role-policy)[1](https://stackoverflow.com/questions/66510222/difference-between-aws-iam-policy-and-aws-iam-role-policy).
2. [**aws_iam_policy_document**](https://runebook.dev/ja/docs/terraform/providers/aws/d/iam_policy_document)[2](https://runebook.dev/ja/docs/terraform/providers/aws/d/iam_policy_document):
    
    - [이 데이터 소스는 IAM 정책 문서를 JSON 형식으로 생성합니다](https://runebook.dev/ja/docs/terraform/providers/aws/d/iam_policy_document)[2](https://runebook.dev/ja/docs/terraform/providers/aws/d/iam_policy_document).
    - [이 데이터 소스는 IAM 정책 문서의 JSON 표현을 구축하기 위해 사용할 수 있으며, `aws_iam_policy` 리소스 등에서 정책 문서를 기대하는 리소스에서 사용됩니다](https://runebook.dev/ja/docs/terraform/providers/aws/d/iam_policy_document)[2](https://runebook.dev/ja/docs/terraform/providers/aws/d/iam_policy_document).
    - [이 데이터 소스를 사용하여 정책 문서를 생성하는 것은 선택 사항이며, 구성 내에서 리터럴 JSON 문자열을 사용하거나, 파일 보간 함수를 사용하여 원시 JSON 정책 문서를 파일에서 읽어올 수도 있습니다](https://runebook.dev/ja/docs/terraform/providers/aws/d/iam_policy_document)[2](https://runebook.dev/ja/docs/terraform/providers/aws/d/iam_policy_document).

따라서, `aws_iam_policy`는 재사용 가능한 관리 정책을 생성하는 데 사용되며, `aws_iam_policy_document`는 IAM 정책 문서를 생성하는 데 사용됩니다. 이 두 리소스는 서로 다른 목적과 동작 방식을 가지므로, Terraform 구성에 따라 적절하게 선택하여 사용해야 합니다.
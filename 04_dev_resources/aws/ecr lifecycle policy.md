
리포지토리에 등록되는 이미지의 생명주기를 관리한다.
lifecycle policy에 등록된 rule의 기준에 부합되는 이미지가 리포지토리에 등록될 경우,
이미지는 24시간 내에 만료될 것을 개발자는 예상해야한다.(삭제가 아니라 만료라고 되어 있다. 테스트는 아직 해보지 않았지만 이미지는 남되 사용할 수 없게 되지 않나 싶다.)

rule evaulator가 있어서 이 evaluator는 policy에 기록된 rule은 priority와는 상관없이 동시에 평가를 진행하며, 그 후 image를 rule의 priority에 따라 하나씩 적용한다.

lifecycle policy의 workflow는 아래와 같다.
1. rule을 생성한다.
2. rule을 저장하고  프리뷰를 실행한다.(프리뷰라고 한 이유는 실제 rule을 이미지에 적용하기 전에 미리 rule을 적용해보고 그 적용결과를 출력하기 때문인 것 같다.)
3. life cycle evaulator가 rule을 평가하고 rule의 영향을 받는 이미지를 표시한다.
4. file cycle evaluator가 rule priority에 기반하여 이미지에 rule을 적용하고 만료로 설정할 이미지를 보여준다.
5. 테스트(2,3,4번이라고 생각한다.)의 결과를 검토하고, 내가 의도한대로 만료로 설정될 이미지를 확인한다.
6. 리포지토리에 대한 lifecycle policy로 test rule을 적용한다. (위의 과정이 끝나고 실제 rule을 policy로 사용할 수 있도록 하는 것 같다.)
7. policy가 생성되면  개발자는 만료 기준에 부합하는 이미지가 24시간 내에 만료될 것을 예상해야한다.


- 사용해본 Lifecyle policy parameter
	- Rule priority
	- Description
	- Tag status
	- Count type
		- sinceImagePushed
		  Count number에 정의된 일수보다 older(오래되면)인 image는 만료된다.
	- Count unit
	  Count type이 sinceImagePushed일 경우 사용된다. 사용한 단위는 `days`다.
	- Count number
	- action
	  지원되는 value는 `expire`뿐이다.


개인적으로 눈여겨 본 설정은 tag status가 untagged인 rule은 policy에 하나만 생성할 수 있다는 부분이다.


아래는 aws의 공식문서에 기재된 policy template이다.
```json

{
    "rules": [
        {
            "rulePriority": integer,
            "description": "string",
            "selection": {
                "tagStatus": "tagged"|"untagged"|"any",
                "tagPrefixList": list<string>,
                "countType": "imageCountMoreThan"|"sinceImagePushed",
                "countUnit": "string",
                "countNumber": integer
            },
            "action": {
                "type": "expire"
            }
        }
    ]
}

```


Reference
https://docs.aws.amazon.com/AmazonECR/latest/userguide/LifecyclePolicies.html
https://docs.aws.amazon.com/AmazonECR/latest/userguide/lifecycle_policy_examples.html
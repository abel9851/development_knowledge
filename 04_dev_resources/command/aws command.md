커맨드를 외우려고 하지 마라.
커맨드가 무엇을 뜻하는지 이해하고 파이프처럼 연결이 무엇을 뜻하는지만 알아두면 된다.
* `aws ecr get-login-password --region $P{AWS_REGION} | docker login --username AWS --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com`
	aws ecr에서 로그인하고, 패스워드를 얻어서 standard input으로 도커 로그인을 할 때 얻은 패스워드를 aws의 ${AWS_ACCOUNT_ID}.dkr.ecr. ${AWS_REGION}.amazonaws.com에 사용한다.

- `aws ecs execute-command --cluster "iereach-prod-blue" --task "102c0fd708084e45a32a89a935b91cde" --container "api-blue" --interactive --command "/bin/bash"` 
 ecs task 안에 들어가서 커맨드를 실행하기 위해 cluster, task, 컨테이너를 지정하고 bash를 실행한다.
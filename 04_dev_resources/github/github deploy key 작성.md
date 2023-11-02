
deploy key를 사용하기 위해서는 SSH agent를 통해서 deploy key를 서버(혹은 deploy를 하기 위해 사용되는 docker container)안에 ssh key로 저장해야한다.
deploy key를 등록하기 전에 ssh-keygen으로 key를 생성해야한다.

실제 구현은 아래와 같다.(deploy를 하기 위한 전용 컨테이너를 사용하는 것을 전제로 한다.)
1. image를 만들 때 openssh, git, awscli, unzip을 install(unzip은 awscli를 설치하기 위해)
2. github deploy key를 github repository에서 사전에 만들어 놓고 deploy key value를 별도 장소에 보관
3. container에서 deploy key를 설정하고 source code를 pull하는 shell script를 준비
	1. shell script 안에서는 어딘가에 보관된 deploy key를 pull해서 컨테이너 안에서 id_rsa와 같은, ssh private key형식으로 만들어서 보관한다.
	2. 그 다음 git clone으로 source code를 container안에 다운로드한다.
	3. deploy관련 코드를 설정하고 실행한다.
4. shell script를 실행하는 CMD를 Dockerfile에 설정
5. container를 실행했을 때 shell script를 실행하는 것으로 deploy진행.


이때 사용되는 deploy key 관리는 아래의 URL이다.
https://docs.github.com/ko/authentication/connecting-to-github-with-ssh/managing-deploy-keys#deploy-keys

Reference
https://docs.github.com/ko/authentication/connecting-to-github-with-ssh/managing-deploy-keys
https://docs.github.com/ko/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent#generating-a-new-ssh-key
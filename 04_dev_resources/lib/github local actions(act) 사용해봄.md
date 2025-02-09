![[스크린샷 2025-02-09 23.46.06.png]]
- demo file로 github local action을 실행한 결과
    
    - 사용할 때 느낀점
        - 터미널의 cwd에 .github/workflows 디렉토리가 존재하지 않는다면 인식을 하지 못한다.
            
            - ex
                
                - private/github-actions-demo/.github/workflows path인 상황에서 터미널의 cwd가 private라면 인식을 하지 못한다.
            - 설정에서 수정가능한지 확인해봐야겠다.
                
                • 설정에서는 할 수 없더라.
                
        - mac에서 사용할 컨테이너의 프로그램이 amd64아키텍처라면 컨테이너를 create할 때 `—container-architecture linux/amd64` 로 지정해야하는데 이를 자동화 할수 있나?
            
            - cli로 act를 사용할 때에는 alias를 .zshrc에 등록해서 사용했었다.
            - 설정에서 act 커맨드를 `act --container-architecture linux/amd64` 로 변경하면 된다.
            ![[스크린샷 2025-02-09 23.57.42.png]]


Link
[[act]]

Reference
[https://github.com/cplee/github-actions-demo](https://github.com/cplee/github-actions-demo)
[https://nektosact.com/usage/index.html?highlight=-j#jobs](https://nektosact.com/usage/index.html?highlight=-j#jobs)
[https://stackoverflow.com/questions/37513511/whats-the-difference-between-the-docker-commands-run-build-and-create](https://stackoverflow.com/questions/37513511/whats-the-difference-between-the-docker-commands-run-build-and-create)

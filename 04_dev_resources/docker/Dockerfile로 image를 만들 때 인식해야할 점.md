Dockerfile로 이미지를 만들 때 아래와 같이 명령어를 설정할 경우, 
명령어는 컨테이너가 실행될 때 작동하므로 명령어를 실행하면서 그 명령어가
다른 파일을 실행하는 것이었을 경우, 컨테이너 생성은 성공하되 컨테이너 내부에서 에러가 발생하는 경우가 있다.

아래의 Dockerfile을 사용해서 이미지를 만들 때는 에러가 발생하지 않는다.
컨테이너를 생성된 이미지를 사용해서 생성할 경우, 그리고 아래의 `${GITHUB_REPO_NAME}` 인 환경변수가 지정되어 있지 않을 경우에는 컨테이너는 생성되지만 에러가 발생한다.

- Dockerfile
```Dockerfile

# COPY는 생
CMD ["/bin/deploy.sh"]

```

- 해당 shell script
```bash
git clone "git@github.com:${GITHUB_REPO_NAME}"
```
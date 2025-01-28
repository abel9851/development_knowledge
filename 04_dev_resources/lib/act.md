- mariaDB의 version upgrade를 한 후 github action을 사용해서 ci test를 했더니 mariaDB container가 build되지 않았다.
- yml에 `docker logs` 를 추가하거나 version을 예전 version으로 변경해서 로그를 출력하거나 다시 새로운 version으로 바꿔서 로그를 출력하거나 했었는데 이러한 테스트를 하기 위해 매번 pull request를 하는게 번거로웠다.
- 로컬 컴퓨터에서 github action을 실행할 수 있나 찾아봤더니 act라는 cli tool이 있었다.
- vscode의 extension으로도 사용가능하다.
- secrets, variable은 직접 입력이 가능하다.
	- variable은 적절한 권한이 있다는 전제하에 github에서 가져오기가 가능하다.
- 개인 프로젝트에서는 이미 사용하고 있다. 회사에서도 사용해야겠다.
	- 사용에 익숙해지면 사내발표를 하도록 한다.



Reference
[https://github.com/nektos/act?tab=readme-ov-file](https://github.com/nektos/act?tab=readme-ov-file)
[https://en.wikipedia.org/wiki/Make_(software)](https://en.wikipedia.org/wiki/Make_(software))
[https://sanjulaganepola.github.io/github-local-actions-docs/](https://sanjulaganepola.github.io/github-local-actions-docs/)
[https://nektosact.com/usage/runners.html](https://nektosact.com/usage/runners.html)
[https://docs.github.com/en/actions/using-github-hosted-runners/using-github-hosted-runners/about-github-hosted-runners](https://docs.github.com/en/actions/using-github-hosted-runners/using-github-hosted-runners/about-github-hosted-runners)
[https://github.com/cplee/github-actions-demo](https://github.com/cplee/github-actions-demo)
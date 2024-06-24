- `-n` option을 사용하면 [[ logical cpu ]]개수만큼의 process를 할당한다.
- logical cpu개수를 넘어서 설정하면 thread를 할당한다.
  thread에 관해서 global interpreter lock에 의해 process당 하나의 job만 사용할 수 있다고 알고 있는데
  어떤 원리도 multi threading을 하는지까지는 모르겠다. 조사가 필요하다.
- `-n auto`를 사용하면 pytest-xdist가 logical cpu개수를 자동적으로 감지하여 그 개수만큼의 process를 할당한다.
- `-n auto`로 설정된 process개수를 넘어서 `-n` 으로 process개수를 지정하면 multi threading이 가동되므로
  IO bound로 느려지는 테스트를 개선할 수 있겠지만 테스트 코드에 IO bound가 발생하는 곳이 없다면 CPU bound를 해결하는 multi processing만으로 충분하다.

Reference
[https://velog.io/@dahunyoo/파이썬의-병렬실행과-병렬테스트-Pytest-xdist](https://velog.io/@dahunyoo/%ED%8C%8C%EC%9D%B4%EC%8D%AC%EC%9D%98-%EB%B3%91%EB%A0%AC%EC%8B%A4%ED%96%89%EA%B3%BC-%EB%B3%91%EB%A0%AC%ED%85%8C%EC%8A%A4%ED%8A%B8-Pytest-xdist)
[https://velog.io/@sangyeon217/pytest-plugin-pytest-xdist](https://velog.io/@sangyeon217/pytest-plugin-pytest-xdist)
[https://pytest-xdist.readthedocs.io/en/stable/how-to.html#identifying-the-worker-process-during-a-test](https://pytest-xdist.readthedocs.io/en/stable/how-to.html#identifying-the-worker-process-during-a-test)
[https://blog.raccoony.dev/pytest-for-django-test/](https://blog.raccoony.dev/pytest-for-django-test/)
[https://qwlake.github.io/django/2020/06/05/building-django-test-environment/](https://qwlake.github.io/django/2020/06/05/building-django-test-environment/)
[https://blog.jerrycodes.com/pytest-split-and-github-actions/](https://blog.jerrycodes.com/pytest-split-and-github-actions/)
[https://pytest-xdist.readthedocs.io/en/stable/distribution.html#running-tests-across-multiple-cpus](https://pytest-xdist.readthedocs.io/en/stable/distribution.html#running-tests-across-multiple-cpus)
[https://qiita.com/yaboxi_/items/0cdc2818bf8acf6f00de](https://qiita.com/yaboxi_/items/0cdc2818bf8acf6f00de)
[https://docs.pytest.org/en/7.1.x/how-to/tmp_path.html#the-tmp-path-factory-fixture](https://docs.pytest.org/en/7.1.x/how-to/tmp_path.html#the-tmp-path-factory-fixture)
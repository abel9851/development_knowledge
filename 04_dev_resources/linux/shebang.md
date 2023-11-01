#!은 shebang이라고 하며, 이 라인은 해당 스크립트를 실행할 때 사용할 인터프리터를 지정한다.
`#! /usr/bin/env bash`는  시스템에 설치된 Bash 인터프리터의 위치를 환경변수를 통해 동적으로 찾아서 사용하도록 지시한다. env 명령어를 통해 bash의 위치를 찾는다.
이 방식을 사용하면 Bash가 `/bin/bash`나 `/usr/bin/bash` 또는 시스템의 다른 윛에 설치되어 있어도 스크립트가 올바르게 실행될 수 있다.

#(hash)와 !(bang)의 합성어다.

```bash
#!/usr/bin/env bash

```
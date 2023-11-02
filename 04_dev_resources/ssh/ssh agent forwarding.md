
ssh agent forwarding은 로컬에 있는 ssh key를 이용해 원격서버1에서 원격서버2로 ssh connection을 할 수 있는 기술이다.

우선은 로컬 컴퓨터에 ssh 개인 키와 원격서버에 접속을 실행하는 ssh client, 개인 키를 메모리에 보관하고 관리하는 ssh agent, 원격서버1, 2에는 ssh server가 설치되어 있어야한다.
그리고 `ssh-add` 커맨드를 통해서 ssh-agent의 개인키를 등록해야한다.

원리는 사용자가 로컬에서 원격서버1에 접속한 후, 다른 서버로 ssh 접속을 하는 것이다.

로컬에서 원격서버1에 접속을 할 때 ssh agent fowarding을 활성화하면 원격서버1에 ssh agent forwarding이 가능해진다. 원격서버1에 ssh agent와 ssh 개인 키가 없음에도 불구하고
원격서버1에서 원격서버2에 접속이 가능해진다.
원격서버2에 접속을 원격서버1에서 시도할 때, 원격서버1은 원격서버2로부터 auth request를 받았으면
로컬에 auth request를 하여 인증정보를 취득한다. 그 인증정보를 이용하여 원격서버2에 접속한다.

로컬과 원격서버1 사이에서는 ssh client가 원격서버1에 접속을 시도하고 원격서버1의 ssh server는 인증정보를 원격서버1에 요구한다. 이때 로컬 내부에서는 ssh client가 ssh agent에게 인증정보를 요구하고
ssh agent는 인증정보를 ssh client에 반환하고 ssh client는 원격서버1의 ssh server에 인증정보를 반환하는 것으로 원격접속이 가능해진다.


ssh agent fowarding을 사용하는 이유는 다음과 같다.
1. ssh agent를 한 곳에만 두고 여러 서버에 접속할 수 있다.
2. 개인 키를 원격서버에 두거나 네트워크를 통해서 전달할 필요가 없으므로 안전하다.

아래는 아키텍처다.

```plain text

[Local Computer]          [Remote Server 1]         [Remote Server 2]
        |                          |                          |
        |--- SSH Connection --->|                          |
        |                          |                          |
        |<-- Auth Request ---------|                          |
        |--- Auth Response ------->|                          |
        |                          |                          |
        |                          |--- SSH Connection --->|
        |                          |                          |
        |                          |<-- Auth Request -------|
        |<-- Forwarded Auth Request-|                          |
        |--- Forwarded Auth Response|                          |
        |                          |--- Auth Response ------->|
        |                          |                          |


```
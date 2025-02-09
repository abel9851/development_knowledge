# Docker & Linux 개념 정리

## 1. `mocha ./tests --recursive` 명령어
- **목적:** 테스트 실행
- **옵션:**
  - `./tests`: 테스트 디렉토리 지정
  - `--recursive`: 하위 디렉토리 포함

## 2. Node.js REPL 종료 방법
1. `.exit` 입력
2. **Ctrl + C** 두 번
3. **Ctrl + D** 한 번

## 3. 패키지 설명
| 패키지                  | 설명                                                                 |
|-------------------------|---------------------------------------------------------------------|
| `chai`                 | Assertion 라이브러리 (expect/should/assert 스타일)                  |
| `chai-http`            | HTTP API 테스트용 Chai 플러그인                                      |
| `eslint`               | JavaScript 코드 린터                                                |
| `eslint-config-airbnb` | Airbnb React 스타일 가이드                                          |
| `mocha`                | 테스트 프레임워크 (describe/it 구조)                                |

## 4. ARM64 vs AMD64
- **ARM64:** 모바일/맥용, 전력 효율 우수 (RISC 아키텍처)
- **AMD64:** 데스크톱/서버용, 고성능 (CISC 아키텍처)
- **호환성:** 서로 다른 아키텍처 바이너리는 에뮬레이션 필요 (QEMU, Rosetta 2)

## 5. 도커 명령어
- M1/M2 Mac 호환성 설정
	- act --container-architecture linux/amd64
- 컨테이너 생성 및 실행 예시
	- docker create --network host --platform linux/amd64
	- docker run -d --name my_container alpine tail -f /dev/null
	- docker exec my_container ls /

## 6. `/dev/null` 이해
- **특성:**
  - 쓰기: 데이터 즉시 삭제
  - 읽기: 항상 빈 값 반환
- **사용 사례:**
  - 컨테이너 무한 실행 유지 (`tail -f /dev/null`)
  - 불필요한 출력 무시 (`command > /dev/null`)

## 7. 네트워크 모드 (`--network host`)
- **동작:** 컨테이너가 호스트의 IP/포트 직접 사용
- **장점:** 로컬 서비스 접근 용이 (포트 포워딩 불필요)
- **예시:** 호스트의 MySQL에 `localhost:3306`로 접근 가능

---

> **참고:**  
> - 가상 장치(`/dev/null`)는 물리적 하드웨어 없이 커널 레벨에서 구현됨  
> - `docker exec`는 컨테이너 내부에서 명령 실행하지만, 터미널 세션은 호스트에 유지

### 테스트한 코드
```python
# main.py
from fastapi import FastAPI
from app.router import router

app = FastAPI()

app.include_router(router)
```

```python
# router.py
from fastapi import APIRouter
import time
import asyncio

router = APIRouter()

# [Case 1] 나쁜 예: async 함수 내에서 동기 함수(time.sleep) 사용
# -> 메인 스레드(Event Loop) 자체가 멈춰서 다른 요청도 못 받음.
@router.get("/blocking-sleep")
async def blocking_sleep():
    # Simulating a blocking synchronous operation
    print(">>> [Blocking] Start sleep")
    time.sleep(2) # 2초간 서버 전체가 멈춤!
    print(">>> [Blocking] End sleep")
    return {"type": "blocking", "duration": 2}

# [Case 2] 좋은 예: async 함수 내에서 비동기 함수(await asyncio.sleep) 사ㅛㅇ
# -> 대기하는 동안 제어권을 넘겨서 다른 요청(fast-ping)을 처리하게 함.
@router.get("/async-sleep")
async def async_sleep():
    print(">>> [Async] Start await")
    await asyncio.sleep(2) # 2초간 대기하지만, 제어권은 양보함.
    print(">>> [Async] End await")
    return {"type": "async", "duration": 2}

# [Probe] 테스트용 가벼운 요청
@router.get("/fast-ping")
async def fast_ping():
    print(">>> [Ping] 처리중")
    return {"message": "pong"}

```


```python
# test_concurrency.py
import pytest
import time
import asyncio
from httpx import AsyncClient, ASGITransport
from app.main import app

@pytest.mark.asyncio
async def test_blocking_vs_non_blocking():
    """
    시나리오:
    1. 무거운 요청(2초 소요)과 가벼운 요청(Ping)을 동시에 보낸다.
    2. 'Blocking' 상황에서는 Ping도 2초 뒤에 응답이 와야한다. (Fail) 이부분은 무거운 요청이 먼저 요청되어야한다는 조건이 필요하지 않나? 동시에 보내도 되나?
    gather에 적은 순서가 중요하다.
    3. 'Non-blocking' 상황에서는 Ping이 즉시(0.1초 이내) 응답이 와야 한다. (Success)
    """

    async with AsyncClient(transport=ASGITransport(app=app), base_url="http://test") as ac:

        # --- 실험 1: Event Loop가 차단되는 경우 (Blocking) ---
        print("\n\n--- [실험 1] Blocking (async def + time.sleep) ---")
        start_time = time.perf_counter()

        # # gather로 동시에 쏘지만, blocking_sleep이 loop를 잡고 안 놔줌
        # # asyncio.create_task를 사용하지 않은 이유는 두 개의 api요청이 모두 끝나는 시간을 정확하게 기록하기 위해서?
        # # 내가 이부분의 로직을 정확하게 모른다. create_task를 써도 될듯하면서도 gather가 안전하다는 근거없는 생각을 하고 있다.
        # res_block, res_fast = await asyncio.gather(
        #     ac.get("/blocking-sleep"),
        #     ac.get("/fast-ping")
        # )

        blocking_task = asyncio.create_task(ac.get("/blocking-sleep"))        
        await asyncio.sleep(0.1)
        await ac.get("/fast-ping")
        await blocking_task

        end_time = time.perf_counter()
        total_time = end_time - start_time

        print(f"Total Time: {total_time: .4f}s")
        # print(f"Block Response: {res_block.json()}")
        # print(f"Fast Response: {res_fast.json()}")

        # 검증: Event Loop가 막혔으므로 Fast 요청도 2초 가까이 걸려야함
        assert  total_time >= 2.0
        print("=> 결과: Fast요청도 Blocking 되어 늦게 처리됨 (의도된 나쁜 예시)")

        # --- 실험 2: Event Loop가 원활한 경우 (Non-Blocking) ---
        print("\n\n -- [실험 2] Non-blocking (async def + await asyncio.sleep) ---")
        start_time = time.perf_counter()
        
        # res_non_block, res_fast_2 = await asyncio.gather(
        #     ac.get("/async-sleep"),
        #     ac.get("fast-ping")
        # )

        non_blocking_task = asyncio.create_task(ac.get("/async-sleep"))        
        await asyncio.sleep(0.1)
        await ac.get("/fast-ping")
        await non_blocking_task

        end_time = time.perf_counter()
        total_time = end_time - start_time
    
        print(f"Total Time: {total_time: .4f}s")
        # print(f"Block Response: {res_non_block.json()}")
        # print(f"Fast Response: {res_fast_2.json()}")
        assert  total_time >= 2.0
        print("=> 결과: fast-ping이 중간에 처리된다. (좋은 예시)")
```

### 테스트 결과
- `create_task` 를 사용하던 `gather` 를 사용하던 시간차는 조금 있을지언정 출력되는 순서는 같았다.
- 즉, 비동기 안의 동기함수가 있다면, 그 동기함수처리가 끝날 때까지 다른 api의 리퀘스트를 처리하지 못한다.
```bash

================================== 1 passed in 4.15s ===================================
❯ uv run pytest -s
================================= test session starts ==================================
platform darwin -- Python 3.12.9, pytest-9.0.1, pluggy-1.6.0
rootdir: /Users/heejunshin/private/fastapi-sendbox
configfile: pyproject.toml
plugins: anyio-4.11.0, asyncio-1.3.0
asyncio: mode=Mode.STRICT, debug=False, asyncio_default_fixture_loop_scope=None, asyncio_default_test_loop_scope=function
collected 1 item                                                                       

test_concurrency.py 

--- [실험 1] Blocking (async def + time.sleep) ---
>>> [Blocking] Start sleep
>>> [Blocking] End sleep
>>> [Ping] 처리중
Total Time:  2.0064s
=> 결과: Fast요청도 Blocking 되어 늦게 처리됨 (의도된 나쁜 예시)


 -- [실험 2] Non-blocking (async def + await asyncio.sleep) ---
>>> [Async] Start await
>>> [Ping] 처리중
>>> [Async] End await
Total Time:  2.0017s
=> 결과: fast-ping이 중간에 처리된다. (좋은 예시)
.

================================== 1 passed in 4.14s ===================================

```


#python #비동기

## [ID: 202601220745] Timsort의 Natural Run 탐지 (Greedy Partitioning)

**태그:** #algorithm #timsort #adaptive-sort #greedy #linear-scan

### 본문

Timsort가 데이터 내에 이미 존재하는 정렬된 구간(**Run**)을 찾아내는 방식. 무작위 분할이 아닌 데이터의 '질서'를 이용하는 **적응형 정렬(Adaptive Sort)**의 핵심 기법이다.

1. **동작 원리 (Linear Scan):**
    
    - 배열을 한 번 훑으며($O(n)$) 인접한 두 원소의 대소 관계를 비교.
        
    - `arr[k] >= arr[k-1]`인 동안은 하나의 런(`new_run`)에 계속 포함시킴 (**Greedy**).
        
    - 흐름이 깨지는 순간(Else) 현재까지의 런을 저장하고 새로운 런을 시작함.
        
2. **구현 특징 (State Machine):**
    
    Python
    
    ```
    # 탐욕적 분할(Greedy Partitioning) 예시
    for k in range(1, len(arr)):
        if arr[k] >= arr[k-1]: 
            new_run.append(arr[k])
        else: 
            runs.append(new_run)
            new_run = [arr[k]] # 새로운 객체 생성(Rebinding)
    runs.append(new_run) # 루프 종료 후 마지막 런 처리
    ```
    
3. **엔지니어링적 이점:**
    
    - **최선 $O(n)$:** 이미 정렬된 배열이 들어올 경우 단 하나의 런만 생성되어 병합 비용이 0에 수렴함.
        
    - **정보 엔트로피 활용:** 무작위 데이터가 아닌 현실 세계의 '부분 정렬' 특성을 이용해 $n \log n$의 하한선을 물리적으로 우회하는 시도.
        

### 연결 (Links)

- **이전 개념:** [[202601220740-파이썬_빈_시퀀스_검사]]
    
- **상위 개념:** [[비교 기반 정렬의 하한선과 정보 이론]]
    
- **심화 개념:** [[Python의 객체 참조와 메모리 재할당 메커니즘]]
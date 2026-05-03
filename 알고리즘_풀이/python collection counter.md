## 1. 개요 (Overview)

`collections.Counter`는 해시 가능한 객체의 개수를 세기 위한 `dict`의 서브클래스이다. 리스트나 이터러블의 요소들을 **Key**로, 그 출현 빈도를 **Value**로 매핑한다.


```python
from collections import Counter

nums = [1, 1, 1, 2, 2, 3, 3, 3, 3]
count = Counter(nums)

# 결과: Counter({3: 4, 1: 3, 2: 2})
# Key: 원본 요소, Value: 빈도수
```

---

## 2. 핵심 메커니즘 & 성능 (Performance)

### C-Level 최적화

- **성능:** 일반적인 `for` 루프와 `dict.get()`을 사용하는 것보다 압도적으로 빠르다.
    
- **이유:** CPython에서 `Counter`는 내부적으로 **C 언어**로 작성된 최적화된 루프를 사용한다. Python 인터프리터가 매 루프마다 바이트코드를 해석하고 메서드 룩업(Method Lookup)을 수행하는 오버헤드를 제거했기 때문이다.
    

### 순서 (Ordering)

1. **시각적 출력 (`repr`):** `print(count)` 호출 시 빈도수가 높은 순서대로 보여준다.
    
2. **내부 저장:** Python 3.7+부터는 일반 `dict`와 동일하게 삽입 순서(Insertion Order)를 유지한다. `list(count.keys())` 호출 시 데이터가 처음 발견된 순서대로 나온다.
    

---

## 3. 주요 메서드 (Key Methods)

|**메서드**|**설명**|**시간 복잡도**|
|---|---|---|
|`Counter(iterable)`|요소 카운팅 및 객체 생성|$O(N)$|
|`most_common(k)`|빈도순 상위 $k$개 반환 (튜플 형태)|$O(N \log k)$|
|`elements()`|빈도만큼 요소를 반복하는 이터레이터 반환|$O(N)$|

---

## 4. 면접 전략 (Interview Strategy)

- **전문성 어필:** "표준 라이브러리를 활용해 생산성을 높이되, 내부적으로 C로 최적화되어 Python 루프보다 빠르다는 점을 인지하고 있음"을 언급한다.
    
- **커뮤니케이션:** "기본기 검증을 위해 `dict`로 직접 구현할까요, 아니면 생산성을 위해 `Counter`를 사용할까요?"라고 먼저 질문하여 주도권을 잡는다.
    
- **복잡도 언급:** `most_common(k)`가 내부적으로 **Heap**을 사용하여 $O(N \log k)$의 효율을 낸다는 점을 강조한다.
    

---

## 5. Implementation Code (Comparison)

Python

```
# 1. Manual Implementation (Python level)
count = {}
for n in nums:
    count[n] = count.get(n, 0) + 1

# 2. Recommended (C level)
from collections import Counter
count = Counter(nums)
top_k = [item[0] for item in count.most_common(k)]
```
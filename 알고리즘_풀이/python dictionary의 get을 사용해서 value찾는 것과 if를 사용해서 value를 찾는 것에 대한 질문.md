## 질문

하나 더 물을게.

```python

count = {}
for n in nums:
	count[n] = 1 + count.get(n, 0)

```

이 부분은 내가 해답을 보고 난 다음에 쓴 코드야. 그 전에는
```python

for n in nums:
	if n in count:
		count[n] += 1
	else:
		count[n] = 1
```


이렇게 작성했어.여기서 질문인데

1. dictionary는 if를 사용할 경우, set처럼 시간복잡도가 O(n)이야?

2. 첫번째 코드는 count.get(n, 0)을 사용하고 두번째는 if를 사용했어. 둘의 시간복잡도는 차이가 있어?


## 답변

결론부터 말씀드리면, 둘 다 시간 복잡도는 동일하지만 내부 동작 횟수에서 미세한 차이가 납니다.

1. Dictionary/Set의 조회 시간 복잡도
$O(1)$입니다. $O(N)$이 아닙니다.

Python의 dict와 set은 Hash Table을 사용하여 구현되어 있습니다.

if n in count 연산은 입력값 n의 해시값을 계산해 해당 버킷을 바로 찾아가므로, 데이터 양(N)에 관계없이 평균적으로 **O(1)**에 수행됩니다. (리스트의 $O(N)$과는 대조적입니다.)

2. get(n, 0) vs if n in count 성능 차이
둘 다 전체 루프의 시간 복잡도는 **O(N)**으로 동일합니다. 하지만 해시 함수 호출 횟수에서 차이가 발생합니다.

if/else 방식 (2회 조회):

- if n in count: n이 있는지 확인하기 위해 해시 조회 (1회)
- count[n] += 1: 값을 업데이트하기 위해 다시 해시 조회 (1회)
- count.get(n, 0) 방식 (사실상 2회):
- count.get(n, 0): 값을 가져오기 위해 해시 조회 (1회)
- count[n] = ...: 결과를 저장하기 위해 해시 조회 (1회)

실제 성능:
이론적으로는 비슷하지만, Python 바이트코드 수준에서는 if/else가 get() 함수 호출 오버헤드가 없어서 미세하게 더 빠를 때도 있습니다. 그러나 가독성 면에서는 get()이 더 선호됩니다.

3. 추천하는 'Expert' 스타일 (Proactivity)
더 효율적이고 깔끔한 코드를 원한다면 아래 두 방법을 사용하세요.

A. collections.defaultdict (가장 권장)
존재하지 않는 키에 대해 기본값을 자동으로 생성해 줍니다. 해시 조회를 최소화하며 코드가 간결합니다.
```python
from collections import defaultdict

count = defaultdict(int) # 기본값이 0인 dict
for n in nums:
    count[n] += 1

```

B. collections.Counter (가장 Pythonic)
이 문제(Top K Frequent)의 의도에 가장 부합하는 도구입니다. 내부적으로 C로 최적화되어 있어 속도가 매우 빠릅니다.
```python

from collections import Counter

count = Counter(nums) # 루프 돌 필요 없이 한 번에 빈도수 계산

```

- 요약
	- dict와 set의 in 연산은 **O(1)**이다.
	- get과 if는 성능 차이가 거의 없으나, **defaultdict**나 **Counter**를 쓰는 것이 성능과 가독성 면에서 더 우월하다.
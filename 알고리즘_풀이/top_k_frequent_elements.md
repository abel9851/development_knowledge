
## 문제

Given an integer array `nums` and an integer `k`, return the `k` most frequent elements within the array.

The test cases are generated such that the answer is always **unique**.

You may return the output in **any order**.

**Example 1:**

```java
Input: nums = [1,2,2,3,3,3], k = 2

Output: [2,3]
```

**Example 2:**

```java
Input: nums = [7,7], k = 1

Output: [7]
```

**Constraints:**

- `1 <= nums.length <= 10^4`.
- `-1000 <= nums[i] <= 1000`
- `1 <= k <= number of distinct elements in nums`.

링크: https://neetcode.io/problems/top-k-elements-in-list/question?list=neetcode150

## 풀이

- 첫번째 풀이
```python
class Solution:
	def topKFrequent(self, nums: List[int], k: int) -> List[int]:
		count = {}
		for n in nums:
			if n in count:
				count[n] += 1
			else:
				count[n] = 1
		
		freq = list(count.values()) # 여기를 set으로 해야 아래의 res의 요소 찾기 부분에서 O(n^2)시간 복잡도가 발생하지 않는다.
		freq.sort(reverse=True) # 이 시점에서 시간복잡도는 O(NlogN)
		
		top_freq = freq[:k]
		res = []
		
		for n, c in count.items():
			if c in top_freq:
				res.append(c)
		
		return res


```
- 두번째 풀이
```python

class Solution:
    # 습득해야할 것
    # 1. 제약조건 분석: 이게 어떻게 문제의 힌트가 되는지, 어떻게 논리, 코드로 도출할지
    # 2. 정답지 확인
    # 3. 패턴을 어떻게 도출해낼지, 지문과 예제, 제약조건을 보고 도출해서 정리
    # 4. 코드 다시 도출(복습 및 인출)
    # ? "The test cases are generated such that the answer is always unique." 이 조건이 왜 있는지 이해가 안감.
    # 지문을 봤을 때 알 수 있는 것
    # 1. 각 숫자마다 빈도를 계산 할 것
    # 2. bucket sort이던 뭐던 정렬이 필요하다는 것(bucket sort의 경우는 이미 빈도가 배열 레벨에서 정리가 되어있다.)
    # 3. k만큼의 요소를 return할 array에 추가해야하는 것
    # 제약 조건에서 알 수 있는 것
    # 1. 10^4은 1만, 이를 제곱하면, 1억. 1억번의 연산, 즉 O(n^2)는 1초를 넘길 위험이 있으니 사용하면 안될 것
    # 2. 배열 요소는 2^11 즉, 11비트이상, 16비트는 저장할수 있는 사이즈여야한다?()
    # 3-1. k는 return할 배열의 요소 개수다. 1이상이라는 것은, 무조건 빈도가 1이상이며, 숫자 하나는 있다는 것.
    # 3-2. nums배열에 구분되는 숫자 만큼 k가 있을수 있다, 라는 건데, 이는 어떻게 해석하지?
    # 즉, 모든 배열의 값을 리턴하는 것도 염두해두어라인가? [0,1,2,3,....k] 즉, 각각의 요소는 빈도가 1이니까 전부다 리턴할 수 있다는 뜻?
    # 위의 내용을 참고해서 도출한다면?
    # 제약조건 2는 별로 신경 쓰지 않아도 된다. 파이썬에서는 어떤 숫자든 간에 자료구조에 들어가니까
    # 시간복잡도는 O(n^2)보다는 작아야한다. 이 부분은 옵시디언에 n^2보다 느린 시간복잡도가 뭐가 있는지 정리해보자.
    # k부분은, 빈도 0인 부분은 고려하지 않아도 된다는거다. 어제 문제를 풀어봐서 안건데, bucket sort를 사용할때에는 이부분은 
    # bucket sort의 배열을 for loop할 때 빈도가 0인 부분은 고려하지 않는다. => 길이가 줄어든다.
    # 지문으로 도출한, 알고리즘 순서에서 고려해야할 것들은, 배열의 크기, 시간복잡도.

    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        
        # 1. count frequency
        count = {}
        for n in nums: 
            count[n] = 1 + count.get(n, 0)

        # 2. bucket sort
        freq = [[] for _ in range(len(nums) + 1)] # freq의 인덱스는 0부터 len(nums)까지가 된다. len(nums)으로 range를 했다면 len(nums) - 1의 인덱스까지 되니까.
        for n, c in count.items():
            freq[v].append(c)

        res = []

        # 3. return res
        for i in range(len(freq)-1, 0, -1):
            for n in freq[i]:
                res.append(n)
                if len(res) == k:
                    return res


```


### 풀 때 어려웠던 점
- 다음날 bucket sort로 문제를 풀 때, freq nested list에 value와 key를 반대로 넣는 것에 대해 상당히 혼란스러웠다. `list(count.values()).sort(reverse=True)`와 뭐가 다른건지 계속 속으로 갈등했다.

### 궁금점
- 제약조건은 보면 `1 <= nums.length <= 10^4`이 있던데 이부분의 의미는?
		- $O(n^2)$는 c, java라면 1초이내에 통과될지도 모르지만 python에서는 TLE(time limit exceeded)가 발생할지도 모르니, $O(n^2)$ 보다는 빠른 알고리즘을 쓸 것
- 빈도가 같은 요소가 3개 이상일 경우, 어떻게 되는건가? 제약조건에 항상 answer가 unique하다는 거는 이부분의 논란여지를 없애기 위한 조건인건가?
	- 맞다.

### 반복되서 나오는 개념
- sort
	- python에서는 timsort를 사용하니까 timsort가 뭔지를 sort가 3번이상 나올 경우 정리할 것

## 풀이에서 알아야 할 것
- bucket sort는 `len(nums) + 1`만큼의 길이를 가진 리스트를 생성한다. 공간 복잡도는 대략 $O(N)$ 이다. 시간 복잡도는 $O(N)$ 이다. (14ms가 걸렸다.)
	- 파이썬 루프 오버헤드와 리스트 객체 생성비용 때문에 실제 속도는 느릴수 있으나 이론적으로는 가장 효율적이다.
- `heapq.nlargest`를 사용하면 시간복잡도는 $O(NlogK)$ , 공간 복잡도는 $O(N)$ 이다. heapq는 내부에서 c언어를 사용해서 최적화하기 때문에 시간이 적게 걸린다.(leetcode에서 테스트했을 때에는 2ms가 걸렸다. )
	- C확장 모듈 덕분에 파이썬 환경에서 빠른 성능을 보여준다.

#### link
[[복잡도 정리]]
[[python dictionary의 get을 사용해서 value찾는 것과 if를 사용해서 value를 찾는 것에 대한 질문]]
[[python collection counter]]
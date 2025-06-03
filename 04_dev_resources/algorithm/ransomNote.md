
```python
class Solution:
    def canConstruct(self, ransomNote: str, magazine: str) -> bool:
        mag_dic = {}
        for buc in magazine:
            if buc in mag_dic:
                mag_dic[buc] += 1
            else:
                mag_dic[buc] = 1

        for buc in ransomNote:
            if buc in mag_dic:
                mag_dic[buc] -= 1
                if mag_dic[buc] < 0:
                    return False
            else:
                return False

        return True
```

- 일단 hash map으로 몇번 틀리면서 풀긴했는데 key에 해당하는 문자를 value에 카운팅하는 방향성은 맞았다.
- 하지만 로직은 더 간단히 할수 있으니 찾아보자.
- hash map형식이 아닌, 리스트에서 하나하나 찾는 방법도 있으니 이것도 이번에 한번보자.
- 그리고 시간복잡도, 공간복잡도 잘 이해가 안가니까 이에 대해서도 코드 한줄한줄 파보면서 하자.
	- hash map, english lowcase만 따졌을 경우, 공간복잡도는 O(k)라는 것은 이해했다.
		- 왜 k라는 문자를 사용하는지는 이해가 안갔으니 다시한번 보자.


```python


def can_construct(ransomNote: str, magazine: str) -> bool:

	count = [0] * 26

	for c in magazine:
		count[ord(c) - ord("a")] += 1

	for c in ransomNote:
		count[ord(c) - ord("a")] -= 1
		if count[ord(c) - ord("a")] < 0:
			return False
	return True

ransomNote = "abc"
magazine = "abcd"

print(can_construct(ransomNote, magazine))
```
- ord를 사용해서 해결할 수 있다.
- ord는 ordinal_number(순서상의 숫자), a, b, c를 정수 순서로 표현할 수 있다는 의미다.
- 파이썬의 내장함수로, 문자를 그에 해당하는 유니코드 정수값으로 변환하는 함수다.
- ransom note에서는 알파벳 소문자만 사용하기 때문에 최대 26종류의 문자가 존재한다는 제약이 있다.
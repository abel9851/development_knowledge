fillter할 때에는 evaluate되지 않지만 다음과 같은 경우, evaluate된다.
- iteration: 반복분
- asynchronous iteration
- slicing: slicing은, unevaluated QuerySet을 slicing하면, slicing된 그 QuerySet도 Unevaluated하다.
  step parameter를 사용하면 list를 return하면서 evaluate한다. 마지막 2가 요소를 2개씩 건너뛰어서 슬라이싱한다.  `Entry.objects.all()[:10:2]`
- repr()
- len()
- bool()
	- bool()이외에도 or, and if에 사용될 때에도 evaluate된다.
```python

# boolean context에서 Query가 실행된다.
if Entry.objects.filter(headline="Test"):
	print("There is at least one Entry with the headline Test")

```

`Entry.objects.all().select_related("book")` 같이, all()을 호출한 다음에 join을 해도 evaluate되지 않는다..

Reference
https://docs.djangoproject.com/en/4.2/ref/models/querysets/

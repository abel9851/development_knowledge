- 리스트와는 다르게 모든 요소를 메모리에 올리지 않고 필요할 때 내보내는 객체
```python

# list comprehension
my_list = [x for x in range(10_000_000)]

# generator expression
my_gen = (x for x in range(10_000_000))

```
- 리스트는 모든 요소를 메모리에 올리지만 제네레이터는 필요할 때(next로 generator를 호출한다던가) 값을 내보낸다.
- 인덱싱이 불가능하다. 오직 next()로 순차 접근할 수 있다.
- 재사용도 불가능하다. 한번 소비되면 끝이다.
- 전체 생성은 느릴 수 있으나 첫 값 반환이 즉시 일어난다.
	- (첫 값 반환이 즉시 일어난다는 게 무슨 뜻이지?)
		- 리스트는 전체 생성은 빠를 수 있으나 초기 로딩이 크다.
- 제네레이터는 데이터를 생성하는 규칙(instruction)만 저장한다. 값이 필요할 때(next 호출 시) 그때서야 CPU를 써서 값을 계산하고 반환 후 메모리에서 값을 잊는다.
	- 아래의 코드에서 192bytes는 제네레이터 객체(Struct)자체의 크기다.
```python
import sys

my_gen = (x for x in range(10_000_000))
print(f"{sys.getsizeof(my_gen):,} bytes")
# 192 bytes
```

- 실무에서 제네레이터가 사용되는 부분
	- 대용량 파일/로그 처리(ETL 파이프라인)
		- (ETL 파이프라인이 뭐지?)
	- 데이터베이스 배치 처리
	- 리소스 관리(Setup/Teardown패턴)
	- 로직의 결합도 분리(Decoupling)

다음 읽어볼 것
https://docs.python.org/3.11/reference/simple_stmts.html#yield

Reference
https://docs.python.org/3.11/glossary.html#term-generator

#python #기초 #generator


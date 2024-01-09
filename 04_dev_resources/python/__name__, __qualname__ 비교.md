
\_\_name\_\_과 \_\_qualname\_\_은 클래스의 인스턴스에는 할당되지 않는다.
두개의 차이점은, \_\_name\_\_은 객체의 이름만 출력하고 \_\_qualname\_\_은 객체의 전체 경로를 나타낸다.
예를 들어 `def my_function(): pass`가 있을 때, `my_function.__name__`은 `my_function`만 반환한다. 
`my_function.__qualname__`은 class가 MyClass였을 경우,
`MyClass.my_function`을 반환한다.  \_\_qualname\_\_은 상속관계는 포함하지 않지만 중첩 클래스일 경우, 중첩되어있는 클래스까지 경로로 포함해서 반환한다.


- 테스트한 코드
```python

class WrapperTest:
    """현재 진화를 이뤄가는 중이다."""

    def __init__(self):
        self.name = "Wrapper test"


class WrapperTest2(WrapperTest):
    """현재 해나가는 중이다."""

    def __init__(self):
        self.name = "Wrapper test2"


def test():

    print("test")


test.view = "wow"

wrapper_test_2 = WrapperTest2()

update_wrapper(test, wrapper_test_2, updated=())

print("WrapperTest의 __module__: ", test.__module__)
print("WrapperTest의 __name__: ", test.__name__)
print("WrapperTest의 __qualname__: ", test.__qualname__)
print("WrapperTest의 __annotations__: ", test.__annotations__)
print("WrapperTest의 __doc__: ", test.__doc__)
print("WrapperTest의 __doc__: ", wrapper_test_2.__name__)

class WrapperTest3(WrapperTest2):

    def __init__(self):
        self.name = "Wrapper test3"

    @classmethod
    def as_view(cls):

        def test():
            print("test")

        update_wrapper(test, cls, updated=())

        return test


test = WrapperTest3.as_view()
print("WrapperTest3의 __module__: ", test.__module__)
print("WrapperTest3의 __name__: ", test.__name__)
print("WrapperTest3의 __qualname__: ", test.__qualname__)
print("WrapperTest3의 __annotations__: ", test.__annotations__)
print("WrapperTest3의 __doc__: ", test.__doc__)


```


- 출력결과
```plain text


❯ python test_4.py
WrapperTest의 __module__:  __main__
WrapperTest의 __name__:  WrapperTest
WrapperTest의 __qualname__:  WrapperTest
WrapperTest의 __annotations__:  {}
WrapperTest의 __doc__:  현재 진화를 이뤄가는 중이다.
❯ python test_4.py
WrapperTest의 __module__:  __main__
WrapperTest의 __name__:  WrapperTest2
WrapperTest의 __qualname__:  WrapperTest2
WrapperTest의 __annotations__:  {}
WrapperTest의 __doc__:  현재 해나가는 중이다.
❯ python test_4.py
WrapperTest의 __module__:  __main__
WrapperTest의 __name__:  __init__
WrapperTest의 __qualname__:  WrapperTest2.__init__
WrapperTest의 __annotations__:  {}
WrapperTest의 __doc__:  None
❯ python test_4.py
WrapperTest의 __module__:  __main__
WrapperTest의 __name__:  test
WrapperTest의 __qualname__:  test
WrapperTest의 __annotations__:  {}
WrapperTest의 __doc__:  현재 해나가는 중이다.
❯ python test_4.py
WrapperTest의 __module__:  __main__
WrapperTest의 __name__:  test
WrapperTest의 __qualname__:  test
WrapperTest의 __annotations__:  {}
WrapperTest의 __doc__:  현재 해나가는 중이다.
Traceback (most recent call last):
  File "/home/shinheejun/private/practice_django/test_4.py", line 52, in <module>
    print("WrapperTest의 __doc__: ", wrapper_test_2.__qualname__)
                                    ^^^^^^^^^^^^^^^^^^^^^^^^^^^
AttributeError: 'WrapperTest2' object has no attribute '__qualname__'
❯ python test_4.py
WrapperTest의 __module__:  __main__
WrapperTest의 __name__:  test
WrapperTest의 __qualname__:  test
WrapperTest의 __annotations__:  {}
WrapperTest의 __doc__:  현재 해나가는 중이다.
Traceback (most recent call last):
  File "/home/shinheejun/private/practice_django/test_4.py", line 52, in <module>
    print("WrapperTest의 __doc__: ", wrapper_test_2.__name__)
                                    ^^^^^^^^^^^^^^^^^^^^^^^
AttributeError: 'WrapperTest2' object has no attribute '__name__'. Did you mean: '__ne__'?


❯ python test_4.py
WrapperTest3의 __module__:  __main__
WrapperTest3의 __name__:  WrapperTest3
WrapperTest3의 __qualname__:  WrapperTest3
WrapperTest3의 __annotations__:  {}
WrapperTest3의 __doc__:  None


```

Link
[[__annotations__]]
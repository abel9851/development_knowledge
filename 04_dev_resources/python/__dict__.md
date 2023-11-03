
update_wrapper의 updated 인자를 사용한 업데이트가 \_\_dict\_\_을 사용하고 있었기 때문에 어떤게 출력되는지 기록한다.

- 테스트한 코드
```python

class WrapperTest3(WrapperTest2):

    def __init__(self):
        self.name = "Wrapper test3"

    

    @classmethod
    def as_view(cls):

        def test():
            print("test")

        update_wrapper(test, cls, updated=())
        update_wrapper(test, cls, assigned=()) # 이 부분이 update_wrapper에서 updated 튜플 안에 있는 '__dict__'를 for loop를 사용해서 test에 update하는 부분이다.

        return test


test = WrapperTest3.as_view()
print("WrapperTest3의 __module__: ", test.__module__)
print("WrapperTest3의 __name__: ", test.__name__)
print("WrapperTest3의 __qualname__: ", test.__qualname__)
print("WrapperTest3의 __annotations__: ", test.__annotations__)
print("WrapperTest3의 __doc__: ", test.__doc__)
print("WrapperTest3의 __dict__: ", test.__dict__)

```

- 출력결과
```plain text

WrapperTest3의 __dict__:  {'__wrapped__': <class '__main__.WrapperTest3'>, '__module__': '__main__', '__init__': <function WrapperTest3.__init__ at 0x7f47b7a01300>, 'as_view': <classmethod(<function WrapperTest3.as_view at 0x7f47b7a013a0>)>, '__doc__': None, '__annotations__': {}}

```

Link
[[[update_wrapper in functools]]]
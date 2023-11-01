
self는 메소드가 정의된 클래스가 self다. 즉, 호출한 test가 아니라 Example의 인스턴스인 example이다.
여기서 example.value는 Example 클래스의 value 메소드를 인스턴스 example에 바인딩한 메서드 객체다.
test 안의 self.func는 example.value를 참조하고 있으므로, 결국 example.value()를 호출하는 것과 동일하다. 이때 value 메서드의 self 파라미터에는 example 인스턴스가 자동으로 전달된다.

여기서 봤듯이 self.func()에는 self에 해당되는 인스턴스를 인자로 건네줄 필요가 없다.
인터프리터가 자동으로 인자를 전달하기 때문이다.
하지만 디스크립터에서는 다르다. 디스크립터 프로토콜에서는 명시적으로 self에 해당하는 인자를 지정해줘야한다.
```python

class Test:

    def __init__(self, func):
        self.func = func

    def call_func(self):
        self.func()


class Example:

    # @ClassOnlyMethod
    def value(self):
        print("self:", self)
        print("value")

example = Example()
test = Test(example.value)
test.call_func()

# 출력결과
# self: <__main__.Example object at 0x7fa6375d8690>
# value



```


- 명시적으로 self(혹은 cls) 인자를 전달, 명시해야하는 디스크립터와 그렇지 않은 일반 클래스의 호출 비교
```python

class ClassOnlyMethod:
    def __init__(self, func):
        self.func = func

    def __get__(self, instance, owner):
        if instance is not None:
            raise AttributeError("This method is available only at the class level")
        else:
            print("owner, 즉 cls 뿐이다! :", owner)
            print("intance는 존재하지 않는다! :", instance)

        def wrapper(*args, **kwargs): # 이게 그냥 return self.func했을 떄 owner(cls)가 필요해지는데
                                      # 그 문제를 해결해준다.
                                      # 여담으로 디스크립터 프로토콜 말고 이외의 보통 class에서는
                                      # self.func(*args, **kwargs)로 호출해줘야한다.
                                      # 파이썬 인터프리터가 자동으로 인자를 전달하기 때문이다.
                                      # 디스크립터 프로토콜에서는 명시적으로 전달해야한다.
            return self.func(owner, *args, **kwargs)
        return wrapper


```

```python


 def dispatch(self, request, *args, **kwargs):
        # Try to dispatch to the right method; if a method doesn't exist,
        # defer to the error handler. Also defer to the error handler if the
        # request method isn't on the approved list.
        if request.method.lower() in self.http_method_names:
            handler = getattr(self, request.method.lower(), self.http_method_not_allowed)
        else:
            handler = self.http_method_not_allowed
        return handler(request, *args, **kwargs)

```
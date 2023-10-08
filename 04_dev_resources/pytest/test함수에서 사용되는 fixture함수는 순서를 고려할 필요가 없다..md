
```python


import pytest


class Fruit:
    def __init__(self, name):
        self.name = name

    def __eq__(self, other):
        return self.name == other.name


@pytest.fixture
def my_fruit():
    return Fruit("apple")


@pytest.fixture
def fruit_basket(my_fruit):
    return [Fruit("banana"), my_fruit]

def test_my_fruit_in_basket(fruit_basket, my_fruit):
    assert my_fruit in fruit_basket



```

`test_my_fruit_in_basket`에서 `fruit_basket`과 `my_fruit` fixture 함수를 사용하고 있다.
`fruit_basket` 함수에서 `my_fruit` 를 인자로 사용하고 있기 때문에 `test_my_fruit_in_basket` 에서 `my_fruit` 함수를 먼저 호출할 필요가 있다고 생각했다.
하지만 pytest의 fixture에서는 테스트함수 시그니처를 가진 테스트함수처럼 인자로 받은 fixture함수를 호출하도록 되어있기 때문에 `fruit_basket` 함수가 호출될 때 인자로 설정한 `my_fruit`와 같은 이름을 가진 fixture함수를 호출한다.



```python

import pytest


class Fruit:
    def __init__(self, name):
        self.name = name

    def __eq__(self, other):
        return self.name == other.name


@pytest.fixture
def my_fruit():
    return Fruit("apple")


@pytest.fixture
def fruit_basket(my_fruit):
    return [Fruit("banana"), my_fruit]


# def test_my_fruit_in_basket(my_fruit, fruit_basket):
#     assert my_fruit in fruit_basket


def test_my_fruit_in_basket(fruit_basket):
    assert len(fruit_basket) == 1

# 



```

```bash

fruit_basket = [<test_case.Fruit object at 0x7f2065993590>, <test_case.Fruit object at 0x7f2065993710>]

    def test_my_fruit_in_basket(fruit_basket):
>       assert len(fruit_basket) == 1
E       assert 2 == 1
E        +  where 2 = len([<test_case.Fruit object at 0x7f2065993590>, <test_case.Fruit object at 0x7f2065993710>])

test_case.py:27: AssertionError
======================================================================================== short test summary info =========================================================================================
FAILED test_case.py::test_my_fruit_in_basket - assert 2 == 1
=========================================================================================== 1 failed in 0.11s ===========================================================================================

```

```python

@pytest.fixture
def mock_s3_client():
    def _put_object(body, bucket, key):
		pass

    with patch("boto3.client") as mock:

        mock_client = MagicMock()
        mock_client.put_object.side_effect = _put_object
        mock.return_value = mock_client
        yield mock

def test_s3_client(mock_s3_client):
    client = boto3.client()
    assert isinstance(client, MagicMock)
    mock_s3_client.assert_called_once() # mock 객체가 한번만 호출됬다는 것을 확인하는 메소드
```

mocking한 객체를 대신하여 mock객체가 호출되는지 확인하기 위해 사용하는 메소드가 있다.
`assert_called_once`, `assert_called`, `assert_called_with`, `assert_called_once_with`, `assert_any_call`이 그러한 메소드들이다.

`Mock`과 `MagicMock`에서 사용할 수 있는 것을 확인했다.

- assert_called
	- `Mock`이 적어도 한번은 호출됬다는 것을 assert한다.

```bash
mock = Mock()
mock.method()
<Mock name='mock.method()' id='...'>
mock.method.assert_called()
```
	
- assert_called_once
	- `Mock`이 정확히 한번 호출됬다는 것을 assert한다.

```bash

mock = Mock()
mock.method()
<Mock name='mock.method()' id='...'>
mock.method.assert_called_once()
mock.method()
<Mock name='mock.method()' id='...'>
mock.method.assert_called_once()
Traceback (most recent call last):
...
AssertionError: Expected 'method' to have been called once. Called 2 times.
```
- assert_called_with
	- 마지막 호출이 특정한 방식으로 이루어졌음을 assert하는 편리한 메소드다.
	- 특정한 방식이 뭔지는 잘 모르겠지만 예제 코드에서는 인자를 사용해서 호출한 것을 보여주고 있다.

```bash
mock = Mock()
mock.method(1, 2, 3, test='wow')
<Mock name='mock.method()' id='...'>
mock.method.assert_called_with(1, 2, 3, test='wow')

```

- assert_called_once_with
	- `Mock`이 지정한 인자를 받아서 정확히 한번 호출되었다는 것을 assert한다.

```bash
mock = Mock(return_value=None)
mock('foo', bar='baz')
mock.assert_called_once_with('foo', bar='baz')
mock('other', bar='values')
mock.assert_called_once_with('other', bar='values')
Traceback (most recent call last):
  ...
AssertionError: Expected 'mock' to be called once. Called 2 times.

```

- assert_any_call
	- 지정한 인자를 사용해서 `Mock`이 호출되었다는 것을 assert한다.
	- 가장 최근의 호출에 대해서만 pass되는 `assert_called_with`과 `assert_called_once_with`과는 달리
	  호출된적이 있기만 하면 assert한다.
	- 아래의 코드를 보면 최근 호출된 것이 아닌 `mock(1, 2, arg='thing'` 이 pass되어 assert한 것을 볼 수 있다.

```bash
mock = Mock(return_value=None)
mock(1, 2, arg='thing')
mock('some', 'thing', 'else')
mock.assert_any_call(1, 2, arg='thing')

```

Reference
https://docs.python.org/3/library/unittest.mock.html#unittest.mock.Mock.assert_called
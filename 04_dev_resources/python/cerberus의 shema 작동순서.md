잊어버릴 것 같아서 적어둔다.

```python

schema = {
                'name': {'type': 'string', 'empty': True},
                'limit': {'type': 'integer',
                          'coerce': lambda x: set_default(x, default=10)},
                'offset': {'type': 'integer',
                           'coerce': lambda x: set_default(x, default=0)},
            }

```

validate에서 사용되는 schema 중에 
limit, offset이 coerce옵션을 사용한다. 이때 schema가 적용되는 순서는 coerce -> type이다.  
예를 들어 `localhost:8000?limit=2&offset=1` 이라고 클라이언트로부터 요청이 왔을 경우,
limit과 offset은 파이썬 안에서는 string으로 취급된다.
그렇기 때문에 위의 schma의, limit, offset의 타입을 string으로 해야한다고 생각하는데
corece의 람다함수가 int를 return한다면, invalid literal이라는 에러가 발생한다.

왜냐하면, corece의 람다함수가 먼저 실행되고 그 다음 type을 확인하기 때문이다.
위의 schema에서 limit과 offset의 값을 확인한 후, int로 돌려준다면, 즉 사용되는 함수의 타입에 맞게 type을 수정해줘야한다.
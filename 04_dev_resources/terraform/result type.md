테라폼에서는 bracket의 유형이 결과의 유형을 결정한다. []은tuple을, {}은 object를 반환한다.

```
{for s in var.list: s => upper(s)}
```

위의 코드에서 반환되는 결과는 var.list의 요소들(s)를 대문자로 바꿔서 value로 하고 attribute는 소문자 그대로 사용해서 object가 된다.

```
{
  foo = "FOO"
  bar = "BAR"
  baz = "BAZ"
}

```

for문만 사용하면 결과는 tuple이나 object value를 반환할 수 있지만, 테라폼 자동 전환 규칙 덕분에 리스트, 맵, 집합이 예상되는 위치에서 이 결과를 사용할 수 있다.
(테라폼에서는 tuple과 list를 구분하는 별도의 표기법이 없다. map과 object도 마찬가지다. 내부적으로는 타입이 구분되며, 컨테스트난 값의 가변성에 따라 타입이 결정된다.)

Reference
https://developer.hashicorp.com/terraform/language/expressions/for#result-types
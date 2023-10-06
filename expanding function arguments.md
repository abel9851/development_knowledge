
...(three periods)을 사용하면  value를 별도의 인수로 확장(분리)할 수 있다.

```
merge([for box in boxes : {

for item in box:
item.key => item.value
}]...)

```


Reference
https://developer.hashicorp.com/terraform/language/expressions/function-calls#expanding-function-arguments

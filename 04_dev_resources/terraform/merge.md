
merge는 인자로 받은 것들을 하나로 합친다. 보통 여러 맵을 하나의 맵으로 만드는 데 사용한다.

```
merge([for box in boxes : {

for item in box:
item.key => item.value
}]...)


```




link
[[테라폼에서 for표현식]], [[expanding function arguments]]

Reference
https://developer.hashicorp.com/terraform/language/functions/merge

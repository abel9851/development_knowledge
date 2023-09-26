browser에서 쿼리스트링으로 서버에 request를 하면 그 쿼리스트링의 value를 숫자로 입력했더라도 string으로 전달된다.

```plain text
http://localhost:8000/v1/books?limit=1
```

postman으로 확인(valiate는 python의 cerberus 라이브러리를 사용했고, validate할 때 .limit의 type은 integer로 설정했다.)해본 결과, request안에 있는 limit의 값은 integer가 아니라는 것을 확인 할 수 있다.

type을 string으로 바꾸니까 정상적으로 response를 받았다.

![[Pasted image 20230926100638.png]]
request객체와 args, kwargs를 인자로 받아들이고 http response를 리턴한다. 
1. http method를 검사하고
2. http method와 일치하는 method에 위임을 시도한다; GET은 get()으로, POST는 post로 위임된다.

기본적으로 HEAD는 get()으로 위임된다.


dispatch라는 단어는 보내다, 파견하다, 배송하다 등의 의미를 가지고 있다.
또한 특정 작업을 처리하기 위해 적절한 메소드나 함수를 **선택**하고 호출하는 과정하는 지칭할 때도 사용된다.

코드적으로는 handler 메소드(getattr(self, request.mtehod.lower(), self.http_method_not_allowed))를 호출한다. 호출 기준은 base view에 정의된 http_method_names에 요청하고자 하는 method가 있을 것, 있다고 하면 사용하는 view에 method가 정의되어 있을 것을 확인한다.

handler라는 변수에 메소드를 할당해서 return한다.
공식 문서에서는 http response를 반환한다고 하는데 정확히는 handler를 호출해서 handler가 반환하는 http response를 반환하는 형태다.

handler 메소드 자체는 dispatch안에서 정의하지 않고 따로 정의된 메소드를 할당하는 형식으로 되어있다. 이는 dispatch라는 메소드 이름 안에 handler(method)를 개발자에게 파견한다는 역할만 하도록 함수를 적절히 분리시킨 케이스라고 생각한다.(기억하고 습득하자.)

실제로 http_method_not_allowed나 getattr로 얻은 http요청에 대응되는 get(), post() 등의 메소드는 따로 정의한다.(dispatch 안에 인자로 메소드를 제공하지 않고도 호출하고자 하는 메소드를 호출하는 방식으로 getattr이나 self를 사용하고 있다. 이부분도 기억하고 습득하자.)

getattr를 사용하는 이유는, 주어진 value로 객체 안에 있는 attribute(이 경우에는 method)를 찾는 것과 찾지 못할 시 호출할 메소드를 지정하기 위함이다.

```python

def dispatch(self, request, *args, **kwargs):
	if request.method.lower() in self.http_method_names:
		handler = getattr(self, request.method.lower(), self.http_method_not_allowed)
	else
		hanlder = self.http_method_not_allowed
	return handler(request,*args, **kargs)

```


기능적으로 dispatch는 http 요청을 감지하고 적절한 핸들러 메소드로 라우팅하는 역할을 한다.
예를 들어 HTTP GET 요청이 들어오면 dispatch 메소드는 이를 감지하고 get 메소드를 호출한다.
마찬가지로 POST 요청이 들어오면 이를 감지하고 post 메소드를 호출한다.
이를 봤을 때 dispatch라는 이름은 이러한 라우팅 역할을 하는데 어울리는 이름이다.
요청이 들어오면 요청을 감지하여, 그 요청을 처리하기 위해 메소드를 '파견'하는 역할을 하기 때문이다.

dispatch가 들어온 요청을 해결하기 위해 우리 개발자들에게 method를 파견한다. 라고 생각하면 이해하기 쉽다.

Reference
https://docs.djangoproject.com/en/4.2/ref/class-based-views/base/#django.views.generic.base.View.dispatch
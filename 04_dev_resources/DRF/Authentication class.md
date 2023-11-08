
Authentication은 credentials와 request를 연결하는 메커니즘이다.
식별이 되면 들어오는 request에 대해 허용할 것인지 허용하지 않을 것인지는 permission과 throttle에서 판단한다.

authentication은 클래스의 리스트로 정의되는데 authentication이 성공한 첫번째 클래스로부터 request.user와 request.auth의 value를 할당한다.

request.user는 contrib.user의 인스턴스가 일반적으로 할당되고 request.auth는 요청에 서명한 인증 토큰을 나타내는 데 사용하는, 추가적인 인증정보로 활용된다.

```python

from rest_framework.authentication import SessionAuthentication, BasicAuthentication
from rest_framework.permissions import IsAuthenticated
from rest_framework.response import Response
from rest_framework.views import APIView

class ExampleView(APIView):
    authentication_classes = [SessionAuthentication, BasicAuthentication]
    permission_classes = [IsAuthenticated]

    def get(self, request, format=None):
        content = {
            'user': str(request.user),  # `django.contrib.auth.User` instance.
            'auth': str(request.auth),  # None
        }
        return Response(content)


@api_view(['GET'])
@authentication_classes([SessionAuthentication, BasicAuthentication])
@permission_classes([IsAuthenticated])
def example_view(request, format=None):
    content = {
        'user': str(request.user),  # `django.contrib.auth.User` instance.
        'auth': str(request.auth),  # None
    }
    return Response(content)

```

authentication class list에 있는 authenticate가 전부 실패할 시, request.user에는 AnonymouseUser가 할당되고 request.auth는 None이 할당된다.

인증되지 않은 request.user와 request.auth에 대한 value는 UNAUTHENTICATED_USER와 UNAUTHENTICATED_TOKEN 설정으로 수정할 수 있다.

이후 permission에서 AllowAny가 설정되어있다면 view를 사용할 수 있지만, 그렇지 않다면
인증 실패로 처리된다.

Reference
https://www.django-rest-framework.org/api-guide/authentication/
https://www.django-rest-framework.org/api-guide/authentication/#unauthorized-and-forbidden-responses

Link
[[DRF의 authentication 실패 시 동작]]
[[DRF의 deafult authentication, default permission 적용범위]]
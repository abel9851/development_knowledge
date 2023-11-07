choice field에 사용할 tuple을 모델 내에 정의하지 않고 모델 내의 중첩 클래스로, choice class를 사용해서 choice의 member를 설정할 수 있다. string, int의 choice가 기본으로 제공되고 `models.Choices`를 활용해서 `DateField`와 같은 다른 타입의 choice도 구현 가능하다.

DRF에서 value가 아닌, choice의 label을 json에 담아서 response하고 싶다면 `Model.get_FOO_display()`를 호출하면 label을 취득할 수 있다.(FOO대신 choice를 사용하고 있는 Field의 변수명을 지정하면 된다.)
```python

from rest_framework import serializers

class YourModelSerializer(serializers.ModelSerializer):
    status_display = serializers.SerializerMethodField()

    class Meta:
        model = YourModel
        fields = ['id', 'status', 'status_display']

    def get_status_display(self, obj):
        return obj.get_status_display()


```

Enumuration types를 사용해서 choice를 구현하면 `.choices`, `.labels`, `.values`, `.names` 등의 custom properties가 자동으로 설정된다.

이러한 점이 choice class를 사용하는 이점이라고 생각한다.
```bash
>>> Task.Priority.labels
['HIGH', 'MEDIUM', 'LOW']
>>> Task.Priority.choices
[(0, 'HIGH'), (1, 'MEDIUM'), (2, 'LOW')]
>>> Task.Priority.values
[0, 1, 2]
>>> Task.Priority.names
['HIGH', 'MEDIUM', 'LOW']

```

Reference
https://docs.djangoproject.com/en/4.2/ref/models/instances/#django.db.models.Model.get_FOO_display

Link
[[choice Enumuration types]]
[[choice Enumeration types human readable name 설정]]
[[DRF의 데이터 validate는 어느 계층에서 하는게 좋을까]]
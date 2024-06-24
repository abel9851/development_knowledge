
`MyModel.objects.get(id=2)`이나 `MyModel.objects.filter(id=2).only("name")`을 했을 때
name에 연결되어 있는 charField의 내부에서는 `from_db_value`가 실행되서 db로부터 value를 가져온 뒤,
None일 경우에는 None을 그냥 반환하고, 아닐 경우, python object로 바꾸는 작업을 한다.

Reference
https://docs.djangoproject.com/en/4.2/howto/custom-model-fields/#converting-values-to-python-objects


get_db_prep_value(value, connection, prepared=False)는 데이터를 로드할 때 from_db_value를 사용하는데
from_db_value는 built_in_field에서는 사용되지 않는다.
JsonField도 built_in_field일테니 from_db_value를 커스텀 get_db_prep_value를 사용할 경우,
from_db_value를 사용하지 않으면 안될터.
버그 티켓에서 이걸 확인해보자.(from_db_value는 지정했던 것 같다.)
같은 json인데도 불구하고 from_db_value를 사용하지 않는다면 database backend를 만져야할지도 모른다.
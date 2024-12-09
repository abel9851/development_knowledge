
```python

class OwnerAssessmentResultListSerializer(serializers.ModelSerializer):

	class Meta:
		model = AssessmentRequest
		fields = ["id", "price", "assessment_ok", "ng_reason", "checked_flg"]

	price = serializers.IntegerField(source="show_price", help_text="査定価格 (円)")
	assessment_ok = serializers.BooleanField(source="show_assessment_ok", help_text="査定OKか否か")
	ng_reason = serializers.SerializerMethodField("get_ng_reason", help_text="NG理由")
	checked_flg = serializers.SerializerMethodField("get_checked_flg")

```

```python

# model에 정의되어있는 field

price = models.PositiveBigIntegerField(verbose_name="価格(円)", default=0, null=True)
assessment_ok = models.BooleanField(verbose_name="査定OK", default=True, null=True)

```


```python

# serializer에 넣은 인자
# obj는 None

obj = qs.filter(

	assessment_request_master__owner_id=owner_id,

	assessment_request_master__building_id=owner_building.building_id,

).first()

serializer = self.get_serializer(obj, context={"owner_id": owner_id})

```

- obj가 None이다. 
	- 여기서 알 수 있는 사실
		- model serializer는 None객체를 인자로 받을 수 있다.
		- serializer의 data에 담긴 key와 value중 value는 null=True을 따른 것 같기도 하면서 FooField의 종류에 따라 다르게 적용되는듯 싶다.
			- null=True일 경우, 전부 None으로 값을 리턴하는 줄 알았는데 BooleanField의 경우에는 False를 반환했다. 여기서 알수 있는 것은 FooField마다 반환하는 것이 다른 것 같다는 것이다.
![[Pasted image 20241205091723.png]]

```bash

# 반환된 값

 {'status': 0, 'msg': '', 'data': {'price': None, 'assessment_ok': False}}

```
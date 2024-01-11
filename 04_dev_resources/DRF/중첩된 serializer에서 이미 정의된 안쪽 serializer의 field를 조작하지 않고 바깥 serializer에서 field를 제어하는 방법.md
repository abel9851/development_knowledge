pomodoro와 task는 foreign key관계이고 이미 중첩된 serializer가 정의되어 있을 때,
다른 api에서 같은 serializer를 사용하되, field를 바꾸고 싶다면 아래와 같이 `get_` 메소드를 사용해서 field를 제어할 수 있다.

```python
# 이미 정의되어 있는 serializer
class TaskPomodoroCreateSerializer(serializers.ModelSerializer):

"""Task Create model Serializer"""
	pomodoro_count = serializers.IntegerField(max_value=42, min_value=1)
	pomodoros = PomodoroCreateSerializer(many=True, read_only=True)

	class Meta:
		model = Task
		fields = ["id", "name", "priority", "due_date", "pomodoro_count", "pomodoros"]

	def create(self, validated_data):
		pomodoro_count = validated_data.get("pomodoro_count")
		# pomodoro_count = validated_data.pop("pomodoro_count")
		task = Task.objects.create(**validated_data)
		pomodoros = [Pomodoro(task=task) for _ in range(pomodoro_count)]
		Pomodoro.objects.bulk_create(pomodoros)

		return task

class PomodoroCreateSerializer(serializers.ModelSerializer):
    class Meta:
        model = Pomodoro
        fields = ["id", "pomodoro_length", "break_length"]


# 정의된 serializer를 수정하지 않고 field를 get 메소드를 사용해서 제어
class TaskPomodoroCreateSerializer(serializers.ModelSerializer):
    # ... 기존 코드 ...

    pomodoros = serializers.SerializerMethodField()

    def get_pomodoros(self, obj):
        # PomodoroCreateSerializer를 사용하여 pomodoros 필드를 커스텀
        pomodoros = Pomodoro.objects.filter(task=obj)
        return PomodoroCreateSerializer(pomodoros, many=True, fields=['pomodoro_length']).data

    # ... 기존 코드 ...

```

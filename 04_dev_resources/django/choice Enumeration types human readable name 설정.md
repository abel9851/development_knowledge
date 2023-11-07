Enumeration types는 choe


- human readable name을 직접 지정하지 않을 경우 
  `choice_class.property.label`에 member name(property name)으로부터 추론해서 지정된다.
```python

# 실제 코드
class Task(core_models.TimeStampedModel):
    """Task Model Definition"""

    class Priority(models.IntegerChoices):
        HIGH = 0
        MEDIUM = 1
        LOW = 2

# human readable name이 없을 경우의 migration

from django.db import migrations, models


class Migration(migrations.Migration):

    dependencies = [
        ("tasks", "0001_initial"),
    ]

    operations = [
        migrations.AddField(
            model_name="task",
            name="priority",
            field=models.IntegerField(
	            # High라고 지정하지 않았는데 자동으로 지정.
                choices=[(0, "High"), (1, "Medium"), (2, "Low")], default=1
            ),
        ),
    ]



```

![[Pasted image 20231107235752.png]]


- 지정했을 경우, 지정한 human readable name이 설정된다.(첫번째 값이 db에 저장되는 실제 값이다.
  human readable name이 표시되는 곳은 admin page, form, 모델 인스턴스 메소드(`get_FOO_display()`) 등으로, DRF에서 json으로 response하는 경우, 커스터마이징하지 않는 이상 첫번째 값이 활용된다.)
```python

# 실제 코드
class Task(core_models.TimeStampedModel):
    """Task Model Definition"""

    class Priority(models.IntegerChoices):
        HIGH = 0, _("HIGH")
        MEDIUM = 1, _("MEDIUM")
        LOW = 2, _("LOW")

# migration
from django.db import migrations, models


class Migration(migrations.Migration):

    dependencies = [
        ("tasks", "0002_task_priority"),
    ]

    operations = [
        migrations.AlterField(
            model_name="task",
            name="priority",
            field=models.IntegerField(
                choices=[(0, "HIGH"), (1, "MEDIUM"), (2, "LOW")], default=1
            ),
        ),
    ]

```
![[Pasted image 20231108001234.png]]
![[Pasted image 20231108001500.png]]

Reference
https://docs.djangoproject.com/en/4.2/ref/models/fields/#field-choices-enum-subclassing

Link
[[choice Enumuration types]]
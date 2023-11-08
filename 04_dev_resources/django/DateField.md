파이썬에서 datetime.date로 표현되는 Date를 저장한다.
auto_now와 auto_now_add 인자를 지정할 수 있다.

auto_now_add는 모델 인스턴스를 생성할 때의 현재 Date를 저장한다.
auto_now는 인스턴스가 save될 때마다 매 시간이 field에 설정된다.
주의할 것은 `Model.save()`를 할 때 저장되지, `QuerySet.update()`를 할 때에는 저장되지 않는다.
그리고 auto_now_add는 default로 사용할 수 없다.
default에 현재 시간을 지정하고 싶다면 아래와 같이 하면 된다.

```python

from datetime import date
from django.utils import timezone

date = DateField(default=date.today)
date_and_time = DateTimeField(default=timezone.now)

```


두 인자는 default timezone을 사용한다. 커스터마이징을 하고 싶다면 공식문서를 참고하자.

`default=date.today`를 사용해서 생성했을 때의 SQL이다. `2023-11-08` 현재 날짜가 default도 저장된다.
```bash

INSERT INTO `tasks_task` (`created`, `updated`, `name`, `memo`, `priority`, `due_date`)
VALUES ('2023-11-08 14:13:42.761008', '2023-11-08 14:13:42.761055', 'test_task', 'test task', 1, '2023-11-08')
Execution time: 0.014418s [Database: default]

```


Reference
https://docs.djangoproject.com/en/4.2/ref/models/fields/#datefield
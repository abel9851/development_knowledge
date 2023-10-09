values_list는 values와 비슷하지만 딕셔너리를 반환하지 않고 튜플을 반환한다.
즉, key없이 value만 반환된다.

```bash
>>> Entry.objects.values_list("id", "headline")
<QuerySet [(1, 'First entry'), ...]>
>>> from django.db.models.functions import Lower
>>> Entry.objects.values_list("id", Lower("headline"))
<QuerySet [(1, 'first entry'), ...]>

```

`flat` 인자를 True하면 단일 values 즉, 튜플없이 value를 얻을 수 있고 리스트처럼 사용할 수 있다. 주의할 점으로는 field가 하나만 사용 경우에만 사용할 수 있다.
```bash

>>> Entry.objects.values_list("id").order_by("id")
<QuerySet[(1,), (2,), (3,), ...]>

>>> Entry.objects.values_list("id", flat=True).order_by("id")
<QuerySet [1, 2, 3, ...]>

```



`named=True`를 사용하면 namedtuple을 사용할 수 있다.
```bash
>>> Entry.objects.values_list("id", "headline", named=True)
<QuerySet [Row(id=1, headline='First entry'), ...]>

```

특정 모델 인스턴스에 대해 필드 값을 얻으려면 아래와 같다.
```bash
>>> Entry.objects.values_list("headline", flat=True).get(pk=1)
'First entry'

```

link
[[values]]

Reference
https://docs.djangoproject.com/en/4.2/ref/models/querysets/#django.db.models.query.QuerySet.values_list
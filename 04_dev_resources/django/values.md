모델 인스턴스가 아니라 딕셔너리를 돌려주는 쿼리셋을 반환한다.
모델 인스턴스가 2개 이상일 경우에는 각 모델 인스턴스에 해당하는 딕셔너리들을 반환한다. 

```bash

>>> Blog.objects.filter(name__startswith="Beatles")
<QuerySet [<Blog: Beatles Blog>]>

>>> Blog.objects.values()
<QuerySet [{'id': 1, 'name': 'Beatles Blog', 'tagline': 'All the latest Beatles news.'}]>
>>> Blog.objects.values("id")
<QuerySet [{'id': 411271}, {'id': 412711}]>


```

그 밖에도 optional keyword arguments를 취할 수 있다.
```bash
>>> from django.db.models.functions import Lower
>>> Blog.objects.values(lower_name=Lower("name"))
<QuerySet [{'lower_name': 'beatles blog'}]>

```

커스텀 lookup도 가능하다.
```bash
>>> from django.db.models import CharField
>>> from django.db.models.functions import Lower
>>> CharField.register_lookup(Lower)
>>> Blog.objects.values("name__lower")
<QuerySet [{'name__lower': 'beatles blog'}]>

```

foreign key관계인 field를 values에서 지정할 경우, 그 field명이 foo라면
foo, foo_id 양쪽 다 가능하다. foo로 했을 경우에는 딕셔너리의 key가 foo,
foo_id로 했을 경우에는 딕셔너리의 key가 foo_id로 반환된다.

```bash
>>> Entry.objects.values()
<QuerySet [{'blog_id': 1, 'headline': 'First Entry', ...}, ...]>

>>> Entry.objects.values("blog")
<QuerySet [{'blog': 1}, ...]>

>>> Entry.objects.values("blog_id")
<QuerySet [{'blog_id': 1}, ...]>

```


link
[[values_list]]

Reference
https://docs.djangoproject.com/en/4.2/ref/models/querysets/#django.db.models.query.QuerySet.values
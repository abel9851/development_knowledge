`only()`는 `values()`나 `values_list()`와 마찬가지로 db에 쿼리할 때 필요한 column만 지정해서 쿼리한다.
다른 점은 `only()` 는 모델 인스턴스를 반환하고 `values()`나 `values_list()`는 쿼리셋을 반환한다.
model serializer를 사용한다면 `only()`를 사용해야한다. 
`is_valid()` , `save()` 등, model serializer에서 행하는 메소드는 모델이 필요한게 많기 때문이다.

Reference
https://docs.djangoproject.com/en/4.2/ref/models/querysets/#django.db.models.query.QuerySet.only
https://books.agiliq.com/projects/django-orm-cookbook/en/latest/select_some_fields.html?highlight=only


Link
[[values]]
[[values_list]]
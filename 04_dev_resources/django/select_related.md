select_related를 사용하면 foreign key, one to one 관계가 있는 객체의 데이터를 prepopulate(미리 채워진, 미리 로드된)한다.

foreign key 관계에서는 정방향(Entry모델 안에 blog가 foreign key 필드로 정의되어 있다.), one to one관계에서는 역방향(아래의 코드로 설명하면, Entry모델은 blog에 관한 one to one 필드가 정의되어 있지 않고, Blog모델에 정의되어 있다. 이 때 Blog모델의 entry 필드에 related_name=blog라고 작성되어 있어야만 한다.)만 select_related를 사용할 수 있다.

```python
# Hits the database.
# Queryset은 lazy하지만, get을 호출했으므로 database에 hit한다.
# 이때 Entry 객체의 정보 뿐만 아니라 Entry.id=5와 관계된 blog 객체의 정보도 가져온다.
e = Entry.objects.select_related("blog").get(id=5)

# 위의 코드에서 database에서 정보를 미리 가져왔기 때문에 database에 hit하지 않는다.
b = e.blog

```

SQL로 확인하면 아래와 같다. INNER JOIN연산을 사용한다.

```SQL

SELECT entry.id, entry.some_field, ...,
	blog.id, blog.some_field,...
	FROM entry
	INNER JOIN blog on entry.blog_id = blog.id
	WHERE entry.id = 5;

```

Reference
https://docs.djangoproject.com/en/4.2/ref/models/querysets/#select-related
https://leffept.tistory.com/312
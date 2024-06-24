
foreignkey나 one to one field는 가리키고 있는 컬럼의 데이터타입과 같아야한다.
(일치하지 않으면 일관적이지 않고 실제 데이터베이스의 테이블에서도 foreignkey가 가리키고 있는 부모 테이블의 column과 데이터타입이 일치해야한다.)

이를 장고에서 구현하기 위해 `rel_db_type`을 사용한다.

```python

class Book(models.Model):
	pass

class Page(models.Model):
	address = models.CharField()
	Book = models.Foreignkey(Book, on_delete=models.CASCADE)


```

위의 코드에서 Page안에서 Book을 가리키는 foreignkey를 설정할 경우, Foreignkey 내부에서는 인자로 받은 Book을 `db_type`메소드 안에서 `Book.rel_db_type` 을 호출하기로 되어있다.
장고에서는 일반적으로 외래키를 설정할 때에는 id(int)를 사용하기 때문에 foreignkey는 가리키고 있는 Book의 id와 같은 데이터 타입을 갖게 된다.

- 해당 코드
```python

# django/db/models/fields/related.py class Foreignkey(ForeignObject) 1159lines
    def db_check(self, connection):
        return None

    def db_type(self, connection):
        return self.target_field.rel_db_type(connection=connection)

```


Reference
https://docs.djangoproject.com/en/4.2/howto/custom-model-fields/#custom-database-types
- db_type은 보통 데이터베이스의 타입을 string으로 직접 “char(25)”처럼 지정해서 리턴하는데 컬럼에 복잡한 SQL setup을 하고 싶다면, None을 리턴하면 된다.
    
    - 일반적으로 db_type을 리턴할 때의 코드
    
    ```python
    # This is a silly example of hard-coded parameters.
    class CharMaxlength25Field(models.Field):
        def db_type(self, connection):
            return "char(25)"
    
    # In the model:
    class MyModel(models.Model):
        # ...
        my_field = CharMaxlength25Field()
    ```
    
    - doc의 설명
    
    Finally, if your column requires truly complex SQL setup, return `None` from [db_type()](<https://devdocs.io/django~4.2/ref/models/fields#django.db.models.Field.db_type>). This will cause Django’s SQL creation code to skip over this field.
    

Reference
https://docs.djangoproject.com/en/4.2/howto/custom-model-fields/#custom-database-types
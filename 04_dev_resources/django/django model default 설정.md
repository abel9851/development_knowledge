```python

book = models.CharField(verbose_name="책", max_length=20, default='', null=False)

```

위의 모델의 Field처럼 default를 설정할 경우, 이 default는 장고 레벨에서만 적용된다.
장고에서 model의 인스턴스가 create될 때 사용되는 것이지, database의 book column에 default로 설정되는 것이 아니다.

위의 field를 migrate를 했을 경우 효과는 아래와 같다.
1. 기존의 records에 book column이 추가되고 값이 default의 ''"(empty string)으로 설정된다.
2. 장고 레벨에서 새롭게 model insance를 create할 때 book의 value를 지정하지 않으면 database에는 ''"(empty string)이 저장된다.

```python

# 장고에서 model을 create했을 때 SQL
# default로 설정한 값을 직접 넣어주는 형식이다.

INSERT INTO `desk` (`book`)
VALUES ('',)

```


주의할 것은, database의 column에는 default는 NULL로 설정되어 있어서 database에 직접 query할 경우에는 default값이 설정되지 않는다.

이는 `python manage.py dbshell` 로 database에 connection해서 `DESCRIBE my_tables;` 를 하면 table의 column의 정보를 확인 할 수 있는데 여기서 default가 NULL로 되어있는 것을 확인 할 수 있다.




Reference
https://docs.djangoproject.com/en/4.2/ref/models/fields/#default
https://stackoverflow.com/questions/32280343/default-value-of-djangos-model-doesnt-appear-in-sql
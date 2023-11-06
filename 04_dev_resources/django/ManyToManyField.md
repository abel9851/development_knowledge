ForeignKey와는 다르게 정의하는 곳은 어느 쪽이던 상관없지만 아래의 조건으로 고려해서
정의할 모델을 고르는 것이 좋다.
1. 의미적 명료함
   예를 들어 project와 task 두 모델이 있을 때, project가 여러개의 task를 갖을 수 있다.
   이럴 때에는 의미를 명확하게 파악하기 위해 project에 `ManyToManyField`를 정의한다.
2. query가 빈번한 쪽에 정의
   미묘한 차이지만 query를 많이 하는 쪽에서 ManyToMany Relationship을 갖은 모델을 참조할 경우,
   object manager의 이름이 field명으로 지정되어 있기 때문에 그렇다.
3. 모델 변경이 많지 않은 쪽에 정의
   
ManyToMany를 정의할 시, 두 테이블의 assciation table이 생성된다.
table의 이름은 ManyToMany를 정의한 모델의 이름과 field명으로 결정된다.
두 모델 사이에 추가적인 데이터를 정의하고 싶거나 두 모델 중 하나가 삭제되었을 시, 레코드가 삭제되도록 하고 싶다면 `through` 옵션을 사용해서 명시적으로 두 모델을 중개하는 모델을 생성하면 된다.

```python

class Project(models.Model):
	tasks = models.ManyToManyField("Task", releated_name="projects", verbose_name="related tasks",
	through="ProjectTaskAssociation")

class Task(models.Model):
	pass

class ProjectTaskAssociation(models.Model):
	project = models.ForeignKey("Project", related_name="task_association")
	task = models.ForeignKey("Task", related_name="project_association")

```
   
Reference
https://docs.djangoproject.com/en/4.2/ref/models/fields/#manytomanyfield

Link


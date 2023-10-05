`user.set_password({password})` 와 `user.save()` 를 하면 된다.


```python

user_1 = User.objects.get(username="test_user")
password_1 = "test1238928"
user_1.set_password(password_1)
user_1.save()

```



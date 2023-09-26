Case-insensitivae containement, 즉 대소문자를 구분하지 않는다.
`Entry.objects.get(headline__icontains="Lennon")`
SQL로 표현하자면 아래와 같다.
`SELECT ... WHERE headline LIKE "%Lennon%";`

구분할 경우의 SQL은 아래와 같다.
`SELECT ... WHERE BINARY headline LIKE "%Lennon%"
BINARY 키워드는 문자열 비교에서 대소문자를 구분하도록 MySQL에게 지시한다.

References
https://docs.djangoproject.com/en/4.2/ref/models/querysets/#icontains
https://www.w3schools.com/django/ref_lookups_icontains.php
테라폼에서는 다른 프로그래밍 언어와는 달리 선언형이기 때문에 for문은 선언형 구조에서만 사용할 수 있다.


아래는 그 예시다.
```

resource "aws_instance" "example" { for_each = toset(["us-west-1a", "us-west-1b", "us-west-1c"]) ami = "ami-0c55b159cbfafe1f0" instance_type = "t2.micro" subnet_id = element(aws_subnet.example[*].id, count.index) }


resource "aws_instance" "example" { count = 3 # Creates 3 instances ami = "ami-0c55b159cbfafe1f0" instance_type = "t2.micro" }

user_emails = {for user in users : user.name => user.email}

new_map = {for item in original_list : key_transform(item) => value_transform(item)}

```

이 방식은 사용할 수 없다. 왜냐하면 merge의 인자로 for loop가 사용되고 있기 때문이다.
사용하려면 map이나 콜렉션으로 감싸줘야한다.
```

merge(
	for item in items : [
		item
	]
)

```
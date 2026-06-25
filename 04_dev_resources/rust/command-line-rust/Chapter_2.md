- echo를 rust로 구현해본다.
- 사양
	- echo는 스페이스로 인자를 구분한다.
		- ex
			- `echo Rust has assumed control`을 입력하면 입력된 인자는 4개다.
	- 반면, quotes를 사용하면 하나의 인자로 처리된다.
		- ex
			- `echo "Rust has assumed control"`
	- `man echo`를 실행하면 매뉴얼이 출력된다.
	- -n 옵션은 맨 뒤에 줄바꿈을 하지 않도록 해준다.
- unit type은 반환할 값이 없다는 뜻이지만 실제 값을 가지는 타입이다. 가질 수 있는 값은 `()`뿐이다.
	- null pointer와는 다르다. rust에서는 null 참조를 허용하지 않는다.
	- `println!("Hello world");`는 아무것도 return하지 않는 것처럼보이지만 사실 unit type을 반환하고 있다.
- cli가 arguments를 찾을 때 environments를 참조한다.
- 아래의 코드를 `cargo run`하면(main은 생략) "format argument must be a string literal"라는 에러를 compiler가 출력한다. format이란, 데이터의 형식을 지정하여 인간이 읽을 수 있는 문자열 형태로 만드는 행위다. 여기서 말하는 데이터의 형식이란 출력되는 형태를 뜻한다. 소수점 데이터의 경우, 값이 3.3434라면 출력할 때 이를 3.34, 3.3 등으로 출력할 수 있다. 이러한 형태를 지정해야한다는 거다. 
	- 그리고 rust의 데이터를 출력하기 위해서는 그에 맞는 format string(틀)이 필요하다. 
		- "format argument must be a string literal"이라는 에러를 출력함과 동시에 `{}`를 집어넣으라는 내용도 같이 출력하는데 그 말대로 하면, 이번에는 `Display` trait이 없으니까 `{:?}`를 사용해보라는 내용이 출력된다. 출력된 내용대로 `{:?}`를 입력하면 그제서야 제대로 출력된다.
			- rust에서 trait란 추상적인 방식으로 객체의 행동을 정의하는 방법이다.
				- 추상적인 방식이란, 구체적인 구현내용은 제외하고, 이름과 규칙만 미리 정해두는 방식을 말한다.
				- trait란, 특징이다. 인간과 고래는 포유류니까 포유류의 특징인, "새끼를 낳아 젖을 먹여 키운다"라는 특징을 갖고 있다. 이런 특징을 미리 정의해 놓는 것이다. 이번에 compiler가 출력한 내용인 Display는 trait으로, 텍스트를 출력하고 싶다면, Display을 구현해라, 라고 나온것이다. 하지만 공식문서 https://doc.rust-lang.org/stable/std/env/struct.Args.html  Args의 Trait Implementations를 보면, Display는 존재하지 않는다. 대신 Debug를 보면compiler가 제안한 `{:?}`를 사용할 수 있다고 알 수 있다.(공식문서 내용은 어렵지만, compilier가 제안한 `{:?}`는 Args의 Trai Implementations의 Debug가 있으니, 이 Debug가 Args에 구현되어있으므로 Compiler는 거기서 `{:?}` 를 가져온 거라고 생각한다.)
```rust
println!(std::env::args()); // {}를 넣으라는 에러 발생
println!("{}", std::env::args()); // error[E0277]: `Args` doesn't implement `std::fmt::Display` 라는 에러가 발생함과 동시에 {:?}를 사용해보라는 내용이 출력된다.
```
#rust #2026년 #6월 #24일 #25일 #26일
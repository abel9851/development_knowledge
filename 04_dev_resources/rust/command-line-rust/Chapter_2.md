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

- `cargo.toml`에 정의한 crate는 root dir의 .cargo에 다운로드된다.
	- 그리고 해당 프로젝트에는 `target/debug/deps`에 crate가 인스톨된다.
		- version을 변경할 시, 새롭게 그 version이 `target/debug/deps`에 인스톨된다.
	- 프로젝트별로 version관리를 위해서 가상환경을 사용하는 파이썬과는 달리, cargo는 `.cargo`에 일단 crate를 다운로드 한 후, toml로 version을 지정하여 `target/deub/deps`에 해당하는 version을 두는 것으로, version을 관리한다.
		- 예를 들어 clap의 2.x외 3.x를 toml에 번갈아 지정해서 `cargo build`를 실행하면 `target/debug/deps`에 각각의 version이 인스톨된다.
			- 2.x, 3.x와 같이 알기 쉬운 이름으로 디렉토리가 만들어지진 않고 해쉬값과 같은 복잡한 값으로 디렉토리가 생성된다.
- docs의 version
	- command-line rust 책에서는 clap의 version이 2.x다. 26년 현재는 4.x까지 나왔다. 책이 발매된게 22년이니, 4년이나 지났고, version에 따라 예제 코드도 다르다. 저자가 봤던 시기의 코드는 code chaining을 해서 구현하는 식이었는데 현재의 예제는 struct, trait, derive를 사용해서 구현하는 식으로 바뀌었다.
		- docs를 읽었을 때에는 4.x 문서가 가장 먼저 보이므로, 저자가 쓴 코드를 그냥 그때 version에서는 이랬겠지, 라고 생각하고 지나칠려 했는데 version별 문서가 있다는 것을 떠올리고 2.x의 문서를 봤다.
			- 책에서 사용된 예제 코드와 똑같은 형식으로 되어있었다. 
				- 역시 실제로 직접 확인하는 것이 공식문서와 예제 코드를 매칭시킬수 있고, 공식문서를 읽는 방법도 더 효과적으로 습득할 수 있다는 것을 깨달았다.
					- 현재 프로젝트에서 사용하는 2.34의 문서 https://docs.rs/clap/2.34.0/clap/
-  --의 의미
	- double dash는 double dash 이전 부분은 실행한 명령어 혹은 서브 명령어의 옵션 부분으로 인식하게 하고, 그 이후는 인자로 인식하게 한다.
		- ex
			- `cargo run -- -n Hello World`
				- --가 없다면 -n은 cargo run의 옵션으로 인식된다.
- zen mode
	- 27일은 새벽 1시 40분정도에 눈이 떠졌기 때문에 책의 진도를 나가면서 현재 사용하고 있는 lazyvim의 zen mode를 사용해봤다. 상상한 것과와는 조금 달랐지만 lua로 옵션을 조절한다면 지금보다 더 집중해서 프로그래밍을 할 수 있을 것 같다.
		- 문제는, zen mode로 코딩할 때 space f t 로 터미널을 띄우면, zen mode는 취소되고 화면의 레이아웃이 상단부에는 편집하고 있었던 파일, 하단부 왼쪽에도 편집하고 있었던 파일, 하단부 오른쪽에는 터미널이 생성된다. 왜 이렇게 되는지 모르겠지만 이 문제를 해결해야 쓸만 할 것 같다.



#rust #2026년 #6월 #24일 #25일 #26일 #27일
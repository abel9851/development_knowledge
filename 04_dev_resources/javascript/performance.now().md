- 페이지 또는 스크립트의 실행이 시작된때부터 경과된 시간을 출력한다.
	- 18.20575, 즉 0.001820575초, 약 18밀리초 걸렸다는 거다.
- `Date.now()` 보다 더 정확한 시간을 제공한다.
- 고해상도 타임스탬프(high resolution timestamp)를 제공한다.
	- 빠른 연산을 평가(evaluate)할때 좀 더 정확하고 상세한 시간측정을 해준다.
- 테스트를 하거나 프로파일링(실행동작을 이해하기 위해 프로그램을 분석하는 과정, 프로파일링에서는 코드가 실행하는데 어떤 부분에서 실행됬는지, CPU, memory와 같은 자원을 얼마나 소비했는디, 얼마나 반복했는지, 얼마나 함수를 호출했는지 카운트를 하는등을 분석한다. 이 분석을 통해 개발자는 퍼포먼스, 애플리케이션의 효율성을 개선하기 위해 최적화를 진행하는데 사용한다.)할 때 연산의 더 정확한 지연시간을 이해하는데 사용할 수 있다.
- `Date.now` 보다 정확한 이유는 밀리초 이하의 정확성을 가진 timestamp를 제공하기 때문이다.
- `performance.now()` 는 밀리초 단위로 시간을 나타내는 부동 소수점 숫자를 반환하며, 종종 마이크로초(1/1,000,000초)까지의 정확도를 가지고 있다.
- [[Date.now()]]는 밀리초 단위(1/1,000초)로 시간을 제공한다.
- `performance.now()` 는 실행시간의 더 미세한 차이를 포착할 수 있어서 짧은 연산, 프로파일링 측정에 적합하다.
- `Date.Now()`는 높은 정확성이 요구되지 않는 연산과 일반적인 시간 추적에 사용된다.
- 내 생각에는 어느때나 더 정확성 높은 timestamp를 제공하는 `performnace.now()` 를 사용하면 좋을 것 같은데 ai는 아래와 같이 대답했다.
	- 즉, 소 잡는 칼로 닭을 잡을 필요 없다는 것, 친숙성도 관여하고 있고(보다 자신에게 편한 것), 일반적인 상황인 밀리초보다 더 긴 지연시간이 걸리는 것을 측정하거나 로깅하는데 `Date.now()` 를 사용한다는 것이다.
	
```plain text

While performance.now() is indeed more precise, it might not always be necessary or practical to use it for every timing task. Here are some reasons why developers might choose other methods like console.time()/console.timeEnd() or Date.now():

1. Simplicity and Convenience:

- console.time() and console.timeEnd() offer a simple way to measure time by just marking the start and end of a block of code with a single label.

1. Context:

- Date.now() is widely used for real-world timestamps. It's sufficient for general use cases where extreme precision isn't required, such as logging or measuring durations longer than a few milliseconds.

1. Readability and Familiarity:

- Developers may be more familiar with console methods, especially when debugging or logging in development environments.

1. Performance Considerations:

- While performance.now() itself doesn't significantly impact CPU or memory, it may be seen as overkill for simple tasks where such precision is unnecessary.

Developers choose tools based on the specific needs of their application, balancing precision, simplicity, and familiarity. In many cases, the precision offered by performance.now() is simply not required.
```

#javascript #시간측정  #time-measure
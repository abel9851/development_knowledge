```html

<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Vue Basics</title>
  <link href="https://fonts.googleapis.com/css2?family=Jost:wght@400;700&display=swap" rel="stylesheet" />
  <link rel="stylesheet" href="styles.css" />
  <script src="https://unpkg.com/vue@3.4.9/dist/vue.global.js" defer></script>
  <script src="app.js" defer></script>
</head>

<body>
  <header>
    <h1>Vue Course Goals</h1>
  </header>
  <section id="user-goal">
    <h2>My Course Goal</h2>
    <p>{{ courseGoal }}</p>
  </section>
</body>

</html>

```

```javascript

// data는 function이다. 즉, 이 function은 어딘가에서 호출된다. 그리고 이 function은 항상 객체를 반환해야하는데 이 객체는 key와 value를 갖고 있다.
// 
const app = Vue.createApp({
    data() {
        return {
	        courseGoal: 'Finish!'
        };
    }
});
app.mount('#user-goal');

```

- 주석에 적은대로, data 프로퍼티는 function이어야하며, 항상 객체를 반환한다.
- 반환하는 객체의 key와 value는 어떤 것이든 상관없다.
- data 프로퍼티의 근본적인 원리는, data프로퍼티가 반환하는 어떤 객체든 vue가 조정하는 HTML코드에서 사용할 수 있게 된다는 것이다.
- 정적 컨텐츠를 다루는 것이라면 평범할테지만 동적 컨텐츠, 예를 들어 사용자가 클릭해 데이터를 바꿀때 반응하도록 설정하도록 할 수 있게 되어 손쉽게 반응성을 추가할 수 있다.

Link
[[ mount의 역할 ]]
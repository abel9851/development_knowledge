
- data프로퍼티와는 다르게 methods에는 메소드 혹은 함수인 객체를 할당한다.
- 이 methods는 data프로퍼티처럼 보간구문인 이중 중괄호나 `v-bind`와 같은 vue directive에 사용가능하다.
- 이로서 알 수 있는 건, 보간구문이나 vue directive에 javascript코드를 사용할 수 있다는 것이다.

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
    <p>{{ courseGoal }}</p>
  </header>
  <section id="user-goal">
    <h2>My Course Goal</h2>
    <p>{{ courseGoal }}</p>
    <p>{{ outputAnyThing() }}</p>
    <p>test <a v-bind:href="vueLink">url</a></p>
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
            courseGoal: "Finish the course!",
            vueLink: "https://vuejs.org/"
        };
    },
    methods: {
        outputAnyThing() {
            const ran = Math.random();
            if (ran < 0.5) {
                return 'cool';
            } else {
                return 'coooool';
            }
        }
    }
});
app.mount('#user-goal');

```

![[Pasted image 20241217234407.png]]

Link
[[createApp의 data property]]
[[vue binding syntax - vue directive]]
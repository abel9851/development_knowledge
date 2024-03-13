```javascript

import { createApp, ref } from 'vue'

createApp({
  setup() {
    return {
      count: ref(0)
    }
  }
}).mount('#app')


```

```html

<div id="app">
  <button @click="count++">
    숫자 세기: {{ count }}
  </button>
</div>

```

mount는 Vue 애플리케이션 인터페이스를 특정 DOM요소에 연결(마운트)하는 데 사용된다.
즉 mount는 Vue 애플리케이션을 페이지의 실제 DOM구조에 삽입하는 과정을 담당한다.

- Vue애플리케이션의 시작점 설정: Vue애플리케이션을 실제DOM요소에 연결할 위치를 지정하고
- 랜더링 활성화: 지정된 DOM요소 내에 Vue애플리케이션과 컴포넌트 랜더링을 활성화하고
- 동적 UI 구현: 마운트된 요소 내에서 Vue는 데이터의 변화를 감지하고 이에 따라 DOM을 자동으로 업데이트하여
  동적인 사용자 인터페이스를 제공한다.

Reference
https://ko.vuejs.org/guide/introduction.html
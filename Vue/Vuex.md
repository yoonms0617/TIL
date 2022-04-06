# Vuex

- 데이터를 store에 저장하고, 프로젝트 전체에서 사용할 수 있도록 해주는 것이 Vuex이다.
- 모든 Vuex 애플리케이션의 중심에는 store가 있다.
- store는 애플리케이션이 상태를 저장하고 있는 컨테이너이다.
- Vuex store는 반응형으로 컴포는테이서 저장소의 상태를 검색할 때 정의된 변수 값의 변경 여부를 바로 알 수 있다.
- 저장소의 상태를 직접 변경할 수 없으며 변경하는 유일한 방법은 명시적인 커밋을 이용한 변경이다.

```javascript
import { createStore } from 'vuex'

const store = createStore({
  state: {
    count: 0
  },
  mutations: {
    increment (state) {
      state.count++
    }
  }
})

export default store;
```

vue 컴포넌트에서는 `this.$store`로 접근할 수 있다.

```html
<template>
  <p>Count : {{ count }} </p>
  <button type="button" @click="increment">Increment</button>
</tempate>

<script>
  export default {
    computed: {
      count() {
        return this.$store.state.count
      }
    },
    methods: {
      increment() {
        // 저장소의 state에 바로 접근해 변경하는 것이 아닌 commit을 통해서만 변경할 수 있다.
        this.$store.commit('increment')
      }
    }
  }
</script>
```

## Vuex 구조

```javascript
const store = createStore({
  state: {
  },
  getters: {
  },
  mutations: {
  },
  actions: {
  },
  modules: {
  }
})
```

### state

- 프로젝트 전체에서 공통으로 사용할 변수를 정의하는 곳
- state에 변수를 정의해 모든 컴포넌트에서 사용할 수 있다.
- State 관리를 통해 모든 컴포넌트에서 동일한 값을 사용할 수 있다.

```js
computed: {
  count () {
    return this.$store.state.count
  }
}
```

state에 정의된 변수는 Vue 컴포넌트에서 `computed` 속성을 이용해 변경 사항을 항상 추적할 수 있다.

### getters

- 저장소의 state에 변수로 관리하고 있다면 getters를 정의해 쉽게 데이터를 가져올 수 있다.

```js
const store = createStore({
  state: {
    count: 100
  },
  getters: {
    addCount: (state) => {
      return state.counter
    }
  }
})
// fooComponent.vue
<template>
  <p>{{ addCount }}</p>
</temapte>

...

computed: {
  addCount () {
    return this.$store.getters.addCount
  }
}
```

### mutations

- Vuex는 state에 정의된 변수를 직접 변경하는 것을 허용하지 않는다.
- 값을 변경하기 위해서는 반드시 mutations를 이용해 변경해야 한다. mutation은 state에 정의된 변수를 변경시키는 역할을 한다.
- 동기 처리를 통해 state에 정의된 변수의 변경사항을 추적할 수 있게 해준다.
- computed가 아닌 methods에 작성한다.
- setter라고 이해하면 된다.

```js
// store.js
mutation: {
  increment (state) {
    state.count++
  }
}
// fooComponent.vue
methods: {
  increment() {
    this.$store.commit('increment')
  }
}
```

#### actions

- mutations 비슷한 역할을 한다. actions를 통해 mutations에 정의된 함수를 실행 시킬 수 있다.
- mutations는 순차적(동기)인 로직들을 선언하고 actions에는 비순차적(비동기)인 로직들을 선언한다.
- actions에 등록된 함수는 비동기 처리 후 mutations을 커밋할 수 있어서 store에서 비동기 처리 로직을 관리할 수 있게 해준다.

```js
// store.js
export default createStore({
  ...
  mutations: {
    addCount (state) {
      state.count++
    }
  },
  actions: {
    addCount(contenxt) {
      // mutations의 addCount
      context.commit('addCount')
    }
  }
})
// fooComponent.vue
methods: {
  // mutations
  addCounter() {
    this.$store.commit('addCount')
  }
  // actions
  // actions를 사용할 때는 commit이 아닌 dispatch를 사용한다.
  addCounter() {
    this.$store.dispatch('addCount')
  }
}
```
---

#### 참고

- [Vue.js 프로젝트 투입 일주일 전](http://www.yes24.com/Product/Goods/101926719)

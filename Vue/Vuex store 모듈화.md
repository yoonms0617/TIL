# vuex store 모듈화

애플리케이션의 규모가 커짐에 따라 저장소는 매우 커지기 때문에 하나의 파일에서 모든 것을 관리하기에는 부담스럽다.

- 자체적인 state, getters, mutations, actions를 갖는 store 하위 객체를 만들고 store에서 이를 import해 사용한다.
- 분리된 서브 모듈역시 또 다른 서브 모듈을 가질 수 있다.

```js
// ../store/modules/catStore.js
const catStore = {
  state: {
    name: '나비'
  },
  getters: {
    getName (state) {
      return state.name
    }
  },
  mutations: {
    // local state
    changeName (state, payload) {
      state.name = paylaod
    }
  }
}

export default catStore
```
```js
// ../store/modules/dogStore.js
const dogStore = {
  state: {
    name: '누렁이'
  },
  getters: {
    getName (state) {
      return state.name
    }
  },
  mutations: {
    changeName (state, payload) {
      state.name = paylaod
    }
  }
}

export default dogStore
```
```js
// ../store/index.js
import { createStore } from 'vuex'
import catStore from '@/store/modules/catStore.js'
import dogStore from '@/store/modules/dogStore.js'

export default createStore({
  modules: {
    // key : value 형태로 저장
    cat: catStore,
    dog: dogStore
  }
})
```

<br/>

## store 구성 요소 접근 방법과 namespace 활용

```js
this.$store.getters.getName
this.$store.commit('changeName', 'fofofo')

// state에 접근할 때는 방법이 다르다. this.$store.state.모듈.state값
this.$store.state.cat.name
this.$store.state.dog.name
```

기본적으로 분리된 모듈 내의 actions, mutations, getters는 전역 namespace에서 관리되기 때문에 모듈간 서로 이름이 겹친다면 오류가 발생한다. 

모두 이름을 다르게 설정할 필요가 있는데 새로 이름을 짓는 작업은 매우 힘들다. 그래서 새로 이름을 짓는 것보다는 namespace를 활용하는 것이 좋다.

- 모듈에 `namespaced` 속성을 추가하고 값을 `true`로 설정하면 된다.

```js
// ../store/modules/catStore.js
const catStore = {
  namespaced: true
  ...
}
```

namespace를 적용했기 때문에 접근 방법도 달라졌다.

```js
this.$store.getters['cat/getName']
this.$store.getters['dog/getName']
this.$store.commit('cat/changeName', 'fofofo')
```

## map Helper

helper를 적용해 더욱 편하게 접근할 수 있다.

- mapState, mapGetters, mapMutations, mapActions
- vuex에서 각각 import 받아 사용하면 된다.
- ...helper로 접근

```js
import { mapGetters, mpaMutations } from 'vuex'

exprot default {
  computed: {
    /*
      2가지 방식으로 가져올 수 있다.
      1. 이름 지정해서 가져오기
      2. getters 그대로 가져오기
      
      각 요소는 this를 통해서 접근할 수 있다.
      this.catName
      this.getName
    */
    // 1번 방법
    ...mapGetters('cat', {
      catName: 'getName'
    }),
    // 2번 방법
    ...mapGetters('dog', [
      'getName'
    }]
  },
  method: {
    ...mapMutations('cat', {
      changeCatName: 'changeName'
    }),
    changeName (payload) {
      this.changeCatName(payload)
    }
  }
}
```

---
#### 참고

- <https://velog.io/@skyepodium/VUEX-store-%EC%97%AC%EB%9F%AC-%EA%B0%9C%EB%A5%BC-%EB%AA%A8%EB%93%88%ED%99%94%ED%95%98%EA%B8%B0>
- <https://goodteacher.tistory.com/439>

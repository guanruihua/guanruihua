---
title: vuex
date: 2021-02-05 16:12:27
tags:
- vuex
- vue
---



# vuex

> 状态管理模式来纪中管理状态或信息

## 使用vuex

store.js

```js
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

export default new Vuex.Strore({
  state: {
    count : 1,
    msg: 'msg',
  },
  mutations: {
    
  }, 
  action: {
    
  }
})
```

app.js

```js
export default {
  data(){
    return ({
      msg: 'hello',
    })
  },
  computed: {
    count() {
      return this.$store.state.count;//获取单个状态
    }
  }
}
```

## State

store.js

```js
import Vue from 'vue'
import Vuex from 'vuex'
Vue.use(Vuex);

export default new Vuex.Store({
  state: {
    count: 0,
    msg: 'msg'
  }
})
```

app.vue

```js
import { mapState } from 'vuex'
export defualt {
  data(){
    msg: 'grh',
  }
  //方法一
  computed: mapState({
    msg() {
     return this.msg + 'hello'; 
    },
    msg2(state) {
      return state.msg;
    }
  })
   //方法二: 使用解构符号...
    computed: {
    	...mapState(['count','msg']);
    }
}
```



## Getter

store.js

```js
import Vue from 'vue'
import Vuex from 'vuex'
Vue.use(Vuex);

export default new Vuex.Store({
  state: {
    count: 0,
    msg: 'msg',
    list: [1, 2, 3, 4]
  },
  getters: {
    modifyArr(state) {
      return state.list.filter((item, index, arr) =>{
      	return item % 2 == 0;
      })
    }
  }
})
```

app.js

```js
export default {
  //方法一
 	 computed: {
     list() {
       return this.$store.getters.modifyArr;
     }
   }
  //方法二 : mapGetters
  computed: {
  	...mapGetters(['modifyArr'])
    ...mapGetters({getList:'modifyArr'})//指定别名
	}
}
```



## Mutation

> - 可以修改store里面的状态
>
> - 必须是**同步函数**
>
> - 最好在store中先初始化所需要的属性
>
> - 当需要添加属性时, 使用Vue.set(obj, 'newProp', 123)或用新的对象替换老对象
>
>   - ```js
>     mutation: {
>       addNewState(state, payload){
>         Vue.set(state, 'newProp', '添加一个新的值');
>         //另外一个种
>         this.replaceStae({...state, newProp: '添加一个新值! ' })
>       }
>     }
>     
>     methods: {
>       addNewProp() {
>         this.$store.commit('addNewState', {});
>       }
>       newMsg() {
>         return this.$store.state.newProp || '还没有添加新值';
>       }
>     }
>     ```

```js
import { mapMutation } from 'vuex'
methods: {
  add() {
    this.$store.commit('add');
  },
  reduce() {
    this.$store.commit('reduce');
  },
  loadAdd() {
    
    this.$store.commit('loadAdd', 100)
    this.$store.commit('loadAdd', {
    	extraCount: 100
    })//传输额外的参数
    
    this.$store.commit({
      type:'addLoad'
      extraCount: 100
    },{}) //将多个写在一个上                  
  },
    
  //mapMutation using
  ...mapMutations([
    'increment',//将`this.increment`映射为'this.$store.commit(increment)'
    'incrementBy',//将'this.incrementBy(amount)'映射为'this.$store.commit('incrementBy', amount)'
  ])
  // 取别名
  ...mapMutations([
    add: 'increment',
    get: 'incrementBy',
  ])
  
}

//store.js
mutations: {
  add(state) {
    state.count++;
  },
  reduce(state) {
    state.count--;
  },
  loadAdd(state, payload) {  // 提交载荷，额外参数
    state.count += payload;
  },
},


```



## Action

> - 可以自行异步操作,
> - 类似于Mutation
> - 不可以直接修改state



```js
actions: {
  changeProduct(context, payload) { // 这个context是一个与 store 实例具有相同方法和属性的对象
    // 调用mutation里的changeProduct方法
    // context.commit('changeProduct', {change: 'ship'});
    // 改成异步方式
    // setTimeout(() => {
    //   context.commit('changeProduct', {change: 'ship'});
    // }, 1500)
    // 使用载荷
    let temp = 'ship+' + payload.extraInfo; 
    setTimeout(() => {
      context.commit('changeProduct', {change: temp});
    }, 1500)
  }
}


methods: {
  selectProduct() {
    // this.$store.dispatch('changeProduct')
    // 载荷方式分发
    // this.$store.dispatch('changeProduct', {
    //   extraInfo: 'sportcar'
    // })
    // 或者这种
    this.$store.dispatch({
      type: 'changeProduct',
      extraInfo: '->sportcar'
    })
  }
},
```

利用Promise

```js
state: {
	userInfo: {
  	name: 'guan',
    age:23,
  }
},
mutation: {
	changeInfo(state, payload) {
    state.userInfo.name = 'ruihua'
  }
},
actions: {
	changeInfo(context, payload) {
    return new Promise( (resolve, reject) => {
      setTimeout(() => {
        context.commit('changeInfo');
        resolove();
      },2000)
    })
  }
}
  
//app.js
  
data() {
  return {
    status: 'no changed',
  }
},
methods: {
  modifyInfo() {
		this.$store.dispatch('changeInfo')
  }
}
```

action可以相互调用

```js
actions: {
  actionA ({ commit }) {
    return new Promise((resolve, reject) => {
      setTimeout(() => {
        commit('someMutation')
        resolve()
      }, 1000)
    })
  },
  actionB ({ dispatch, commit }) {
    return dispatch('actionA').then(() => {//调用了actionA
      commit('someOtherMutation')
    })
  }
}

```







## Module

### 分模块管理方法

```js
//先定义两个模块
const moduleA = {
	state: {
  	name: 'guan',
    age: '23'
  },
  mutations: {},
  getters: {},
  action: {},
}

const moduleB = {
	state: {
  	name: 'rui',
    age: '22'
  },
  mutations: {},
  getters: {},
  action: {},
}

//在Vuex里面声明模块
export default new Vuex.Store({
	modules: {
  	ma: moduleA,
    mb: moduleB
  },
  state: {
    ...//其他状态
  },
})
  
  
//app.vue
compute: {
	msg() {
		return this.$store.mb; //{name: 'rui', age: '22'}	
	}
}
```



### 命名空间模块

```js
export const moduleC = {
  namespaced: true,
  state: {
    name: 'moduleC',
    desc: '这是模块C，用来测试命名空间的！',
    list: [1, 2, 3, 4]
  },
  getters: {
    filterList(state) {
      return state.list.filter((item, index, arrSelf) => {
        return item % 2 !== 0;
      });
    }
  },
  mutations: {
    modifyName(state, payload) {
      state.name = payload.newName;
    }
  },
  actions: {
    
  }
}
//store.js
import { moduleC } from './module_c.js';

export default new Vuex.Store({
  modules: {
    mc: moduleC
  },
})

methods: {
  modify() {
    // this.$store.commit('mc/modifyName', {
    //   newName: '命名空间模块C'
    // })
    this.$store.commit({
      type: 'mc/modifyName',
      newName: '命名空间模块C'
    })
  }
}


//命名空间发生改变后,mapState,mapGetters, mapMutations, mapActions用法改变
// 1.
...mapState('mc', ['name', 'age']);
...mapState('mc', {
	name(state) {
    return state.name;
  },
  age(state) {
    return state.age;
  }
})
// 2. 使用createNamespacedHelpers创建基于某个命名空间辅助函数
import { createNamespacedHelpers } from 'vuex';
const { mapState, mapMutations } = createNamespacedHelpers('mc');
```





```vue
<script>

import { mapGetters, mapState, mapMutations, mapActions } from 'vuex'

export default {
	computed:{
		...mapState('loginStore',['text', 'num']),
	},
	methods: {
    // 这里的loginStore 是 命名空间
		...mapGetters('loginStore', ['getData']),
		...mapMutations('loginStore',['add']),
		...mapActions('loginStore', ['addAction']),

}
</script>
```



## vuex与v-model对数据处理

```js
 <input v-model="message">
 computed: {
  message: {
    get () {
      return this.$store.state.obj.message
    },
    set (value) {
      this.$store.commit('updateMessage', value)
    }
  }
}


mutations: {
  updateMessage (state, message) {
    state.obj.message = message
  }
}

```


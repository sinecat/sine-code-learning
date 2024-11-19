## 理解Vuex
### Vuex是什么
1．概念:专门在 `Vue`中实现集中式状态（数据）管理的一个`Vue`**插件**，对`Vue`应用中多个组件的共享状态进行集中式的管理(读/写)，也是一种组件间通信的方式，且适用于任意组件间通信

![image-20221206191510747](https://october-x-image-host.oss-cn-hangzhou.aliyuncs.com/markdown-imgsimage-20221206191510747.png)

### ![image-20221206191515329](https://october-x-image-host.oss-cn-hangzhou.aliyuncs.com/markdown-imgsimage-20221206191515329.png)什么时候使用Vuex
1. 多个组件依赖于同一状态
2. 来自不同组件的行为需要变更同一状态

### Vuex工作原理图
### ![image-20221206191537231](https://october-x-image-host.oss-cn-hangzhou.aliyuncs.com/markdown-imgsimage-20221206191537231.png)
## 求和案例
### 使用纯vue编写
![image-20221206191541123](https://october-x-image-host.oss-cn-hangzhou.aliyuncs.com/markdown-imgsimage-20221206191541123.png)
src/App.vue

```vue
<template>
  <div>
    <Count/>
  </div>
</template>

<script>
import Count from "./components/Count.vue";

export default {
  name: "App",
  components: { Count },
};
</script>
```
src / components/ Count.vue
```vue
<template>
  <div>
    <h2>当前求和为：{{ sum }}</h2>
    <select v-model.number="n">
      <option value="1">1</option>
      <option value="2">2</option>
      <option value="3">3</option>
    </select>
    <button @click="increment">+</button>
    <button @click="decrement">-</button>
    <button @click="incrementOdd">当前求和为奇数再加</button>
    <button @click="incrementWait">等一等再加</button>
  </div>
</template>

<script>
  export default {
    name: "Count",
    data() {
      return {
        sum: 0, // 当前的和
        n: 1, // 用户选择的数字
      };
    },
    methods: {
      increment() {
        this.sum += this.n;
      },
      decrement() {
        this.sum -= this.n;
      },
      incrementOdd() {
        if (this.sum % 2) {
          this.sum += this.n;
        }
      },
      incrementWait() {
        setTimeout(() => {
          this.sum += this.n;
        }, 500);
      },
    },
  };
</script>

<style>
  button {margin-left: 5px;}
</style>
```
### 搭建Vuex环境
1．下载安装vuex `npm i vuex`

   - `npm i vuex`默认是安装`vuex4`，`vuex4`只能在`vue3`中使用。`vue2`中使用的话会出现如下报错

![image-20221206191613366](https://october-x-image-host.oss-cn-hangzhou.aliyuncs.com/markdown-imgsimage-20221206191613366.png)

   - `vue2`中使用`vuex3`版本，安装：`npm i vuex@3`

2．创建`src/store/index.js`该文件用于创建`vuex`中最为核心的`store`
```javascript
import Vue from 'vue'
import Vuex from 'vuex'	// 引入Vuex

Vue.use(Vuex)	// 应用Vuex插件

const actions = {}		// 准备actions——用于响应组件中的动作
const mutations = {}	// 准备mutations——用于操作数据（state）
const state = {}			// 准备state——用于存储数据

// 创建并暴露store
export default new Vuex.Store({
  actions,
  mutations,
  state,
})
```
3.在src/main.is 中创建vm时传入store配置项
```vue
import Vue from 'vue'
import App from './App.vue'
import store from './store'	// 引入store

Vue.config.productionTip = false

new Vue({
	el: '#app',
	render: h => h(App),
	store,										// 配置项添加store
	beforeCreate() {
		Vue.prototype.$bus = this
	}
})
```
### 使用Vuex编写Vuex的基本使用
1. 初始化数据`state`，配置`actions` 、 `mutations`，操作文件`store.js`

2. 组件中读取`vuex`中的数据`$store.state.数据`

3. 组件中修改vuex中的数据`$store.dispatch( 'action中的方法名',数据)`或`$store.commit( ' mutations中的方法名',数据)`若没有网络请求或其他业务逻辑，组件中也可越过`actions`，即不写`dispatch`，直接编写`commit`

**注意：import的引入执行的优先级比普通代码的高，所以import不管在哪里引入都相当于在最上面写的。**
`src/store/index.js `该文件用于创建vuex中最为核心的store
```javascript
import Vue from 'vue'
import Vuex from 'vuex'	// 引入Vuex

Vue.use(Vuex)	// 应用Vuex插件

// 准备actions——用于响应组件中的动作
const actions = {
  /* jia(context,value){
  console.log('actions中的jia被调用了')
  context.commit('JIA',value)
  },
  jian(context,value){
  console.log('actions中的jian被调用了')
  context.commit('JIAN',value)
  }, */
  jiaOdd(context,value){	// context 相当于精简版的 $store
    console.log('actions中的jiaOdd被调用了')
    if(context.state.sum % 2){
      context.commit('JIA',value)
    }
  },
  jiaWait(context,value){
    console.log('actions中的jiaWait被调用了')
    setTimeout(()=>{
      context.commit('JIA',value)
    },500)
  }
}
// 准备mutations——用于操作数据（state）
const mutations = {
  JIA(state,value){
    console.log('mutations中的JIA被调用了')
    state.sum += value
  },
  JIAN(state,value){
    console.log('mutations中的JIAN被调用了')
    state.sum -= value
  }
}
// 准备state——用于存储数据
const state = {
  sum:0 //当前的和
}

// 创建并暴露store
export default new Vuex.Store({
  actions,
  mutations,
  state,
})
```
src/ components/Count.vue
```vue
<template>
	<div>
		<h1>当前求和为：{{ $store.state.sum }}</h1>
		<select v-model.number="n">
			<option value="1">1</option>
			<option value="2">2</option>
			<option value="3">3</option>
		</select>
		<button @click="increment">+</button>
		<button @click="decrement">-</button>
		<button @click="incrementOdd">当前求和为奇数再加</button>
		<button @click="incrementWait">等一等再加</button>
	</div>
</template>

<script>
	export default {
		name:'Count',
		data() {
			return {
				n:1, //用户选择的数字
			}
		},
		methods: {
			increment(){
				this.$store.commit('JIA',this.n)
			},
			decrement(){
				this.$store.commit('JIAN',this.n)
			},
			incrementOdd(){
				this.$store.dispatch('jiaOdd',this.n)
			},
			incrementWait(){
				this.$store.dispatch('jiaWait',this.n)
			},
		}
	}
</script>

<style lang="css">button{margin-left: 5px;}</style>

```
## getters配置项
1. 概念:当`state`中的数据需要经过加工后再使用时，可以使用`**getters**` 加工，相当于**全局计算属性**
2. 在`store.js `中追加`**getters**`配置

```vue
......

const getters = {
	bigSum(state){
		return state.sum * 10
	}
}

// 创建并暴露store
export default new Vuex.Store({
	......
	getters
})
```
3. 组件中读取数据`$store.getters.bigSum`

![image-20221206191729571](https://october-x-image-host.oss-cn-hangzhou.aliyuncs.com/markdown-imgsimage-20221206191729571.png)
src/ store/index.js

```vue
import Vue from 'vue'	// 引入Vue核心库
import Vuex from 'vuex'	// 引入Vuex

Vue.use(Vuex)	// 应用Vuex插件
   
// 准备actions对象——响应组件中用户的动作
const actions = {
    addOdd(context,value){
        console.log("actions中的addOdd被调用了")
        if(context.state.sum % 2){context.commit('ADD',value)}
    },
    addWait(context,value){
        console.log("actions中的addWait被调用了")
        setTimeout(()=>{context.commit('ADD',value)},500)
    },
}
// 准备mutations对象——修改state中的数据
const mutations = {
    ADD(state,value){state.sum += value},
    SUB(state,value){state.sum -= value}
}
// 准备state对象——保存具体的数据
const state = {
    sum:0 // 当前的和
}
// 准备getters对象——用于将state中的数据进行加工
const getters = {
    bigSum(){
        return state.sum * 10
    }
}
   
//创建并暴露store
export default new Vuex.Store({
    actions,
    mutations,
    state,
    getters
})
```
src/ Count.vue
```vue
<template>
	<div>
		<h1>当前求和为：{{ $store.state.sum }}</h1>
		<h3>当前求和的10倍为：{{ $store.getters.bigSum }}</h3>
		<select v-model.number="n">
			<option value="1">1</option>
			<option value="2">2</option>
			<option value="3">3</option>
		</select>
		<button @click="increment">+</button>
		<button @click="decrement">-</button>
		<button @click="incrementOdd">当前求和为奇数再加</button>
		<button @click="incrementWait">等一等再加</button>
	</div>
</template>

<script>
	export default {
		name:'Count',
		data() {
			return {
				n:1,
			}
		},
		methods: {
			increment(){this.$store.commit('ADD',this.n)},
			decrement(){this.$store.commit('SUBTRACT',this.n)},
			incrementOdd(){this.$store.dispatch('addOdd',this.n)},
			incrementWait(){this.$store.dispatch('addWait',this.n)},
		},
	}
</script>

<style>button{margin-left: 5px;}</style>
```
## 四个map方法的使用
1. **mapstate方法**:用于帮助映射`state`中的数据为**计算属性**
```vue
computed: {
  	// 借助mapState生成计算属性：sum、school、subject（对象写法一）
  	...mapState({sum:'sum',school:'school',subject:'subject'}),

  	// 借助mapState生成计算属性：sum、school、subject（数组写法二）
  	...mapState(['sum','school','subject']),
},
```
2. **mapGetters方法**:用于帮助映射`getters`中的数据为**计算属性**
```vue
computed: {
    //借助mapGetters生成计算属性：bigSum（对象写法一）
    ...mapGetters({bigSum:'bigSum'}),

    //借助mapGetters生成计算属性：bigSum（数组写法二）
    ...mapGetters(['bigSum'])
},
```
3. **mapActions方法**:用于帮助生成与`actions`对话的方法，即包含`$store.dispatch(xxx)`的函数
```vue
methods:{
    //靠mapActions生成：incrementOdd、incrementWait（对象形式）
    ...mapActions({incrementOdd:'jiaOdd',incrementWait:'jiaWait'})

    //靠mapActions生成：incrementOdd、incrementWait（数组形式）
    ...mapActions(['jiaOdd','jiaWait'])
}
```

4. **mapMutations方法**:用于帮助生成与`mutations`对话的方法，即包含 `$store.commit(xxx)`的函数
```javascript
methods:{
	//靠mapActions生成：increment、decrement（对象形式）
	...mapMutations({increment:'JIA',decrement:'JIAN'}),
		
		//靠mapMutations生成：JIA、JIAN（对象形式）
		...mapMutations(['JIA','JIAN']),
}
```
**注意**:`mapActions` 与`mapMutations`使用时，若需要传递参数需要:**在模板中绑定事件时传递好参数**，否则参数是事件对象

`src/ components/Count.vue`
```vue
<template>
	<div>
		<h1>当前求和为：{{ sum }}</h1>
		<h3>当前求和的10倍为：{{ bigSum }}</h3>
		<h3>我是{{ name }}，我在{{ school }}学习</h3>
		<select v-model.number="n">
			<option value="1">1</option>
			<option value="2">2</option>
			<option value="3">3</option>
		</select>
		<button @click="increment(n)">+</button>
		<button @click="decrement(n)">-</button>
		<button @click="addOdd(n)">当前求和为奇数再加</button>
		<button @click="addWait(n)">等一等再加</button>
	</div>
</template>

<script>
	import {mapState, mapGetters, mapMutations, mapActions} from 'vuex'	//🔴

	export default {
		name: 'Count',
		data() {
			return {
				n:1, //用户选择的数字
			}
		},
  computed: {		
			...mapState(['sum','school','name']),
			...mapGetters(['bigSum'])
		},
		methods: {
			...mapMutations({increment:'ADD', decrement:'SUBTRACT'}),
			...mapActions(['addOdd', 'addWait'])
		},
	}
</script>

<style>
	button{
		margin-left: 5px;
	}
</style>
```
## 多组件共享数据案例
![image-20221206191747061](https://october-x-image-host.oss-cn-hangzhou.aliyuncs.com/markdown-imgsimage-20221206191747061.png)
src/App.vue

```vue
<template>
  <div>
    <Count/><hr/>
    <Person/>
  </div>
</template>

<script>
import Count from "./components/Count.vue";
import Person from "./components/Person.vue";

export default {
  name: "App",
  components: { Count, Person },
};
</script>
```
src/store/index.js 该文件用于创建Vuex中最为核心的store
```javascript
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

const actions = {
	jiaOdd(context,value){
		console.log('actions中的jiaOdd被调用了')
		if(context.state.sum % 2){
			context.commit('JIA',value)
		}
	},
	jiaWait(context,value){
		console.log('actions中的jiaWait被调用了')
		setTimeout(()=>{
			context.commit('JIA',value)
		},500)
	}
}

//准备mutations——用于操作数据（state）
const mutations = {
	JIA(state,value){
		console.log('mutations中的JIA被调用了')
		state.sum += value
	},
	JIAN(state,value){
		console.log('mutations中的JIAN被调用了')
		state.sum -= value
	},
	ADD_PERSON(state,value){
		console.log('mutations中的ADD_PERSON被调用了')
		state.personList.unshift(value)
	}
}

//准备state——用于存储数据
const state = {
	sum: 0,
	school: '尚硅谷',
	subject: '前端',
	personList: []
}

//准备getters——用于将state中的数据进行加工
const getters = {
	bigSum(state){
		return state.sum*10
	}
}

//创建并暴露store
export default new Vuex.Store({
	actions,
	mutations,
	state,
	getters
})
```
src / components/Count.vue
```vue
<template>
	<div>
		<h1>当前求和为：{{ sum }}</h1>
		<h3>当前求和放大10倍为：{{ bigSum }}</h3>
		<h3>我在{{ school }}，学习{{ subject }}</h3>
		<h3 style="color:red">Person组件的总人数是：{{ personList.length }}</h3>
		<select v-model.number="n">
			<option value="1">1</option>
			<option value="2">2</option>
			<option value="3">3</option>
		</select>
		<button @click="increment(n)">+</button>
		<button @click="decrement(n)">-</button>
		<button @click="incrementOdd(n)">当前求和为奇数再加</button>
		<button @click="incrementWait(n)">等一等再加</button>
	</div>
</template>

<script>
	import {mapState,mapGetters,mapMutations,mapActions} from 'vuex'
	export default {
		name:'Count',
		data() {
			return {
				n:1, //用户选择的数字
			}
		},
		computed:{
			...mapState(['sum','school','subject','personList']),
			...mapGetters(['bigSum'])
		},
		methods: {
			...mapMutations({increment:'JIA',decrement:'JIAN'}),
			...mapActions({incrementOdd:'jiaOdd',incrementWait:'jiaWait'})
		}
	}
</script>

<style lang="css">button{margin-left: 5px;}</style>
```
src / components/Person.vue
```vue
<template>
	<div>
		<h1>人员列表</h1>
		<h3 style="color:red">Count组件求和为：{{ sum }}</h3>
		<input type="text" placeholder="请输入名字" v-model="name">
		<button @click="add">添加</button>
		<ul>
			<li v-for="p in personList" :key="p.id">{{ p.name }}</li>
		</ul>
	</div>
</template>

<script>
	import {nanoid} from 'nanoid'
  import { mapState } from "vuex"
  
	export default {
		name:'Person',
		data() {
			return {
				name:''
			}
		},
		computed:{
			personList(){return this.$store.state.personList},
			sum(){return this.$store.state.sum}
		},
		methods: {
			add(){
        if (this.name === "") return
				const personObj = {id:nanoid(),name:this.name}
				this.$store.commit('ADD_PERSON',personObj)
				this.name = ''
			}
		},
	}
</script>
```
## 模块化+命名空间

1. 目的:让代码更好维护，让多种数据分类更加明确
2. 修改`store.js`

为了解决不同模块命名冲突的问题，将不同模块的`**namespaced: true **`，之后在不同页面中引入`getter ``actions``mutations` 时，需要加上所属的模块名

3. 在创建store的时候，设置`**strict: true**`即可开启严格模式。在严格模式下，只要状态变更不是通过mutation产生的，就会抛出错误。不过`state` 还是会被改变。
```javascript
const countAbout = {
  namespaced: true,	// 开启命名空间
  state: {x:1},
  mutations: { ... },
  actions: { ... },
  getters: {
    bigSum(state){ return state.sum * 10 }
  }
}

const personAbout = {
  namespaced: true,	// 开启命名空间
  state: { ... },
  mutations: { ... },
  actions: { ... }
}

const store = new Vuex.Store({
  modules: {
    countAbout,
    personAbout
  },
	strict: true
})
```
3.开启命名空间后，组件中读取`state`数据
```javascript
// 方式一：自己直接读取
this.$store.state.personAbout.list
// 方式二：借助mapState读取：
...mapState('countAbout',['sum','school','subject']),
```
4.开启命名空间后，组件中读取getters数据
```javascript
//方式一：自己直接读取
this.$store.getters['personAbout/firstPersonName']
//方式二：借助mapGetters读取：
...mapGetters('countAbout',['bigSum'])
```
5．开启命名空间后，组件中调用`dispatch`
```javascript
//方式一：自己直接dispatch
this.$store.dispatch('personAbout/addPersonWang',person)
//方式二：借助mapActions：
...mapActions('countAbout',{incrementOdd:'jiaOdd',incrementWait:'jiaWait'})
```
6．开启命名空间后，组件中调用`commit`
```javascript
//方式一：自己直接commit
this.$store.commit('personAbout/ADD_PERSON',person)
//方式二：借助mapMutations：
...mapMutations('countAbout',{increment:'JIA',decrement:'JIAN'}),
```
![image-20221206191800819](https://october-x-image-host.oss-cn-hangzhou.aliyuncs.com/markdown-imgsimage-20221206191800819.png)
src/ store/index.js
```javascript
import Vue from 'vue'
import Vuex from 'vuex'
import countOptions from './count'		// 引入count
import personOptions from './person'	// 引入person

Vue.use(Vuex)

//创建并暴露store
export default new Vuex.Store({
  modules:{
    countAbout:countOptions,
    personAbout:personOptions,
  }
})
```
src/ store/ count.js
```javascript
export default {
  namespaced:true,
  actions: {
    addOdd(context,value){
      console.log("actions中的addOdd被调用了")
      if(context.state.sum % 2){
        context.commit('ADD',value)
      }
    },
    addWait(context,value){
      console.log("actions中的addWait被调用了")
      setTimeout(()=>{
        context.commit('ADD',value)
      },500)
    }
  },
  mutations: {
    ADD(state,value){ state.sum += value },
    SUBTRACT(state,value){ state.sum -= value }
  },
  state: {
    sum:0,
    school:'尚硅谷',
    subject: '前端'
  },
  getters: {
    bigSum(state){ return state.sum * 10 }
  }
}
```
src/store/person.js
```vue
import axios from "axios"
import { nanoid } from "nanoid"

export default{
    namespaced:true,
    actions:{
        addPersonWang(context,value){
            if(value.name.indexOf('王') === 0){
                context.commit('ADD_PERSON',value)
            }else{
                alert('添加的人必须姓王！')
            }
        },
        addPersonServer(context){
            axios.get('http://api.uixsj.cn/hitokoto/get?type=social').then(
                response => {
                    context.commit('ADD_PERSON',{id:nanoid(),name:response.data})
                },
                error => { alert(error.message) }
            )
        }
    },
    mutations:{
        ADD_PERSON(state,value){
            console.log('mutations中的ADD_PERSON被调用了')
            state.personList.unshift(value)
        }
    },
    state:{
        personList:[]
    },
    getters:{
        firstPersonName(state){ return state.personList[0].name }
    }
}
```
src / components/Count.vue
```vue
<template>
	<div>
		<h1>当前求和为：{{ sum }}</h1>
		<h3>当前求和的10倍为：{{ bigSum }}</h3>
		<h3>我是{{ name }}，我在{{ school }}学习</h3>
		<h3 style="color:red">Person组件的总人数是：{{ personList.length }}</h3>
		<select v-model.number="n">
			<option value="1">1</option>
			<option value="2">2</option>
			<option value="3">3</option>
		</select>
		<button @click="increment(n)">+</button>
		<button @click="decrement(n)">-</button>
		<button @click="incrementOdd(n)">当前求和为奇数再加</button>
		<button @click="incrementWait(n)">等一等再加</button>
	</div>
</template>

<script>
	import {mapState,mapGetters,mapMutations,mapActions} from 'vuex'

	export default {
		name:'Count',
		data() {
			return {
				n:1, // 用户选择的数字
			}
		},
    computed:{
			...mapState('countAbout',['sum','school','name']),
      ...mapState('personAbout',['personList']),
			...mapGetters('countAbout',['bigSum']),
		}
		methods: {
			...mapMutations('countAbout',{increment:'ADD',decrement:'SUBTRACT'}),
			...mapActions('countAbout',{incrementOdd:'addOdd',incrementWait:'addWait'})
		},
	}
</script>

<style>button{margin-left: 5px;}</style>
```
src / components /Person.vue
```vue
<template>
	<div>
		<h1>人员列表</h1>
		<h3 style="color:red">Count组件求和为：{{ sum }}</h3>
        <h3>列表中第一个人的名字是：{{ firstPersonName }}</h3>
		<input type="text" placeholder="请输入名字" v-model="name">
		<button @click="add">添加</button>
        <button @click="addWang">添加一个姓王的人</button>
        <button @click="addPerson">随机添加一个人</button>
		<ul>
			<li v-for="p in personList" :key="p.id">{{ p.name }}</li>
		</ul>
	</div>
</template>

<script>
	import {nanoid} from 'nanoid'
	export default {
		name: 'Person',
		data() {
			return {
				name:''
			}
		},
		computed: {
			personList(){
				return this.$store.state.personAbout.personList
			},
			sum(){
				return this.$store.state.countAbout.sum
			},
      firstPersonName(){
        return this.$store.getters['personAbout/firstPersonName']
      }
		},
		methods: {
			add(){
				const personObj = {id:nanoid(),name:this.name}
				this.$store.commit('personAbout/ADD_PERSON',personObj)
				this.name = ''
			},
      addWang(){
        const personObj = {id:nanoid(),name:this.name}
        this.$store.dispatch('personAbout/addPersonWang',personObj)
        this.name = ''   
      },
      addPerson(){
        this.$store.dispatch('personAbout/addPersonServer')
      }
		},
	}
</script>
```

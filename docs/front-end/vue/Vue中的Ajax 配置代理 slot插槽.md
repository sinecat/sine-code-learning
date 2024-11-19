## Vue脚手架配置代理
![image-20221206191053328](https://october-x-image-host.oss-cn-hangzhou.aliyuncs.com/markdown-imgsimage-20221206191053328.png)
中间的为代理服务器，服务器与服务器之间通行不是用ajax。本来是使用nginx开启，我们可以借助vue-vli配置。

本案例需要下载`axios`库`npm install axios`

**配置参考文档**[Vue-Cli devServer.proxy](https://cli.vuejs.org/zh/config/#devserver-proxy)

`vue.config.js`是一个可选的配置文件，如果项目的(和`package.json`同级的)根目录中存在这个文件，那么它会被`@vue/cli-service`自动加载。你也可以使用`package.json`中的`vue`字段，但是注意这种写法需要你严格遵照JSON的格式来写

### **方法一**

在`vue.config.js` 中添加如下配置

```javascript
module.exports = {
  devServer:{
    proxy:"http://localhost:5000"
  }
}
```
说明

1. 优点:配置简单，请求资源时直接发给前端(8080)即可
2. 缺点:不能配置多个代理，不能灵活的控制请求是否走代理
3. 工作方式:若按照上述配置代理，当请求了前端不存在的资源时，才会将

请求会转发给服务器(优先匹配前端资源)

### **方法二**

编写`vue.config.js`配置具体代理规则

```javascript
module.exports = {
  devServer: {
    proxy: {
      '/api1': {													// 匹配所有以 '/api1'开头的请求路径
        target: 'http://localhost:5000',	// 代理目标的基础路径
        pathRewrite: {'^/api1':''},				// 代理往后端服务器的请求去掉 /api1 前缀
        ws: true,													// 用于支持webSocket
        changeOrigin: true,								// 默认为true，但是react相反
        
      },
      '/api2': {
        target: 'http://localhost:5001',
        pathRewrite: {'^/api2': ''},
        changeOrigin: true								// 用于控制请求头中的host值
      }
    }
  }
}
/*
changeOrigin设置为true时，服务器收到的请求头中的host为：localhost:5000
changeOrigin设置为false时，服务器收到的请求头中的host为：localhost:8080
changeOrigin默认值为true
*/
```
说明

1. 优点:可以配置多个代理，且可以灵活的控制请求是否走代理
2. 缺点:配置略微繁琐，请求资源时必须加前缀

`vue.config.js`
```javascript
module.exports = {
  pages: {
    index: {
      entry: 'src/main.js',
    },
  },
  lintOnSave:false,
  // 开启代理服务器（方式一）
  // devServer: {
  //     proxy:'http://localhost:5000'
  // }
  
  //开启代理服务器（方式二）
  devServer: {
    proxy: {
      '/api1': {
        target: 'http://localhost:5000',
        pathRewrite:{'^/api1':''},
        // ws: true, //用于支持websocket,默认值为true
        // changeOrigin: true //用于控制请求头中的host值,默认值为true
      },
      '/api2': {
        target: 'http://localhost:5001',
        pathRewrite:{'^/api2':''},
      }
    }
  }
}
```
`src/App.vue`
```vue
<template>
<div>
  <button @click="getStudents">获取学生信息</button>
  <button @click="getCars">获取汽车信息</button>
  </div>
</template>

<script>
  import axios from 'axios'
  export default {
    name:'App',
    methods: {
      getStudents() {
        axios.get('http://localhost:8080/students').then(
          response => {
            console.log('请求成功了',response.data)
          },
          error => {
            console.log('请求失败了',error.message)
          }
        )
      },
      getCars() {
        axios.get('http://localhost:8080/demo/cars').then(
          response => {
            console.log('请求成功了',response.data)
          },
          error => {
            console.log('请求失败了',error.message)
          }
        )
      }
    },
  }
</script>
```
![image-20221206191218394](https://october-x-image-host.oss-cn-hangzhou.aliyuncs.com/markdown-imgsimage-20221206191218394.png)
## GitHub用户搜索案例
`public/index.html`
```
<!DOCTYPE html>
<html lang="">
    <head>
        <meta charset="UTF-8">
        <!-- 针对IE浏览器的特殊配置，含义是让IE浏览器以最高渲染级别渲染页面 -->
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <!-- 开启移动端的理想端口 -->
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <!-- 配置页签图标 -->
        <link rel="icon" href="<%= BASE_URL %>favicon.ico">
      
        <!-- 引入bootstrap样式 -->
        <link rel="stylesheet" href="<%= BASE_URL %>css/bootstrap.css">
      
        <!-- 配置网页标题 -->
        <title><%= htmlWebpackPlugin.options.title %></title>
    </head>
    <body>
        <!-- 容器 -->
        <div id="app"></div>
    </body>
</html>
```
`src/App.vue`
```vue
<template>
  <div class="container">
    <Search/>
    <List/>
  </div>
</template>

<script>
  import Search from './components/Search.vue'
  import List from './components/List.vue'

  export default {
    name:'App',
    components:{ Search, List },
  }
</script>
```
`src/App.cue`
```vue
<template>
  <div class="container">
    <Search/>
    <List/>
  </div>
</template>

<script>
  import Search from './components/Search.vue'
  import List from './components/List.vue'

  export default {
    name:'App',
    components:{ Search, List },
  }
</script>
```
`src/ components /Search.vue`
```vue
<template>
  <section class="jumbotron">
    <h3 class="jumbotron-heading">Search Github Users</h3>
    <div>
      <input type="text" placeholder="enter the name you search" v-model="keyWord"/>&nbsp;
      <button @click="searchUsers">Search</button>
    </div>
  </section>
</template>

<script>
import axios from "axios";
export default {
  name: "Search",
  data() {
    return {
      keyWord: "",
    };
  },
  methods: {
    searchUsers() {
      //请求前更新List的数据
      this.$bus.$emit("updateListData", {
        isLoading: true,
        errMsg: "",
        users: [],
        isFirst: false,
      });
      axios.get(`https://api.github.com/search/users?q=${this.keyWord}`).then(
        (response) => {
          console.log("请求成功了");
          this.$bus.$emit("updateListData", {	//请求成功后更新List的数据
            isLoading: false,
            errMsg: "",
            users: response.data.items,
          });
        },
        (error) => {
          this.$bus.$emit("updateListData", {	//请求后更新List的数据
            isLoading: false,
            errMsg: error.message,
            users: [],
          });
        }
      );
    },
  },
};
</script>
```
`src / components /List.vue`
```vue
<template>
  <div class="row">
    <!-- 展示用户列表 -->
    <div v-show="info.users.length" class="card" 
         v-for="user in info.users" :key="user.login">
      <a :href="user.html_url" target="_blank">
        <img :src="user.avatar_url" style="width: 100px" />
      </a>
      <p class="card-text">{{ user.login }}</p>
    </div>
    <!-- 展示欢迎词 -->
    <h1 v-show="info.isFirst">欢迎使用！</h1>
    <!-- 展示加载中 -->
    <h1 v-show="info.isLoading">加载中....</h1>
    <!-- 展示错误信息 -->
    <h1 v-show="info.errMsg">{{ info.errMsg }}</h1>
  </div>
</template>

<script>
export default {
  name: "List",
  data() {
    return {
      info: {
        isFirst: true,
        isLoading: false,
        errMsg: "",
        users: [],
      },
    };
  },
  mounted() {
    this.$bus.$on("updateListData", (dataObj) => {
      this.info = { ...this.info, ...dataObj };
    });
  },
};
</script>

<style scoped>
.album {min-height: 50rem; /* Can be removed; just added for demo purposes */
  padding-top: 3rem;padding-bottom: 3rem;background-color: #f7f7f7;}
.card {float: left;width: 33.333%;padding: 0.75rem;margin-bottom: 2rem;
  border: 1px solid #efefef;text-align: center;}
.card > img {margin-bottom: 0.75rem;border-radius: 100px;}
.card-text {font-size: 85%;}
</style>
```
![image-20221206191224515](https://october-x-image-host.oss-cn-hangzhou.aliyuncs.com/markdown-imgsimage-20221206191224515.png)
## vue-resource
`vue`项目常用的两个`Ajax`库

1. `axios` :通用的Ajax请求库，官方推荐，效率高

2. `vue-resource` : vue插件库，vue 1.x使用广泛，官方已不维护
    下载`** vue-resource**`库`**npm i vue-resource**`

`src/main.js`
```javascript
import Vue from 'vue'
import App from './App.vue'
//引入插件
import vueResource from 'vue-resource'

Vue.config.productionTip = false
//使用插件
Vue.use(vueResource)

new Vue({
  el:"#app",
  render: h => h(App),
  beforeCreate(){
    Vue.prototype.$bus = this
  }
})
```
`src/App.vue`
```vue
<template>
	<div class="container">
		<Search/>
		<List/>
	</div>
</template>

<script>
	import Search from './components/Search.vue'
	import List from './components/List.vue'

    export default {
        name:'App',
		components:{ Search, List },
	}
</script>
```
`src / components/Search.vue`
```vue
<template>
    <section class="jumbotron">
		<h3 class="jumbotron-heading">Search Github Users</h3>
		<div>
      <input type="text" placeholder="enter the name you search" v-model="keyWord"/>&nbsp;
      <button @click="getUsers">Search</button>
		</div>
    </section>
</template>

<script>
  export default {
    name:'Search',
    data() {
      return {
        keyWord:''
      }
    },
    methods: {
      getUsers(){
        //请求前更新List的数据
        this.$bus.$emit('updateListData',
                        {isLoading:true,errMsg:'',users:[],isFirst:false})
        //和axios的使用是axios.get，后面几乎差不多。
        this.$http.get(`https://api.github.com/search/users?q=${this.keyWord}`).then(
          response => {
            console.log('请求成功了')
            //请求成功后更新List的数据
            this.$bus.$emit('updateListData',
                            {isLoading:false,errMsg:'',users:response.data.items})
          },
          error => {
            //请求后更新List的数据
            this.$bus.$emit('updateListData',
                            {isLoading:false,errMsg:error.message,users:[]})
          }
        )
      }
    }
  }
</script>
```
`src/ components /List.vue`
```vue
<template>
    <div class="row">
        <!-- 展示用户列表 -->
        <div class="card" v-show="info.users.length" v-for="user in info.users" :key="user.id">
            <a :href="user.html_url" target="_blank">
                <img :src="user.avatar_url" style='width: 100px'/>
            </a>
            <h4 class="card-title">{{user.login}}</h4>
        </div>
        <!-- 展示欢迎词 -->
        <h1 v-show="info.isFirst">欢迎使用！</h1>
        <!-- 展示加载中 -->
        <h1 v-show="info.isLoading">加载中...</h1>
        <!-- 展示错误信息 -->
        <h1 v-show="info.errMsg">{{errMsg}}</h1>
    </div>
</template>

<script>
    export default {
        name:'List',
        data() {
            return {
                info:{
                    isFirst:true,
                    isLoading:false,
                    errMsg:'',
                    users:[]
                }
            }
        },
        mounted(){
            this.$bus.$on('updateListData',(dataObj)=>{
                this.info = {...this.info,...dataObj}
            })
        },
        beforeDestroy(){
            this.$bus.$off('updateListData')
        }
    }
</script>
```
## slot插槽
`<slot>`**插槽:**让父组件可以向子组件指定位置插入`html` 结构，也是一种组件间通信的方式，
                                适用于**父组件===>子组件**

1. 分类:默认插槽、具名插槽、作用域插槽

2. 使用方式

   

默认插槽

```vue
父组件中：
        <Category>
           <div>html结构1</div>
        </Category>
子组件中：Category
        <template>
            <div>
               <!-- 定义插槽 -->
               <slot>插槽默认内容...</slot>
            </div>
        </template>
```
          b．具名插槽
                  父组件指明放入子组件的哪个插槽`**slot="footer"**`，如果是`template`可以写成`**v-slot:footer**`
```vue
父组件中：
        <Category>
            <template slot="center">
              <div>html结构1</div>
            </template>

            <template v-slot:footer>
               <div>html结构2</div>
            </template>
        </Category>
子组件中：
        <template>
            <div>
               <!-- 定义插槽 -->
               <slot name="center">插槽默认内容...</slot>
               <slot name="footer">插槽默认内容...</slot>
            </div>
        </template>
```
  	c.  作用域宙槽
  	     `**scope**`用于父组件往子组件插槽放的`html`结构接收子组件的数据
  	      理解:**数据在组件的自身，但根据数据生成的结构需要组件的使用者来决定**
  	       ( `games` 数据在`Category` 组件中，但使用数据所遍历出来的结构由`App`组件决定)
```vue
父组件中：
        <Category>
            <template scope="scopeData">
                <!-- 生成的是ul列表 -->
                <ul>
                  <li v-for="g in scopeData.games" :key="g">{{g}}</li>
                </ul>
            </template>
        </Category>

        <Category>
            <template slot-scope="scopeData">
                <!-- 生成的是h4标题 -->
                <h4 v-for="g in scopeData.games" :key="g">{{g}}</h4>
            </template>
        </Category>
子组件中：
        <template>
            <div>
                <slot :games="games"></slot>
            </div>
        </template>
		
        <script>
            export default {
                name:'Category',
                props:['title'],
                //数据在子组件自身
                data() {
                    return {
                        games:['红色警戒','穿越火线','劲舞团','超级玛丽']
                    }
                },
            }
        </script>
```
**注意**:关于**样式**，既可以写在父组件中，解析后放入子组件插槽;也可以放在子组件中，传给子组件再解析
### 默认插槽
`src/App. vue`
```vue
<template>
	<div class="container">
		<Category title="美食" >
			<img src="https://s3.ax1x.com/2021/01/16/srJlq0.jpg" alt="">
		</Category>

		<Category title="游戏" >
			<ul>
				<li v-for="(g,index) in games" :key="index">{{g}}</li>
			</ul>
		</Category>

		<Category title="电影">
			<video controls src="http://clips.vorwaerts-gmbh.de/big_buck_bunny.mp4"></video>
		</Category>
	</div>
</template>

<script>
	import Category from './components/Category'
	export default {
		name:'App',
		components:{ Category },
		data() {
			return {
				foods:['火锅','烧烤','小龙虾','牛排'],
				games:['红色警戒','穿越火线','劲舞团','超级玛丽'],
				films:['《教父》','《拆弹专家》','《你好，李焕英》','《尚硅谷》']
			}
		},
	}
</script>

<style scoped>.container{display: flex;justify-content: space-around;}</style>
```
`src/ components/Category.vue`
```vue
<template>
	<div class="category">
		<h3>{{ title }}分类</h3>
		<!-- 定义一个插槽（挖个坑，等着组件的使用者进行填充） -->
		<slot>我是一些默认值，当使用者没有传递具体结构时，我会出现</slot>
	</div>
</template>

<script>
	export default {
		name:'Category',
		props:['title']
	}
</script>

<style scoped>
	.category {background-color: skyblue;width: 200px;height: 300px;}
	h3 {text-align: center;background-color: orange;}
	video {width: 100%;}
	img {width: 100%;}
</style>
```
### ![image-20221206191335291](https://october-x-image-host.oss-cn-hangzhou.aliyuncs.com/markdown-imgsimage-20221206191335291.png)具名插槽
`src/ App . vue`
```vue
<template>
	<div class="container">
		<Category title="美食" >
			<img slot="conter" src="https://s3.ax1x.com/2021/01/16/srJlq0.jpg" alt="">
			<a slot="footer" href="http://www.atguigu.com">更多美食</a>
		</Category>

		<Category title="游戏" >
			<ul slot="center">
				<li v-for="(g,index) in games" :key="index">{{g}}</li>
			</ul>
			<div class="foot" slot="footer">
				<a href="http://www.atguigu.com">单机游戏</a>
				<a href="http://www.atguigu.com">网络游戏</a>
			</div>
		</Category>

		<Category title="电影">
			<video slot="center" controls src="http://clips.vorwaerts-gmbh.de/big_buck_bunny.mp4"></video>
			<template v-slot:footer>
				<div class="foot">
					<a href="http://www.atguigu.com">经典</a>
					<a href="http://www.atguigu.com">热门</a>
					<a href="http://www.atguigu.com">推荐</a>
				</div>
				<h4>欢迎前来观影</h4>
			</template>
		</Category>
	</div>
</template>

<script>
	import Category from './components/Category'
	export default {
		name:'App',
		components:{Category},
		data() {
			return {
				foods:['火锅','烧烤','小龙虾','牛排'],
				games:['红色警戒','穿越火线','劲舞团','超级玛丽'],
				films:['《教父》','《拆弹专家》','《你好，李焕英》','《尚硅谷》']
			}
		},
	}
</script>

<style scoped>
	.container,.foot{display: flex;justify-content: space-around;}
	h4{text-align: center;}
</style>

```
`src / components/Category.vue`
```vue
<template>
	<div class="category">
		<h3>{{title}}分类</h3>
		<!-- 定义一个插槽（挖个坑，等着组件的使用者进行填充） -->
		<slot name="center">我是一些默认值，当使用者没有传递具体结构时，我会出现1</slot>
		<slot name="footer">我是一些默认值，当使用者没有传递具体结构时，我会出现2</slot>
	</div>
</template>

<script>
	export default {
		name:'Category',
		props:['title']
	}
</script>

<style scoped>
	.category{background-color: skyblue;width: 200px;height: 300px;}
	h3{text-align: center;background-color: orange;}
	video{width: 100%;}
	img{width: 100%;}
</style>
```
### ![image-20221206191345542](https://october-x-image-host.oss-cn-hangzhou.aliyuncs.com/markdown-imgsimage-20221206191345542.png)作用域插槽
`src/ App.vue`
```vue
<template>
	<div class="container">

		<Category title="游戏">
			<template scope="atguigu">
				<ul>
					<li v-for="(g,index) in atguigu.games" :key="index">{{g}}</li>
				</ul>
			</template>
		</Category>

		<Category title="游戏">
			<template scope="{games}">
				<ol>
					<li style="color:red" v-for="(g,index) in games" :key="index">{{g}}</li>
				</ol>
			</template>
		</Category>

		<Category title="游戏">
			<template slot-scope="{games}">
				<h4 v-for="(g,index) in games" :key="index">{{g}}</h4>
			</template>
		</Category>
	</div>
</template>

<script>
	import Category from './components/Category'
	export default {
		name:'App',
		components:{ Category },
	}
</script>

<style scoped>
	.container,.foot{display: flex;justify-content: space-around;}
	h4{text-align: center;}
</style>

```
`src / components/Category .vue`
```vue
<template>
	<div class="category">
		<h3>{{title}}分类</h3>
		<slot :games="games" msg="hello">我是默认的一些内容</slot>
	</div>
</template>

<script>
	export default {
		name:'Category',
		props:['title'],
		data() {
			return {
				games:['红色警戒','穿越火线','劲舞团','超级玛丽'],
			}
		},
	}
</script>

<style scoped>
	.category{background-color: skyblue;width: 200px;height: 300px;}
	h3{text-align: center;background-color: orange;}
	video{width: 100%;}
  img{width: 100%;}
</style>
```
![image-20221206191354516](https://october-x-image-host.oss-cn-hangzhou.aliyuncs.com/markdown-imgsimage-20221206191354516.png)

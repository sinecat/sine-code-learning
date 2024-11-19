## 模块与组件、模块化与组件化
![image-20221206190310987](https://october-x-image-host.oss-cn-hangzhou.aliyuncs.com/markdown-imgsimage-20221206190310987.png)
![](https://cdn.nlark.com/yuque/0/2022/png/1379492/1643034111832-4a659e2d-4a13-4944-a153-ab038b65cbf0.png#crop=0&crop=0&crop=1&crop=1&from=url&id=uMiel&margin=%5Bobject%20Object%5D&originHeight=972&originWidth=1727&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)
**模块**

- 理解:向外提供特定功能的js 程序，一般就是一个js文件

- 为什么: js文件很多很复杂

- 作用:复用、简化js的编写，提高js运行效率

    

 **组件**

- 定义:**用来实现局部功能的代码和资源的集合**
    (html/css/js/image... )
- 为什么:一个界面的功能很复杂
- 作用:复用编码，简化项目编码，提高运行效率

**模块化**
当应用中的js都以模块来编写的，那这个应用就是一个模块化的应用
**组件化**
当应用中的功能都是多组件的方式来编写的，那这个应用就是一个组件化的应用

### 非单文件组件
**非单文件组件**:一个文件中包含有n个组件
**单文件组件**:一个文件中只包含有1个组件
#### 基本使用
`vue`中使用组件的三大步骤
**1. 定义组件**
		使用`Vue.extend(options）`创建，其中`options`和`new Vue(options)`时传入的`options`几乎—样，但也有点区别

- `el`不要写，因为最终所有的组件都要经过一个`vm`的管理，由`vm`中的`el`才决定服务哪个容器       

- `data` 必须写成函数，避免组件被复用时，数据存在引用关系。（如果写成对象会报错，假设可以写成对象，组件更改了data，会导致所有引用的地方都更改。）

**2. 注册组件**

- 局部注册:`new Vue( 	)`的时候`options`传入`components`选项
- 全局注册:`vue.component( '组件名',组件)`

**3. 使用组件**
编写组件标签如**<school></school>**

```html
<title>基本使用</title>
<script type="text/javascript" src="../js/vue.js"></script>

<div id="root">
  <h2>{{msg}}</h2><hr>
  <!-- 第三步：编写组件标签 -->
  <school></school><hr>
  <student></student><hr>
  <hello></hello><hr>
</div>

<div id="root2">
  <hello></hello>
</div>

<script type="text/javascript">
  Vue.config.productionTip = false

  //第一步：创建school组件
  const school = Vue.extend({
    // el:'#root', //组件定义时，一定不要写el配置项，
    // 因为最终所有的组件都要被一个vm管理，由vm决定服务于哪个容器
    template: `
				<div class="demo">
					<h3>学校名称：{{schoolName}}</h3>
					<h3>学校地址：{{address}}</h3>
					<button @click="showName">点我提示学校名</button>	
  			</div>
			`,
    data() {
      return {
        schoolName: '尚硅谷',
        address: '北京昌平'
      }
    },
    methods: {
      showName() {
        alert(this.schoolName)
      }
    },
  })

  //第一步：创建student组件
  const student = Vue.extend({
    template: `
				<div>
					<h3>学生姓名：{{studentName}}</h3>
					<h3>学生年龄：{{age}}</h3>
  			</div>
			`,
    data() {
      return {
        studentName: '张三',
        age: 18
      }
    }
  })

  //第一步：创建hello组件
  const hello = Vue.extend({
    template: `
				<div>	
					<h3>你好啊！{{name}}</h3>
  			</div>
			`,
    data() {
      return {
        name: 'cess'
      }
    }
  })

  //第二步：全局注册组件
  Vue.component('hello', hello)

  //创建vm
  new Vue({
    el: '#root',
    data: {
      msg: '你好啊！'
    },
    //第二步：注册组件（局部注册）
    components: {
      school,
      student
    }
  })

  new Vue({
    el: '#root2',
  })
</script>
```
![image-20221206190517958](https://october-x-image-host.oss-cn-hangzhou.aliyuncs.com/markdown-imgsimage-20221206190517958.png)
#### 组件注意事项
**关于组件名**

- —个单词组成
   -  第一种写法(首字母小写) : school
   - 第二种写法（**首字母大写**) : school
- 多个单词组成
   - 第—种写法(kebab-case命名) : my-school
   - 第二种写法(**CamelCase命名**): MySchool (需要`Vue`脚手架支持)
- 备注
   - 组件名尽可能回避`HTML`中已有的元素名称，例如:h2、H2都不行
   - 可以使用`**name**` 配置项指定组件在开发者工具中呈现的名字

**关于组件标签**
第一种写法:`<school></ school>`
第二种写法:`**<school/>**`(需要Vue脚手架支持)
**备注**:

不使用脚手架时，`<school/>`会导致后续组件不能渲染
**一个简写方式**: 

`const school = Vue.extend(options）`可简写为`const school = options`，因为父组件`components` 引入的时候会自动创建

```html
<title>几个注意点</title>
	<script type="text/javascript" src="../js/vue.js"></script>

	<div id="root">
		<h2>{{msg}}</h2>
		<school></school>
	</div>

<script type="text/javascript">
	Vue.config.productionTip = false

	//定义组件
	const school = Vue.extend({
		name: 'atguigu', // 组件给自己起个名字，用于在浏览器开发工具上显示
		template: `
				<div>
					<h3>学校名称：{{name}}</h3>	
					<h3>学校地址：{{address}}</h3>	
				</div>
			`,
		data() {
			return {
				name: '电子科技大学',
				address: '成都'
			}
		}
	})

	new Vue({
		el: '#root',
		data: {
			msg: '欢迎学习Vue!'
		},
		components: {
			school
		}
	})
</script>
```
![image-20221206190554125](https://october-x-image-host.oss-cn-hangzhou.aliyuncs.com/markdown-imgsimage-20221206190554125.png)
#### 组件的嵌套
![image-20221206190600259](https://october-x-image-host.oss-cn-hangzhou.aliyuncs.com/markdown-imgsimage-20221206190600259.png)
```html
<title>组件的嵌套</title>
<script type="text/javascript" src="../js/vue.js"></script>

<div id="root"></div>

<script type="text/javascript">
  Vue.config.productionTip = false

  //定义student组件
  const student = Vue.extend({
    name: 'student',
    template: `
				<div>
					<h4>学生姓名：{{name}}</h4>	
					<h4>学生年龄：{{age}}</h4>	
  			</div>
			`,
    data() {return {name: '尚硅谷',age: 18}}
  })

  //定义school组件
  const school = Vue.extend({
    name: 'school',
    template: `
				<div>
					<h3>学校名称：{{name}}</h3>	
					<h3>学校地址：{{address}}</h3>	
					<student></student>
 			  </div>
			`,
    data() {return {name: '尚硅谷',address: '北京'}},
    //注册组件（局部）
    components: { student }
  })

  //定义hello组件
  const hello = Vue.extend({
    template: `<h3>{{msg}}</h3>`,
    data() {return {msg: '欢迎来到尚硅谷学习！'}}
  })

  //定义app组件
  const app = Vue.extend({
    template: `
				<div>	
					<hello></hello>
					<school></school>
  			</div>
			`,
    components: { school, hello }
  })

  
  //创建vm
  new Vue({
    el: '#root',
    template: '<app></app>',
    //注册组件（局部）
    components: { app }
  })
</script>
```
![image-20221206190605104](https://october-x-image-host.oss-cn-hangzhou.aliyuncs.com/markdown-imgsimage-20221206190605104.png)
#### VueComponent
关于**VueComponent**

1.  **school**组件本质是一个名为`VueComponent `的构造函数，且不是程序员定义的，而是 `Vue.extend()`生成的
2. 我们只需要写**<school/>**或**<school></school>**，**Vue解析时会帮我们创建school组件的实例对象**，即Vue帮我们执行的`new VueComponent(options)`
3. 每次调用`Vue.extend`，返回的都是一个全新的`VueComponent`，即不同组件是不同的对象           
4. 关于**this** 指向
   - 组件配置中`data函数、`**methods**` 中的函数、 `**watch**` 中的函数、`**computed **`中的函数它们的this均是 `**VueComponent实例对象**`
   - b. `new Vue(options）`配置中: `data`函数、 `methods`中的函数、 `watch` 中的函数、`computed`中的函数它们的 **this**均是`Vue实例对象`
5. `VueComponent`的实例对象，以后简称`vc`(**组件实例对象**)`Vue的实例对象`，以后简称`vm`

**vm上的方法vc都有，vc可以说是微型的vm。二者不能划等号。二者可以理解为双胞胎，组件是可以复用的vue实例。vm可以通过el指定为哪个容器服务，vc不可以。**
![image-20221206190807515](https://october-x-image-host.oss-cn-hangzhou.aliyuncs.com/markdown-imgsimage-20221206190807515.png)
**源码分析：**
![image-20221206190825377](https://october-x-image-host.oss-cn-hangzhou.aliyuncs.com/markdown-imgsimage-20221206190825377.png)  对应上面的第三点
![image-20221206190836796](https://october-x-image-host.oss-cn-hangzhou.aliyuncs.com/markdown-imgsimage-20221206190836796.png)
对应上面第四点，和之前的vm长的非常像。vm里面有一个属性$children,里面存放的就是使用的components，组件里依然是可能存在$children
![image-20221206190849771](https://october-x-image-host.oss-cn-hangzhou.aliyuncs.com/markdown-imgsimage-20221206190849771.png)

```html
<title>VueComponent</title>
<script type="text/javascript" src="../js/vue.js"></script>

<div id="root">
  <school></school>
  <hello></hello>
</div>

<script type="text/javascript">
  Vue.config.productionTip = false

  // 定义school组件
  const school = Vue.extend({
    name: 'school',
    template: `
				<div>
					<h2>学校名称：{{name}}</h2>	
					<h2>学校地址：{{address}}</h2>	
					<button @click="showName">点我提示学校名</button>
  			</div>
			`,
    data() {return {name: '尚硅谷',address: '北京'}},
    methods: {showName() {console.log('showName', this)}},
  })

  const test = Vue.extend({
    template: `<span>atguigu</span>`
  })

  // 定义hello组件
  const hello = Vue.extend({
    template: `
				<div>
					<h2>{{msg}}</h2>
					<test></test>	
  			</div>
			`,
    data() {return {msg: '你好啊！'}},
    components: { test }
  })

  // console.log('@',school)
  // console.log('#',hello)

  // 创建vm
  
  const vm = new Vue({
    el: '#root',
    components: { school, hello }
  })
</script>
```
#### 一个重要的内置关系
![image-20221206190857947](https://october-x-image-host.oss-cn-hangzhou.aliyuncs.com/markdown-imgsimage-20221206190857947.png)

1. 一个重要的内置关系:`**VueComponent.prototype._proto__ === Vue.prototype**`
2. 为什么要有这个关系:让组件实例对象`**vc**`可以访问到`Vue原型`上的属性、方法

## 单文件组件

- School.vue
```html
<template>
    <div id='Demo'>
        <h2>学校名称：{{name}}</h2>
        <h2>学校地址：{{address}}</h2>
        <button @click="showName">点我提示学校名</button>
    </div>
</template>

<script>
    export default {
        name:'School',
        data() {
            return {
                name:'UESTC',
                address:'成都'
            }
        },
        methods: {
            showName(){
                alert(this.name)
            }
        },
    }
</script>

<style>
    #Demo{
        background: orange;
    }
</style>
```

- `Student.vue`
```html
<template>
    <div>
        <h2>学生姓名：{{name}}</h2>
        <h2>学生年龄：{{age}}</h2>
    </div>
</template>

<script>
    export default {
        name:'Student',
        data() {
            return {
                name:'cess',
                age:20
            }
        },
    }
</script>
```
App.vue
```html
<template>
    <div>
        <School></School>
        <Student></Student>
    </div>
</template>

<script>
    import School from './School.vue'
    import Student from './Student.vue'

    export default {
        name:'App',
        components:{
            School,
            Student
        }
    }
</script>
```
main.js
```html
import App from './App.vue'

new Vue({
    template:`<App></App>`,
    el:'#root',
    components:{App}
})
```
index.html
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>单文件组件练习</title>
</head>
<body>
    <div id="root"></div>
    <script src="../../js/vue.js"></script>
    <script src="./main.js"></script>
</body>
</html>
```

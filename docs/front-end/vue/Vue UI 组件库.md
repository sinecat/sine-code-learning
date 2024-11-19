## 常用UI组件库
### 移动端常用UI组件库

1. [Vant](https://vant-ui.github.io/vant/#/zh-CN)
2. [Cube UI](https://didi.github.io/cube-ui/#/zh-CN)
3. [Mint UI](http://mint-ui.github.io/#!/zh-cn)
4. [NutUI](https://nutui.jd.com/#/)
### PC端常用UI组件库

1. [Element UI](https://element.eleme.cn/#/zh-CN)
2. [IView UI](https://www.iviewui.com/)
## element-ui基本使用

1. 安装element-ui: `**npm i element-ui -s**`
2. src/main.js
```vue
import Vue from 'vue'
import App from './App.vue'
import ElementUI from 'element-ui';							// 引入ElementUI组件库
import 'element-ui/lib/theme-chalk/index.css';	// 引入ElementUI全部样式

Vue.config.productionTip = false

Vue.use(ElementUI)	// 使用ElementUI

new Vue({
    el:"#app",
    render: h => h(App),
})
```

3. src/ App . vue
```vue
<template>
	<div>
		<br>
		<el-row>
			<el-button icon="el-icon-search" circle></el-button>
			<el-button type="primary" icon="el-icon-edit" circle></el-button>
			<el-button type="success" icon="el-icon-check" circle></el-button>
			<el-button type="info" icon="el-icon-message" circle></el-button>
			<el-button type="warning" icon="el-icon-star-off" circle></el-button>
			<el-button type="danger" icon="el-icon-delete" circle></el-button>
		</el-row>
	</div>
</template>

<script>
	export default {
		name:'App',
	}
</script>
```
![image-20221206191853164](https://october-x-image-host.oss-cn-hangzhou.aliyuncs.com/markdown-imgsimage-20221206191853164.png)
## element-ui按需引入

1. 安装babel-plugin-component `**npm i babel-plugin-component -D**`
2. 修改`babel-config-js`
```vue
module.exports = {
  presets: [
    '@vue/cli-plugin-babel/preset',
    ["@babel/preset-env", { "modules": false }]
  ],
  plugins: [
    [
      "component",
      {        
        "libraryName": "element-ui",
        "styleLibraryName": "theme-chalk"
      }
    ]
  ]
}
```

3. `src/ main.js :`
```vue
import Vue from 'vue'
import App from './App.vue'
import { Button,Row } from 'element-ui'	// 按需引入

Vue.config.productionTip = false

Vue.component(Button.name, Button);
Vue.component(Row.name, Row);
/* 或写为
 * Vue.use(Button)
 * Vue.use(Row)
 */

new Vue({
    el:"#app",
    render: h => h(App),
})
```

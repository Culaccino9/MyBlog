---
title: VUE搭建环境和结构认识
date: 2021-06-24 22:48:36
tags: Vue
categories: Vue
---

# Vue

vue的安装    yarn global add @vue/cli

或 

npm i @vue/cli -g

查询vue -V

检测vue-cli是否成功：vue	--version

### vue环境搭建

1.根目录下载   vue create my-vue(my-vue是创建的文件夹名字)

2.打开服务器   yarn serve

一些知识点:

​		a.IDE中文件夹名字为全大写，意思是《工作区》

​		b.VUE项目下public存在唯一一张单页面应用。SPA，由一张页面组成，用JS写DOM，换DOM

​			和a标签跳转不同在于，a标签为http跳转，会刷新网页



### Vue-cli





### MVVM设计模式

三个部分组成：

M：Model					数据模型（保存数据，处理数据业务逻辑）

V：View						视图（展示数据，与用户交互）

VM：View	Mode		数据模型和视图f的桥梁

MVVM设计模式最大特点是支持数据的双向传递

M-VM-V || V-VM-M

Vue是支持

```
//这里就是View
<div id="app">
	{{name}}
</div>

<script>
//这里就是MVVM中的View Model
	let vue =new Vue({
		le:"#app",
//这里就是Model
		data:{
			name:"123"
		}
	})
```



### VUE调测工具TOOLS

https://www.chromefor.com/vue-js-devtools_v5-3-0/



### vue结构认识

1.nodeModules        依赖目录

2.public                 	全局静态目录。

3.scr						   项目开发主目录

4.gitignore				git忽略文件配置

5.babel.config.js      配置VUE项目内的编译语言

6.package.json		 依赖配置文件

7.yarn.lock				yarn安装所有依赖地址，作用：锁版本，如果一直安装失败，删除他和nodeModules

8.vue.config.js		  vue脚手架配置工具（目前需要手动创建

##### src目录下文件认识：

​			a.assets					静态文件（js.css.img.font放置

​			b.components		VUE组件，小组件，公共组件

​			c.api						  接口===>注意规范命名，相同

​			手动创建类:

​			d.utils						工具类

​			e.router					路由

​			f.store						Vuex相关文件。数据仓库

​			g.plugins					插件

### VUE引入组件方法

1.引入组件

​		import	组件名(自定义)	from	'@/地址'          地址中@代表src目录

2.注册组件

​		components:{  使用名(自定义)  :  组件名  (对应上面自定义的)}

```script
<script>
import FirstCmp from '@/components/FirstCmp.vue'		//引入组件地址
  export default {
    components:{
      FirstCmp,
    }
  }
</script>
```

3.使用组件

​		和html标签使用一样

### mustach

1.主要用于表达渲染**表达式**				语法{{表达式}}

​		表达式：有返回值的计算过程

​						a.三目运算

​						b.函数

​						c.数学表达式

知识点：**template中只能有一个根目录**，负责搭建页面，可以渲染data中的数据。能使用的都**只能是export defaule中**定义的数据。

2.export default {   }中不能写函数，函数写在其外部，与其同级。

​	想要**调用函数**用methods：{  }

```
<template>
    <div>
    {{dealNum(num1,num2)}}				//函数
    <hr>
    {{num1>num2?"num1比num2大":"num1比num2小"}}			//三目运算
    </div>
</template>

<script>
function dealNum(a, b) {			//函数
  return a - b;
}

export default {
  data() {
    return {
      num1: 10,
      num2: 20
    };
  },

  methods: {				//函数导出
    dealNum
  }
};
</script>
```



### script里的内容

##### export  default	组件实例

​	为组件实例，里面写data数据，在整个对象中都可以用this来访问data中的数据

##### methods	组件方法定义

​	在实例内，methods属性中定义组件的全部方法



##### Vue-cli修改webpack配置

vue.config.js文件，创建在全局下。

Vue-cli对webpack进行了封装，如果提供的不能满足需求，则通过**configureWebpack**中编写原生的配置

```
cosnt path = require(id:'path');
const webpack = require(id:'webpack')	//配置原生的webpack需要导入

module.exports={
	configurWebpack:{
		可以在这里面编写原生的webpack配置，如：
		plugins:[
			new webpack.BannerPlugin({
				banner:'版权信息'
			})
		]
	}

}
```




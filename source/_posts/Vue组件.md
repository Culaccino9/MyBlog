---
title: Vue组件
date: 2021-06-24 22:53:19
tags: Vue
categories: Vue
---

# 1.组件



## 1.1.组件视图模板

视图模板主要指的是`<template></template>`



## 1.2.动态组件

标签：<component	v-bind:is="组件名">

```
<component v-bind:is="组件名"> </component>
如：
<component v-bind:is="变量名"></component>		这里变量设置一个点击事件让他更改组件名来达到更换组件的实现
```

动态组件可以可以保存上一次的记录。使用：

keep-alive包裹动态组件

```
<keep-alive>
	<component v-bind:is="变量名"></component>	
</keep-alive>
```



## 1.3.给组件添加动画

单个组件transition	多个组件transition-group

过渡默认是同时进行的，设置过度模式。



## 1.4.组件自身逻辑

组件自身逻辑主要指的是`<script></script>`,而一个组件逻辑内部常用的一些设定都有：

- data——定义组件自身内部数据

  - 必须，data必须是函数
  - 必须，data必须返回一个对象，对象内就是定义的组件自身的数据
- props——定义组件的属性

- methods——定义组件自身的方法
- watch——监听组件内部数据的变化
- computed——计算属性，根据当前数据衍生出新的数据
- 生命周期钩子——在每一个生命周期阶段，自动执行
- filter——过滤器（新版本已经废除）
- components——注册子组件



## 1.5.props详解：

作用和插槽类似，组件设置一个自带的属性，父盒子可以使用自身的变量数据替代掉组件原有的属性的值。

基本语法：

```
这个组件叫：PropCmpt.vue
// 属性名：属性类型
props:{
    num:Number,
    words:String
}
```

使用属性：

然后引用这个组件(只能传字符串) 		<组件名（属性="属性值"）>	如下：

```
<PropCmpt num="123">
```

如果是变量	则使用 v-bind绑定	如下：

```
<PropCmpt :words=“str”>
或者<PropCmpt	v-bind:words="str"

data(){
	return{
		str:"123",
	}
}
```

完整声明：

```
// 完整声明+自定义规则
props:{
    num:{
        type:Number, // 类型
        default: 10  //默认值
        required:true,			//表示必填，不填就要报错，如果不设置默认值
    }
}

// 引用类型声明和基本类型有区别
props:{
    obj:{
        type:Object,
        default:()=>({name:'张三',age:30})
    },
    arr:{
        type:Array,
        default:()=>([1,2,3,4,5])
    }
}

//数组常用书写方式
props:['num','arr','obj']
```



## 1.6.组件的通信特征

通信类型有三种：

### 1.6.1父传子：

​	就是使用props，在子设置props，父给数据

```
//子
<template>
    <div>
        {{words}}			
    </div>
</template>

<script>
export default {
  props: {
    words: {
      type: String,
      default: ""
    }
  }
};
</script>

//父
<template>
    <div>
        <Child :words="fatherWords"/>	//使用子级组件。并传参
    </div>
</template>

<script>
import Child from "./Child";		//引用子级组件
export default {
  data() {
    return {
      fatherWords: "孩子，我是爹啊"	//父级数据
    };
  },
  components: {
    Child
  }
};
</script>
```



### 1.6.2.子传父	$emit

利用事件的监听和触发。（父传子是触发机制）

$emit(“自定义数据”，传递的数据)

​	a.子级设置一个DOM自带的事件来触发一个函数句柄，这个函数句柄里使用$emit来绑定父级事件和传递子集数据

```
<button @click="doPress">按下按钮，老爹渲染</button>

  data() {
    return {
      toFather: "爹啊，我是儿"
    };
  },

  methods: {
    doPress() {
      //   $emit用于触发事件
      //   $emit还可以传递参数
      this.$emit("childEvent", this.toFather);
    }
  }
```

​	b.父级使用子集模块，在其上，设置一个自定义事件，触发一个函数（这个函数就可以得到子集数据了）。

```
<Child @childEvent="childDeal"/> 这是儿子组件传递来的数据：{{fatherWords}} data() {    return {      fatherWords: ""	//让一个变量来接收儿子的信息    };  }, methods: {    childDeal(data) {      console.log(data);      this.fatherWords = data;		//让儿子的信息渲染到自己的变量上    },  },
```



### 1.6.3.兄弟传兄弟	$emit     $on（“事件”，回调函数）

兄弟指的是，父级引用了两个组件，这两个组件就是兄弟关系

![image-20210516212500083](C:\Users\大魔头的computer\AppData\Roaming\Typora\typora-user-images\image-20210516212500083.png)

使用$on和$emit

​	a.创建一个Bus文件夹，里面创建bus.js文件，引用且暴露实例化VUE作为桥梁。

```
import Vue from 'vue'export default new Vue()
```

​	b.1.在要传递信息的兄弟级，引入Bus

```
import Bus from "@/Bus/Bus";
```

​	b.2.且使用$emit添加触发机制

```
<button @click="doPress">按下按钮，老爹渲染</button>  methods: {    doPress() {      Bus.$emit("callBrother", "兄弟啊，想你啦~~~");    }  }
```

​	c.在兄弟级引入Bus。兄弟级渲染后阶段，使用$on监听触发事件（让他发生

```
  这是兄弟传给我的值：{{brotherWords}}      data() {    return {      brotherWords: ""    };  },    mounted() {    Bus.$on("callBrother", data => {      this.brotherWords = data;    });  }
```

### 1.6.4.子父通信ref

$refs是一个集合，

​	a.ref标记，这样就标记到了这个标签

```
<子组件  ref="自定义名字（一般与标签名相同"><Son ref="son">
```

​	b.那么$refs这个集合就包含了你标记的组件，就可以对应的调用方法

```
this.$refs.ref中自定义的名字.你要获取的数据this.$refs.son.fn()
```



## 1.7插槽

插槽就是组件里面挖个坑，设置一个槽位。在一个vue使用组件的时候，<被使用的组件>填上插槽内容</被使用的组件>

分为三种：匿名插槽、具名插槽、作用域插槽（重难点）

1.slot	匿名插槽(单个插槽使用)

​	a.直接使用，在组件里面设置一个插槽标签<slot></slot>

```j
<div>	<slot></slot></div>
```

​	b.使用组件的时候用双标签包裹里面填入插槽内容

```
<组件名>我是填入的插槽内容</组件名>
```

2.具名插槽，就是带有名字的插槽（多个使用）

​	a.组件设置插槽的时候带个名字

```
<slot name="one"></slot><slot name="two"></slot>
```

​	b.对应插槽的名字使用

```
<组件名><div slot="one">这是插槽1</div><div slot="two">这是插槽2</div></组件名>
```

现在上面已经被淘汰，用v-slot属性来做

```
<tamplate	v-slot:插槽名>	添加的内容</tampalte><slot name="插槽名"></slot>
```



3.作用域插槽	v-slot可以简写为#

​	a.组件插槽v-bind绑定数据传参    （暴露）

直接传递数据需要类型是数字类型，要想传递字符串则要用data里变量保存

```j
<slot :stuName="name" :age="age"></slot>data(){	return{	name:"张三"，	age:18,	}}
```

​	b.在使用组件的地方用template装载他（template只是一个装载容器不是标签	（以前是用这个接收slot-scope

```
<组件名>	<template v-slot="定义一个变量名来装载传递的参数">		{{定义的变量名（渲染他}}	</template></组件名>例：<组件名>	<template v-slot="student">		{{student}}	</template></组件名>
```



![image-20210520101500791](C:\Users\大魔头的computer\AppData\Roaming\Typora\typora-user-images\image-20210520101500791.png)



## 1.8组件的使用

1.局部使用：

​	直接暴露，使用

2.全局使用：

​	a.给子组件设置一个name:

```
<script>	export  default{		name:'Loading'	}</script>
```

​	b.在main.js中暴露他

```
import Loading from '路径'Vue.component(Loading.name,Loading)		//Loading.name拿到名称  Loading注册他使用这个标签
```

3.Vue.use注册

​	a.把组件封装成插件

​	scr下plugin文件夹创建文件夹，里面放一个.vue插件文件，再放一个.js文件

![image-20210520132218187](C:\Users\大魔头的computer\AppData\Roaming\Typora\typora-user-images\image-20210520132218187.png)

​	b..vue文件中写组件

​	c.js文件中

```
//暴露他
import Vue from 'vue'
import  Loading  from  '组件路径'

//配置
export  default{
	install:function(){
		Vue.component(Loading.name,Loading)
	}
}
```

​	d.在main.js文件中导入上面配置的JS文件

```
import Loading from '/JS文件地址'

Vue.use(Loading)
```



## 1.9动态组件 & 异步组件

重新创建动态组件的行为通常是非常有用的，但是在这个案例中，我们更希望那些标签的组件实例能够被在它们第一次被创建的时候缓存下来。为了解决这个问题，我们可以用一个 `<keep-alive>` 元素将其动态组件包裹起来。

```
<!-- 失活的组件将会被缓存！-->
<keep-alive>
  <component v-bind:is="currentTabComponent"></component>
</keep-alive>
```

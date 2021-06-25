---
title: Vue指令
date: 2021-06-24 22:52:02
tags: Vue
categories: Vue
---

# 1.VUE指令

## 1.1.内容系	v-text	v-html

（接收变量、字符串等

1.v-text 		不能转译标签

2.v-html		能转译标签

```
        <!-- v-text：在dom中渲染文本内容(强制文本化) -->
        <p v-text="str"></p>
        <!-- v-html：在dom中渲染富文本 -->
        <p v-html="tit"></p>
```



##  1.2.显隐系	v-show	v-if

（接收boolean

1.v-show		通过控制CSS的display属性来达到显示隐藏效果

2.v-if				通过渲染DOM，false会删掉DOM节点

知识点：<u>**频率高推荐v-show 		频率低且安全性高的用v-if**</u>

```
        <button v-on:click="show=!show">切换显示</button>
        <p v-show="show">这个是v-show控制的</p>
        <p v-if="show">这个是v-if控制的</p>
        script中定义show：true
```



## 1.3.分支系	v-if	v-else-if	v-else

（接收boolean	

1.v-if	v-else-if	v-else

注意：<u>**注意三个条件的顺序**</u>

```
        <p v-if="date === 1">今天是周一</p>
        <p v-else-if="date === 2">今天是周二</p>
        <p v-else-if="date === 3">今天是周三</p>
        <p v-else>我也不知道今天周几</p>
        script中定义date//得到的效果为，1显示周一标签，2显示周二标签
```



## 1.4.列表系	v-for

（重点！接收Array	Object

语法一：v-for="(item,index)	in	Array"

语法二：v-for="(value,key)	in	Objeck"

语法三：v-for="item in 5" 	:key="item"	为循环5次

注意：<u>**在为每个DOM遍历时，要给每个DOM添加一个key属性（和v-bind配合使用）**</u>

v-bind:key可以简写为:key



***使用map实现映射关系***

   */** 

​    *map是一种ES6提供的新数据结构(map的key可以是任何类型)*

​    *语法：*

​      *const map = new Map([*

​        *[key,value],*

​        *[key,value],*

​        *....*

​      *])*

​    

​    *设置map成员：*

​      *map.set(key,value)*

​    *获取map成员：*

​      *map.get(key)*





<u>**添加一维数组**</u>

```
        <ul>
            <li v-for="(item,index) in stus" :key="index" v-text="item"></li>
        </ul>
		      stus: ["颜嘉豪", "高进", "杨茂", "秦富城", "陶前雄"],
```

<u>**添加嵌套循环，将键名添加进li标签**</u>

```
        <ul>
            <li v-for="(item,index) in stusInfo" :key="index">
                <ul>
                    <li v-for="(value,key) in item" :key="key">{{words[key]}}：{{value}}</li>
                </ul>
            </li>
        </ul>
        
        stusInfo: [
        {
          name: "颜嘉豪",
          area: "IT互联网",
          salary: 9,
          major: "web前端开发"
        },
        {
          name: "高进",
          area: "IT互联网",
          salary: 19,
          major: "web前端开发"
        }],
        words: {
        name: "姓名",
        area: "从事领域",
        salary: "薪资水平",
        major: "专业"
      	},
```

<u>**map添加**</u>

```
        <ul>
            <li v-for="(item,index) in stusInfo" :key="index">
                <ul>
                    <li v-for="(value,key) in item" :key="key">{{wordsMap.get(key)}}：{{value}}</li>
                </ul>
            </li>
        </ul>
        
        wordsMap: new Map([
        ["name", "姓名"],
        ["area", "从事领域"],
        ["salary", "薪资水平"],
        ["major", "专业"]
	    	]),
```



## 1.5.绑定系	v-bind

--为DOM绑定一个属性

1.v-bind：属性属性名=“属性值”

​		如果属性值为字符串。则要加‘’。例如=======>v-bind：href=“ 'http://www.baidu.com' ”

2.可以简写	省掉v-bind。例如=========>    ：str=" '这是自建的str属性值是这个' "

```
        <a v-bind:href="'http://www.baidu.com'">百度一下，你就知道</a>
        <p v-bind:test="'随便什么属性'">这是一个p标签</p>
        <p :str="date">这是另一个p标签</p>
```

3.v-bind绑定class属性

​		语法：<u>**v-bind:class="{类名1:布尔值，类名2:布尔值}"**</u>

```
         <p v-bind:class="{red:isRed,blue:isBlue}">这是一个段落</p>
         <button @click="changeBackGround">点击按钮，切换背景</button>
         
           data() {
   			 return {
     		 isRed: true,
     		 isBlue: true
   					 };
 				 },

  		  methods: {
    		changeBackGround() {
     	    this.isBlue = !this.isBlue;
    							}
 				 	}
```

4.v-bind绑定style属性

​		语法：v-bidn：style=“{color:'red',marginLeft:'30px',width:'100px' }”

```
<p v-bind:style="{width:'300px',color:'blue',marginLeft:'200px',backgroundColor:'red'}">这是行内样式</p>
```



## 1.6.提升用户体验系列--   v-cloak

v-cloak，Vue是后渲染，所以网速比较慢，用户看到的是{{name}}这样的数据，那么我们可以在标签中加入v-cloak指令来提升用户的体验。

语法：

```
scc中写：[v-cloak]{	display:none;}标签中写：<div v-cloak>	{{name}}</div>
```



## 1.7.绑定系--单次绑定	v-once

作用：v-once上的数据保存原始数据，只让数据渲染一次，其他的都更改

v-once

语法：

```
<p v-once>原始数据{{name}}</p>	//这个还是name的原始数据<p>当前数据{{name}}</p>		//这个是name更改一次的数据
```



## 1.8.绑定系--双向绑定	v-model

<u>**作用：可以让我们非常方便的操作表单**</u>

1.语法v-model=“  export default 中创建的空对象名.要给这个对象创建的键名  ”

​	那么我们就可以在创建的空对象下得到相应的键名：键值（input输入的值）

2.如果v-model绑定的是一个复选框。则有以下两种情况：

​	a.如果绑定的是一个基本数据类型，那么勾选以后得到的是一个布尔值。

​	b.如果是一个数组的话，那么勾选的复选框会将复选框的vaule添加至数组中。

​	<u>**所以复选框添加要先在建的空对象中添加一个空数组。**</u>

3.如果想要更改默认值，可以在空数组中添加相对应的键名和键值。   

```
        <div style="border:1px solid #000;width:700px">            <div>                用户名：<input type="text"  v-model="formData.username">            </div>            <div>                性别：                <input type="radio" name="sex" v-model="formData.sex" value="男">男                <input type="radio" name="sex" v-model="formData.sex" value="女">女            </div>            <div>                爱好：                <input type="checkbox" name="favorate" v-model="formData.favorate" value="篮球">篮球                <input type="checkbox" name="favorate" v-model="formData.favorate" value="台球">台球                <input type="checkbox" name="favorate" v-model="formData.favorate" value="足球">足球            </div>            <div>                简介：                <textarea name="" id="" cols="30" rows="10" v-model="formData.intro"></textarea>            </div>        </div>        <script>export default {  data() {    return {      formData: {        favorate: []      }    };  }};</script>
```

### 1.8.1.v-model实现语法糖

自定义v-model必须在组件内定义model属性

一进一出：

- 一进
  - 将model中指定一个属性名来接收外界传递的数据（用于内部渲染
  - 指定prop
  - 定义props
  - 监听props属性，定义data属性接收外界值
  - 使用data属性参与内部计算
- 一出
  - model中指定一个事件名来将内部希望发送到外界数据通过$emit触发并传参
  - 在数据改变的狗子函数中，触发事件发送到外部。

```js
model:{	//和data同级    prop:"自定义名，这里定义name",	//这货接收外界的数据    event:"自定义事件名，这里定义changeName"}props:{    name:String},methods:{	触发的事件(res){        this.myname=res.name;        this.$emit("changeName",res.name)    }}data(){    return{        myname:""		//这个接收外界传入的参数，渲染自己    }}watch:{    name(newVal){        this.myname=newVal   //斩断和外界的关系，否则引用类型会影响外部    }}
```

### 1.8.2[修饰符](https://cn.vuejs.org/v2/guide/forms.html#修饰符)

.lazy

在默认情况下，`v-model` 在每次 `input` 事件触发后将输入框的值与数据进行同步 (除了[上述](https://cn.vuejs.org/v2/guide/forms.html#vmodel-ime-tip)输入法组合文字时)。你可以添加 `lazy` 修饰符，从而转为在 `change` 事件_之后_进行同步：

```
<!-- 在“change”时而非“input”时更新 --><input v-model.lazy="msg">
```

.number

如果想自动将用户的输入值转为数值类型，可以给 `v-model` 添加 `number` 修饰符：

```
<input v-model.number="age" type="number">
```

这通常很有用，因为即使在 `type="number"` 时，HTML 输入元素的值也总会返回字符串。如果这个值无法被 `parseFloat()` 解析，则会返回原始的值。

.trim

如果要自动过滤用户输入的首尾空白字符，可以给 `v-model` 添加 `trim` 修饰符：

```
<input v-model.trim="msg">
```



## 1.9.自定义属性	directive	

### 1.9.1全局指令directive

directive方法接收两个参数	directive("自定义指令"，{ } )

​	a.第一个参数：指令的名称

​	b.第二个参数：对象

bind：指令被绑定到元素上的时候执行

inserted：绑定执行的元素被添加到父元素上的时候调用

例：

```
<input v-focus></input>		//v-color就是自定义属性<script>	Vue.direvtive("focus,{	inserted:function(el){		//被绑顶到元素的时候执行			el.focus()		}	}")			//效果：页面刷新，inpu框激活
```

### 1.9.2局部指令directives

将direvtive写在实例化对象中，和data同级，

```
directives：{	自定义属性名：{		inserted:function(el){		//被绑顶到元素的时候执行			el.focus()		}	}}
```



# 2.计算属性	computed

**计算属性和函数的区别：**

函数：每次调用都会执行

计算属性：只要返回的结果没有发生变化，那么计算属性只会被执行一次

用处：提升效率。经常会发生变化的数据，推荐用函数。相反的，推荐使用计算属性



虽然是通过一个函数来返回数据，但是他不是一个函数或者方法。所以不能带()

知识点：计算属性基于data内的属性衍生的新数据，有以下特点

​					a.源数据改变后，计算数据就会发生改变

​					b.计算属性有缓存机制，使用多次只会做一次计算，除非源数据发生改变

<u>**data定义之后，在整个对象中都可以使用this来访问data中的数据**</u>

1.computed:{			}  用于设置计算属性

2.里面数据为函数形式     例如：fn(){			}

```
<p>{{deliveryInfo}}</p><script>export default {  // data定义之后，在整个对象中都可以使用this来访问data的数据  data() {    return {      fullName: "秦富城",      address: "重庆互联网学院"    };  },  //   computed用于设置计算属性  computed: {    deliveryInfo() {      return `${this.fullName}你的快递在${this.address}`;    }  }};</script>
```



# 3.方法

方法都写在methos：{	}内，methods和data（）{	}return同级

1.methods内的方法都可以用this互相调用

写在script中的方法可以直接在methods内调用

2.template中直接用mustach方法

```
<template>    <div>        <p>{{str}}</p>        <p>{{doSth()}}</p>        <p>{{otherFn()}}</p>    </div></template><script>function consoleSth() {  console.log("这是外面定义的方法");}export default {  data() {    return {      str: "hello world"    };  },  methods: {    doSth() {      consoleSth();      console.log(this.str); // methods的方法想要访问data内的数据，可以使用this.xxx的方式来访问      return "我定义了一个方法";    },    // methods内的方法，想要互相调用，可以使用：this.xxx的语法方式    otherFn() {      return `这是otherFn方法，我使用了doSth方法并得到结果：${this.doSth()}`;    }  }};</script>
```



## 3.1.事件	v-on

1.监听语法有两种：可以写（）也可以不写，写（）可以传参数

A.	v-on：cilick=“事件句柄”========>v-on:事件名=“事件句柄”

​			a.	v-on:绑定事件的指令

​			b.	click:事件名称

​			c.	事件句柄（事件执行方法）

B.	@事件名="事件句柄"

2.事件方法也可以传参

### 3.1.1.v-on 一般修饰符

once			 -只触发一次回调函数。（只触发一次函数）

prevent		-调用event.preventDefault()。阻止默认行为。

self				-冒泡事件中，点击其他，冒泡正常但是不执行他，他的冒泡也正常执行。只是当事件是从监听器绑定的元素本身触发时才触发。如果想让回调函数只有当前函数触发才执行

stop			  -调用event.stopPropagation()。阻止事件冒泡，在子代中选。

capture		-添加事件侦听器时使用capture模式。事件冒泡变成事件捕获

语法：

v-on:事件名.修饰符="回调函数"

```
v-on：click.once=“fn”
```

### 3.1.2.按键修饰符

在监听键盘事件时，我们经常需要检查详细的按键。Vue 允许为 `v-on` 在监听键盘事件时添加按键修饰符：

```
<!-- 只有在 `key` 是 `Enter` 时调用 `vm.submit()` --><input v-on:keyup.enter="submit">
```

你可以直接将 [`KeyboardEvent.key`](https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent/key/Key_Values) 暴露的任意有效按键名转换为 kebab-case 来作为修饰符。

```
<input v-on:keyup.page-down="onPageDown">
```

在上述示例中，处理函数只会在 `$event.key` 等于 `PageDown` 时被调用。

更多修饰符查看：https://cn.vuejs.org/v2/guide/events.html



### 3.1.3.事件对象

event			事件对象和形参可以一起传。

用($event)

```
<button @click="showEvent(10,$event)">点击我查看事件对象</button>  methods: {    showEvent(arg, event) {      console.log(arg, event);    }  }};
```



# 4.过滤器filter（新版本已经移除）

1.没有缓存机制，过滤几次调用几次

2.源数据生成新数据

3.源数据发生改变新数据也改变

```
{{arr | dealArr}}  filters: {    dealArr(arr) {      return arr.map(item => {        return item * 10;      });    }  }
```



# 5.侦听器watch

数据发生改变后立即执行某些操作，用<u>**watch**</u>

源数据发生改变后得到一个新数据，用计算属性 <u>**computed**</u>

可以用watch监听路由变化。"$router.path":funcion(newVal,oldVal){

}

1.基本语法：

​		基本语法1----方法

​			属性名（是data中定义的属性）（newVal,oldVal）{

​																							..............

​																								}

```
        <p>{{num}}</p>        <button @click="changeNum">改变数据</button>                export default {  			data() {    			return {     			 num: 10,    					}; 					 },  			methods: {   				 changeNum() {      				this.num = 20;        						}, 					 },  			watch: {    			num(newVal, oldVal) {      			console.log(newVal, oldVal);    								},  					}						};
```

​		基本语法2----对象

​			watch：{

​							属性名：{

​										handler（newVal，oldVal）{

​																			............

​																							}

​										deep：boolean      =====>设置对象类型深层次监听

​										immediate：boolean   ======>设置当前watch在声明之后立即执行一次（解决异步

​											}

​							}

```
<p>{{num2}}</p><button @click="changeNum2">改变num2数据</button>                export default {  data() {    return {      num2: 20    };  },  methods: {    changeNum2() {      this.num2 = 50;    }  },  watch: {    num2: {      handler(newVal, oldVal) {        console.log(newVal, oldVal);      }，      deep:boolean,      immediate：boolean    }  }};
```



# 6.生命周期

**钩子方法：立即调用**（回调函数

vue有四大生命阶段，8大生命周期：

- 创建——将data数据初始化，绑定到组件身上          组件实例刚刚初始化
  - beforeCreate
    - 创建前
  - created
    - 创建后
- 渲染——将template模板加载到dom视图中
  - beforeMount
    - 渲染前
  - mounted（操作dom的话，从这个生命周期后，就可以了）
    - 渲染后
- 更新——拿到新数据，更新dom视图
  - beforeUpdate
    - 更新前
  - updated
    - 更新后
- 销毁——将当前组件从视图上移除
  - beforeDestroy
    - 销毁前
  - destroyed
    - 销毁后

**作用：**

用于监测组件在某个阶段的状态，并及时对当前状态做出反应

留意三个声明周期：

​		1.created(){	}一般ajax请求在这面发送

​		2.mounted(){	}获取DOM节点在这里进行

​		3.beforDestroy(){	}用于取消当前组件所有的事件，取消ajax请求用（abort方法）

```
export default {  data() {    return {    };  },  methods: {  },  beforeCreate() {    console.log("组件准备创建");  },  created() {    //   一般ajax请求都在created中发送    console.log("组件创建完成了");  },  beforeMount() {    console.log("组件准备渲染了");  },  mounted() {    //   一般情况下，获取dom节点，在mounted生命周期内    console.log(document.querySelector(".text"));    console.log("组件渲染完成了");  },  beforeUpdate() {    console.log("组件准备更新了");  },  updated() {    console.log("组件更新完成了");  },  beforeDestroy() {    //   一般情况下，用于取消当前组件所有事件，取消当前ajax（abort方法）    console.log("组件准备销毁了");  },  destroyed() {    console.log("组件已经被销毁了");  }};
```



# 7.过渡动画

注意： A.一个transition中只能放一个元素。多个要用多个transition包裹。

​			B.默认属性如果是true，那么是不会有出现的动画的，如果需要true也出现动画，那么需要在**transtition**中添加一个appear属性

**<transition appear>**

​			C.如果要多个过度动画不同的效果，那么给他添加一个name，在scc中也对应修改。

```
<transition name="one"><transition name="two">css:.one-enter{}.two-enter{}
```

。

## 7.1.transition	类名操作

默认类名V-XXX指定过度动画，自定义类名前缀来指定过渡动画，JS钩子函数来实现动画，自定义类名方式指定过度动画

​	a.使用transition标签包裹他   <transstion>要实现效果的组件</transition>

​	b.设置相对应的类名。分为四个过程

![image-20210519223831113](C:\Users\大魔头的computer\AppData\Roaming\Typora\typora-user-images\image-20210519223831113.png)

​		所以设置类名

```
css:	.v-enter{		opacity:0;	}	.v-enter-to{		opactiy:1;	}	.v-enter-active{		transition:all 3s	}	.v-leave{		opacity:0;	}	.v-leave-to{		opactiy:1;	}	.v-leave-active{		transition:all 3s	}	html:	<transition>		<div v-show="isShow"></div>	</transition>
```

## 7.2.transition钩子函数、

注意：

A.默认他还是会找类名，不想的话，在transition上添加 v-bind:css="false"属性	<transition :css="false">

B.enter有一个回调函数done，要执行他才能进入下一个阶段

![image-20210519225157037](C:\Users\大魔头的computer\AppData\Roaming\Typora\typora-user-images\image-20210519225157037.png)

```
//html中 transition设置如上methods：{	beforeEnter(el){		//动画开始之前	},	enter(el,done){		done()		//如果一开始就要动画，那么给他一个setIntervle(function(){done()},0)	},	afterEnter(el){		//进入动画执行完毕之后	}}
```

## 7.3.自定义类名

![image-20210519230739864](C:\Users\大魔头的computer\AppData\Roaming\Typora\typora-user-images\image-20210519230739864.png)

以上写在transition标签上，enter-class="自定义类名"。

## 7.4.Animate库实现动画

​	a.下载animate

​	b.自定义类名上写animate的类名

## 7.5.给多个元素绑定动画

​	transition-group

transition标签变成transition-group	其他一样

用transition-group会将所有执行动画的元素都放入span中，如果要更改，transition标签上添加一个tag属性。

```
<transition-group tag=“ul”>	</transition-group>
```

## 7.6.过渡模式

mode=“out-in”	mode=“in-out”

加在transition标签上



# 8.$set()

添加并监听新的属性，用于template v-slot="{row}"等场景中

语法：this.$set(向哪添加,"添加什么属性",属性值)

如：

```vue
this.$set(row,"flag",true)
```





# 9.Vue特殊特性

## 9.1.ref

获取DOM $refs

this.$refs是一个空对象，里面装在标签中有ref="自定义名"的对象。调用他this.$refs.自定义名

ref添加给原生拿到就是原生的元素。如果添加给组件，拿到的就是自定义的组件

```
<p ref="getme">123</p>this.$refs.getme就拿到他了
```



## 9.2.key

绑定特殊ID，头从新增元素下面元素状态不乱

## 9.3.is

绑定动态组件

## 9.4.slot

插槽

## 9.5.slot-cope

以前的作用域插槽接收数据属性


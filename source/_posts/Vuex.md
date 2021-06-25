---
title: Vuex
date: 2021-06-24 22:50:07
tags: Vue
categories: Vue
---

# VueX

VueX就是专门为Vue.js应用程序开发的**状态管理模式**。

在这些框架里，状态指的是**数据状态**

它采用集中式储存管理应用的所有组件的状态。 

理解：

- 是vue里的数据管理仓库储存中心，用于分发数据到具体组件
- 当复杂数据被两个以上组件使用时，考虑用xuex来协助



# 下载

```
yarn add vuex --save
npm install vuex --save
```



# 配置

在src文件下创建一个store文件夹，下面创建一个index.js配置文件

### 配置index.js文件

- 引入

  - ```
    import Vue from 'vue'
    import Vuex from 'vuex'
    ```

- 注册

  - ```
    Vue.use(Vuex)   =====这样整个全局就可以使用this.$xxx来访问它
    ```

### state   状态（初始数据

- ```
  options:{
  	state:{
  		全局的数据
  	},
  }
  ```

- 使用方法1

  ```
    this.$store.state.xxx
  ```

- 使用方法2：推荐

  ```
    在使用的组件里引入 mapState方法
    import {mapState} from "vuex"
    
    使用computed计算方法中引入它
    computed:{
    	...mapState(["数据名1","数据名2","数据名3","数据名4"])
    }
    然后直接就可以通过数据名使用数据了
  ```

  

### getters   （计算属性

衍生状态（衍生数据

  - 参数为state

    ```
    getters:{
    	xxxx(state){
    		retrun state.xxx操作它
    	}
    }
    ```

- 使用

  ```
  {{xxx}}
  
  computed:{
  	xxx(){
  		return this.$store.getters.xxxx
  	}
  }
  ```

- 使用2

  ```
  {{xxx}}import {mapGetters} from 'vuex'computed:{	...mapGetters(["xxx"])}mapGetters引用的就可以直接通过[中的名字]使用了
  ```

  ![image-20210603155702325](C:\Users\大魔头的computer\AppData\Roaming\Typora\typora-user-images\image-20210603155702325.png)





### mutations （方法

同步修改state中的数据

  - 默认方法名全大写（规范

  - 有两个参数：state，playload

  - state就是仓库的state

  - padyload就是传递来的参数

  - ```
    mutations:{	UPDATE_NAME(state,payload){		state.name=payload	}}
    ```

  - 使用

  - 方法1

    ```
      this.$store.commit("告知提交的对应是哪个方法",这个方法的第二个参数)  this.$store.commit("UPDATE_NAME","这是修改的数据")
    ```

  - 方法2：推荐

    ```
      组件中引入它  import {mapState, mapMutations} from 'vuex'  methods里使用它  methods{  	...mapMutations(["方法名1","方法名2","方法名3","方法名4"])  	  	xxxx这个组件的方法(){  		this.方法名1("要传的参数")  	}  }  
    ```

    

### actions(异步方法)

参数：接收一个与store实例具有相同方法的属性context，这个属性是：

```js
 context:{		state,   等同于store.$state，若在模块中则为局部状态		rootState,   等同于store.$state,只存在模块中		commit,   等同于store.$commit		dispatch,   等同于store.$dispatch		getters   等同于store.$getters}
```



利用异步timeout提交mutations里的方法。

为什么使用actions提交timeout不在mutations里呢？

因为如果在mutations里是监听不到这个异步数据的来源以及去向的。但是在actions里是可以的

**用的最多的场景**：仓库和组件交互过程需要通过接口请求完成

 异步修改state中的数据，不能直接修改state，必须基于mutations

仓库中：

```
actions：{	XXXX(store){		xxxxx	}}mutations:{	UPDATE_PAGE(state,payload){      state.nowpage=JSON.parse(JSON.stringify(payload))    	}    }actions:{	SYNC_UPDATE_PAGE(store){      list(store.state.getOrderMsg).then(res=>{        store.commit("UPDATE_PAGE",res.data)	//这里调用mutations里的UPDATE_PAGE方法，将res.data作为UPDATE_PAGE的第二个参数传入进UPDATE_PAGE进行修改仓库的数据      })    }}
```

组件中

```
methods:{	xxx(){		this.$store.dispatch("XXXXactions中的方法名")	}}
```



### modules 

vuex的模块组件化

vuex的模块内部结构和基本vuex实例化传入的数据结构一致

特例：state用函数返回方式书写（就像data

```js
export default{    state(){        return{            放数据        }    },    mutations:{            },    actions:{            },    getters{	},}
```

在index.js中

```js
import xxx from 'url'const store = new Vuex.Store({    modules:{        xxx    }})
```

**但是这么操作之后，shis.$store里的state会变成模块划分的。所以**

要用到命名空间

```js
export default{    namespaced:true,    state(){        return{            放数据        }    },    mutations:{            },    actions:{            },    getters{	},}
```

然后mapState引入要用到第二个参数：名字

...mapState("index.js中引入模块对应的模块名",["xxxxxx"])

```js
...mapState("account",["userList"])
```

还有一种方式：

this.$store.dispatch("account/SYNC_UPDATE_USER_LIST")





### 整体：

```
//引入：import	Vue  from  'vue'import	Vuex  from  'vuex'//注册Vue.use(Vuex)//配置实例化对象:const	store	=	new	Vuex.Store(options:{	state:{		全局的数据	},	mutations:{		方法	},	getters:{		计算属性	}，	actions:{		异步修改数据	},	modules:{		模块	},})//暴露给全局:export	default	store
```

- 暴露给全局

  - ```
    //暴露给全局:export	default	store
    ```

- Main.js接收挂载它

  - ```
    import	store	from	'@/store/index.js'		//引入new	Vue({	store,	//注册})
    ```



# 使用

<p style="background:#ff3300;font-weight:bold;text-align:center;color:#fff">state的数据在组件内，都是放在计算属性中获取的</p>

### state数据的使用：

- ```
  computed:{	this.$store.state.xxx}
  ```

- ```
  在使用的组件里引入 mapState方法import {mapState} from "vuex"使用computed计算方法中引入它computed:{	...mapState(["数据名1","数据名2","数据名3","数据名4"])}然后直接就可以通过数据名使用数据了{{数据名1}}
  ```



### mutations  方法的使用：

- ```
  this.$store.commit("告知提交的对应是哪个计算属性方法",这个方法的第二个参数)this.$store.commit("UPDATE_NAME","这是修改的数据")mutations:{	UPDATE_PAGE(state,payload){      state.nowpage=JSON.parse(JSON.stringify(payload))    	}    }actions:{	SYNC_UPDATE_PAGE(store){      list(store.state.getOrderMsg).then(res=>{        store.commit("UPDATE_PAGE",res.data)	//这里调用mutations里的UPDATE_PAGE方法，将res.data作为UPDATE_PAGE的第二个参数传入进UPDATE_PAGE进行修改仓库的数据      })    }}
  ```

- 组件中引入它

  ```
  import {mapState, mapMutations} from 'vuex'
  ```

- methods里使用它

  ```
  button @click="xxxx这个组件的方法"methods{	...mapMutations(["方法名1","方法名2","方法名3","方法名4"])	xxxx这个组件的方法(){	this.方法名1("要传的参数")					}		}
  ```

- 例

  ```
  {{name}}<button @click="chageName">chagename</button>import {mapState, mapMutations} from 'vuex'computed:{	...mapState(["name"])},methods:{	...mapMutations(["CHANGE_NAME"])	changeName(){		this.CHANGE_NAME("axi")	}}
  ```



### Getters的使用

import {mapGetters} from 'vuex'

也是写在组件的计算属性之中





# VUEX中的dispatch()和commit()

commit: 同步操作
存储

```
this.$store.commit('changeValue'``,name)
```

取值

```
this.$store.state.changeValue
```

 

dispatch: 异步操作

还可以加上命名空间操作dispath("命名空间/要调用的方法名")

存储

```
this.$store.dispatch('getlists',name)
```

取值

```
this.$store.getters.getlists
```

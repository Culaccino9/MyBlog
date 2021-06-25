---
title: Vue路由
date: 2021-06-24 22:51:11
tags: Vue
categories: Vue
---

# 代理与环境变量

vue.config.js

```vue
module.exports={
    devServer:{
        proxy: {
            '/api': {     //这里最好有一个 /
                target: 'http://localhost:5000',  // 后台接口域名
                ws: true,        //如果要代理 websockets，配置这个参数
                secure: false,  // 如果是https接口，需要配置这个参数
                changeOrigin: true,  //是否跨域
                pathRewrite:{
                    '^/api':''			//它是一个正则表达式，表示全部'http://localhost:8001/api转成'http://localhost:5000'
                }
            }
        }
    }
}

```

## 环境变量

vue-cli有三个模式：(package.json下)

- development	开发模式
- test     测试模式
- production      生产模式

```js
  "scripts": {
    "serve": "vue-cli-service serve --mode development", --mode development是默认可以更改
    "build": "vue-cli-service build",
    "lint": "vue-cli-service lint"
  },
```

环境变量就是要结合**模式**使用

环境变量：

创建一个.env.dev文件

环境变量的使用有两种：

- 变量名
  - 可以直接在vue.config.js中通过process.env.变量名来获取
- VUE_APP_变量名
  - 可以在vue应用程序代码中使用（.js，.vue)
  - 使用方式：process.env.变量名



# 前端路由

```js
import Vue from 'vue'
import VueRouter from 'vue-router'

Vue.use(VueRouter)

export const routes = [
    // 主页
    {
        path: '/',
        name: 'index',
        component: () => import("@/views/Layout/Layout"),
        redirect: '/home',
        meta:{
            icon:'sell-zhuye',
            title:'后台首页',
            show:true,
            hasChildren:false
        },
        children: [
            {
                path: '/home',
                name: 'home',
                component: () => import('@/views/Home/Home'),
                meta:{
                    icon:'',
                    title:'',
                    show:false,
                    hasChildren:false
                },
            },
        ]
    },
]


const router = new VueRouter({
    routes
})

router.beforeEach((to, from, next) => {
    if (sessionStorage.getItem("token")) {
        next()
    } else {
        if (to.path === '/login') {
            next()
        } else {
            next('/login')
        }
    }
})

export default router
```



### 优缺点

前端路由优点：

​	1.前端路由速度极快，不用每次都返回通过服务器返回。（因为只是更换组件渲染到DOM上

​	2.前端拥有更多的主动权

前端路由的缺点：

​	1.使用浏览器的前进，后退会重新发送请求，没有合理使用缓存（get请求有本地缓存

​	2.单页面无法记住之前滚动的位置，无法在前进后退的时候记住滚动的位置。



### vue-router基本用法

是vue官方插件，构建单页面应用，配置路由

1.安装模块（插件？）：yarn add vue-router --save

2.src下创建一个router文件夹，创建一个index.js 文件\



### 配置路由

1.在index.js文件中写下配置

​		a.引入：将vue-router和vue引入。

```
import Vue from 'vue'           //全局安装的依赖
import VueRouter from 'vue-router'          //刚刚安装的，引入
```

​		b.注册：使用vue.use(	)	将VueRouter注册在全局

```
Vue.use(VueRouter)      //注册之后，整个vue项目中所有组件内都可以访问到路由对象。
```

​		c.定义路由结构：引用谁？提供一个保存路径的变量

```
const routes = [
    {
        path:'/',                   //当服务器地址为http://localhost:8080/#/         #/时，使用HelloWorld这个组件
        component:()=>import('@/components/HelloWorld.vue')   //路径：把路径对应的组件渲染出来
    },
    {
        path:'/a',                   //当服务器地址为http://localhost:8080/#/a         #/a时，使用HeyCmpt这个组件
        component:()=>import('@/components/HeyCmpt.vue')   //路径：把路径对应的组件渲染出来
    },
]
```

​		router-view有一个name属性。配合components:   自定义名字:路径来实现

​		这样可以实现多个router-view不同显示画面

```
<router-view name="hey"></router-view>

components:{hey:地址}
猜测：components:{
	const hey = ()=>import('地址')
}
```

​		d.实例化：生成实例化对象，将定义路由结构的数据传入其中

​		linkActiveClass属性：linkActiveClass:"类名"		为选中状态添加的类名。选中A，A可以有个样式

```
//实例
// new VueRouter====>生成实例化对象
// 初始化（new）的时候，需要将定义好的路由结构传入进去
const router = new VueRouter({
    routes,         //routes:routes
})
```

​		e.挂载：把实例化对象暴露出去

```
//挂载// 将实例化对象roter暴露出去，让全局引用//在main.js中引入router。并挂载在Vue实例化过程中，让全局使用export default router;
```

2.main.js文件中引入index.js文件，并引入进new Vue()中

​				

```
import Vue from 'vue'import App from './App.vue'import router from '@/router'   //默认这样找index.js文件，省略提升效率Vue.config.productionTip = falsenew Vue({  render: h => h(App),  router      //引入}).$mount('#app')
```



3.vue.app组件中引入他

```
  <router-view></router-view>   
```

所以：我们一般会用一个views文件夹放置HOME文件夹下home.vue，以此类推

![image-20210514200556556](C:\Users\大魔头的computer\AppData\Roaming\Typora\typora-user-images\image-20210514200556556.png)



### router-view详解

router-view是vue-router提供的一个路由出口组件，其主要作用为：根据路由规则来切换渲染对应的组件。（前端路由跳转）

**app.vue是整个uve项目中唯一保持生命活性的组件，这就是什么app.vue被称为入口组件的原因**



### 路由跳转两种方式

1.link跳转

​	a.使用router-link组件就可以实现路由导航跳转

```
<router-link to="/">主页</router-link><router-link to="/about">关于我们</router-link>
```

2.函数跳转

​	函数跳转需要用到router实例

##### 	$router是vue-router的实例，是全局路由对象，包含了VueRouter全部方法和属性

##### 	$route是当前路由对象，包含了当前路由的相关信息

```
 <button @click="toHome">点击跳转到主页</button>      export default {        methods:{            toHome(){                this.$router.push("/")            }        }    }       
```

​	push有两种方式：

​		a.直接 this.$router.push(path)   直接传目标路由地址

​		b.push({path:path,query|params:xxx})

```
this.$router.push({	path:"目标路由地址"	query|parmas:参数//下一张路由可能需要前一张路由的一些信息，就以此传过来})
```



### 路由传参

1.使用query来传递参数

​	a.特点：参数体现在地址栏上，刷新页面，参数不会消失

​					可以用name和query来传参

```
this.$router.push({	path:"/about",	query:{		id:100,	}})
```

2.使用parmas来传递参数

​	a.特点：不支持用path来跳转，要使用name来跳转。name为index.js中结构（定义路由数据）const routes =[ ]中的name属性值

​					参数不会在地址栏体现，刷新页面参数会消失

```
this.$router.push({	name:"about",	parmas:{		id:200,	}})const routes = [    {        path:'/',                   //当服务器地址为http://localhost:8080/#/         #/时，使用HelloWorld这个组件        name:"home",        component:()=>import('@/views/Home/Home')   //路径：把路径对应的组件渲染出来    },    {        path:'/about',                   //当服务器地址为http://localhost:8080/#/about         #/about时，使用HeyCmpt这个组件        name:"about",        component:()=>import('@/views/About/About')   //路径：把路径对应的组件渲染出来    },    {        path:'/contact',                   //当服务器地址为http://localhost:8080/#/about         #/about时，使用HeyCmpt这个组件        name:"contact",        component:()=>import('@/views/Contact/Contact')   //路径：把路径对应的组件渲染出来    },]
```



### 路由嵌套

路由的解析匹配，是层层匹配。 先解析匹配/about，等到/about对应组件渲染完成后，才开始解析匹配/about下的子路由/about-product

​	a.在结构数据下添加children:[   ]数据

```
const routes = [    {        path: '/',                   //当服务器地址为http://localhost:8080/#/         #/时，使用HelloWorld这个组件        component: () => import('@/views/Home/Home'),   //路径：把路径对应的组件渲染出来        children: [            {                path:'/home',                name:'home',                component:()=>import('@/路径地址')            },            {                path:'/home',                name:'home',                component:()=>import('@/路径地址')            }        ]    },]
```

​	b.在页面设置一个出口

```
 <router-view></router-view>   
```

​	c.配置跳转（函数跳转，link跳转）



### 实现登录login和主体内容的切换

1.主页结构为views下的Layout文件夹下的Layout.vue文件

2.让Layout和Login作为一级切换来解决问题。

3.将<u>内容页</u>作为二级子路由存在，然后通过redirect重定向。他的作用相当于，我进入一级路由（主页结构），然后马上展示我的二级路由<u>内容页面</u>

4.redirect: '地址'        默认进入哪个路由   （重定义）



### 路由守卫

在配置的router 	index.js 文件下

语法：

实例化的router.beforeEach((to,from,next)=>{	})

```
const router = new VueRouter({    routes})router.beforeEach((to,from.next)=>{	to====从哪个路由来	from====去到哪个路由	这个函数代表放行的意思。	next()})export default router
```



### 路由动态渲染一级菜单

使用路由的meta属性，暴露export，在一级菜单引入，使用

```js
export const routes = [    // 主页    {        path: '/',        name: 'index',        component: () => import("@/views/Layout/Layout"),        redirect: '/home',        meta:{            icon:'sell-zhuye',            title:'后台首页',            show:true,            hasChildren:false        },        children: [            {                path: '/home',                name: 'home',                component: () => import('@/views/Home/Home'),                meta:{                    icon:'',                    title:'',                    show:false,                    hasChildren:false                },            },        ]    },]
```

然后用template v-for v-if

```js
  <el-menu     class="remove-border"    background-color="#304156"    text-color="#fff"    :router="true">    <template v-for="(item,index) in routes">    <template v-if="item.meta.hasChildren">      <el-submenu v-if="item.meta.show" :key="index" :index="item.path">        <template slot="title">          <i :class="item.meta.icon"></i>          <span>{{item.meta.title}}</span>        </template>        <el-menu-item v-for="(_item,_index) in item.children" :key="_index" :index="_item.path">          <span>{{_item.meta.title}}</span>        </el-menu-item>      </el-submenu>    </template>    <template v-else>      <el-menu-item v-if="item.meta.show" :index="item.path" :key="index">        <i :class="item.meta.icon"></i>        <span>{{item.meta.title}}</span>      </el-menu-item>    </template>  </template>      </el-menu>
```



# 动态路由权限管理

### 1.将路由分组

- 所有人都可以访问的先去渲染页面
- 需要访问权限的单独放置，meta里设置

```js
    {        path: '/order-manage',        component: () => import('@/views/Layout/Layout'),        meta: {            icon: 'sell-dingdan',            title: '订单管理',            visible: true,            hasChild: false,            roles: ['super', 'normal']        },        redirect: '/order-manage/order-list',        children: [            {                path: 'order-list',                component: () => import('@/views/OrderList/OrderList'),                meta: {                    icon: '',                    title: '',                    visible: false,                }            }        ]    },
```



### 2.登录的时候

2.1 sessionStorage.getItem("role")存入临时储存，然后再index.js中取得

```js
let role = sessionStorage.getItem('role')
```

2.2 使用管理员权限信息过滤我们刚刚设置meta需要权限的数组

```js
const dealedRoutes = dynamicRoutes.filter(item => {    return item.meta.roles.includes(role);				//得到复合我们权限的路由数据});
```

2.3然后将这个数组动态添加到router中，使用一个API：

router.addRoute( )来添加他，用法如下

```ts
addRoute(route: RouteConfig)    =>route:RouteConfig，添加的路由addRoute(parentName: string, route: RouteConfig)     =>向某个父节点添加子路由
```

```js
const router  = new Router({    routes:normalRoutes})dealedRoutes.forEach(route=>{    router.addRoute(route)				//router是我们实例化的vue-router对象，简写语法糖})										//向router中添加路由
```



### 3.但是这样会出现问题

因为我们登录才存了role在本地，但是role过滤是在登录之前进行的。所以根本没有路由数组被添加上！！！

单页面应用，不会自动刷新页面，所以role数组就一直不会添加。

手动刷新页面就会添加进去，但是不能这样做！解决方案看4



### 4.所以VueX参与路由管理权限

router.matcher.getRoutes()     ======>本地现有的路由存在这里

console.log(router.marcher.getRoutes)

所以，我们在创建一个新的router实例，在登录时候拿到权限筛选出动态路由添加至新的router实例，

在用新的router实例覆盖掉以前老的router。

router.matcher=newRouter.matcher

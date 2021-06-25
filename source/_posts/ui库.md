---
title: ui库
date: 2021-06-24 22:53:53
tags: Vue
categories: Vue
---

# element-ui库

### 下载：

yarn	add	element-ui	--save

下载完成注意：package.jason文件下

​	a.dependencies===>生产依赖下要有element-ui

​	b.devDependencies====>开发依赖下如果有element-ui把他移上去

### 注册

在main.js中完整注册他。

注册中可以使用第二个参数，为全局属性。

```
//引入
import	ElementUI	from	'element-ui'
import 'element-ui/lib/theme-chalk/index.css';

//注册
Vue.use(ElementUI)		
Vue.use(ElementUI,{size:"small"})
```

### 使用

1.清除默认样式，并引入

​	a.assets(静态文件)下新建一个css文件，下面新建一个base.less文件（为全局清除样式文件）

​																		下面新建一个common.less文件（为全局公用样式文件）

​	b.main.js中引入他

```
import '@/assets/css/base.less'
import '@/assets/css/common.less'
```





# 项目配置

1.在根目录创建vue.config.js文件，并且配置他

```
//module.exports是nodejs的commonJS模块规范
//当前配置会暴露出去用于覆盖vue-cli脚手架的webpack默认配置
module.exports={
	//配置本地服务
	devServer:{
		open:true	//本地服务器编译成功后，自动打开默认浏览器预览项目，需要启动命令设置--open
		//转到package.json里面找到scripts:{server:"vue-cli-service serve"}
									改为：{server:"vue-cli-service serve	--open"}
	}
}
```



# less的使用

1.下载less文件(开发依赖)

less	less-loader

yarn add less less-loader --dev

​	注：如果有报错。可能是less-loader版本太高的原因，remove 他（yarn remove less-loader)。

​			再检查 dependencies和devDependencies

2.style样式下增加语言	lang="less"

```
<style scoped lang="less">
```



# 配置路由

### 设置404页面

我们配置routes其实是遍历循环他，当他找不到下面的所有的时候我们得到一个空白，我们要让他不得到空白，默认*都跳转到404页面

```
    {
        path:'/404',
        name:'404',
        component:()=>import('@/views/404/404')
    },
    {
        path:'*',
        redirect:'/404'
    }
```



# 布局

### 页面布局

`<el-container>`：外层容器。当子元素中包含 `<el-header>` 或 `<el-footer>` 时，全部子元素会垂直上下排列，否则会水平左右排列。

`<el-header>`：顶栏容器。

`<el-aside>`：侧边栏容器。

`<el-main>`：主要区域容器。

`<el-footer>`：底栏容器。

### 基础布局

el-row		代表一整行，内部板块划分通过el-col实现

​					a.		el-row有一个gutter属性，用于控制每一个单元格之间的间距

el-col		  代表一列(一个小模块)，el-row将一行分为24个小格

​					a.		el-col有一个span属性，用于指明当前el-col占多少小格。

​					b.		el-col有一个offset属性，用于设置当前el-col向右偏移的量



# icont字体

1.下载

2.放入assets文件下的font文件下

3.main.js全局下引入css文件

```
// 引入字体图标
import '@/assets/font/iconfont.css'
```

4.修改原本的iconfont.css文件

```
*[class^='login-']  {
  font-family: "iconfont" !important;
  font-size: 16px;
  font-style: normal;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
```

5.使用：

```
<i class="login-xiaoshoue"></i>
```

6.放到input框前的操作

```
<el-input v-model="user.accont">            <i class="sell-user" slot="prefix"></i></el-input>
```



# Form表单

### 表单属性：

el-form上的（他是最外层）

| size        | 输入框尺寸 | string  | medium/small/mini | —        |
| ----------- | ---------- | ------- | ----------------- | -------- |
| placeholder | 占位符     | string  | —                 | 请选择   |
| :inline     | 行内排布   | boolean | 默认为false       | true开启 |
| label-width | 标题长度   |         |                   |          |

el-form-item（他是el-form里的子元素，是el-input的父辈）

- fixed===是否固定。
  - right====固定在右边
  - left====固定在左边
- label=" " ====表单标题

el-input

```
<el-form label-width="80px" :inline="true">      <el-form-item label="订单号">        <el-input v-model="orderInquiry.orderNo"></el-input>      </el-form-item></el-form>
```



### 非空验证

el-form组件上必备三大属性：

```
：model="你要绑定的数据"			将表单数据对象绑定到e-form中：rules="你设置的规则（以对象形式写在date()中）"		验证规则：ref=""				
```

el-form-item组件上必备

```
prop="绑定数据的精确位置"		精准验证
```

例：

```
：model和prop用法：<el-form label-width="0" :model="user">		form绑定user最外层数据	<el-form-item prop="account">			prop绑定account具体的		<el-input v-model="user.account" >		</el-input>	</el-form-item></el-form>data(){	return{		user:{		account:''		}	}}
```

```
：rules用法<el-form label-width="0" :model="user" :rules="userInfRules">	......</el-form>data(){        return{            user:{                account:'',                password:''            },            userInfRules:{			这个规则就是非空验证              account:[{                required:true,                message:'请输入账号',                trigger:"blur",                               }],            }        }    },
```

### 自定义验证

书写步骤：

- 定义一个function验证方法，接收rules，value，callback
  - rules返回当前字段的验证规则
  - value返回当前字段接收到的值
  - callback回调，用于通过验证或者返回错误信息
    - callback()的时候则通过验证
    - callback(new Error('验证失败，请重新输入'))则提示错误
- 新增一条验证规则，验证规则中需要一个validator字段，用于放置定义的function验证方法

```
<script>const 方法名 = (rules,value,callback)=>{	函数判断方法，如：	value.length>8?callback(new Error("账号长度不能大于8")):callback()}在绑定的：rules方法下面添加：{	validator:方法名,	trigger:"触发方式burl,change...."}
```

例：

```
const ifoLength = (rules,value,callback)=>{  value.length>8?callback(new Error("账号长度不能大于8")):callback()}userInfRules:{              account:[              {                required:true,                message:'请输入账号',                trigger:"blur",                               },              {                validator:ifoLength,                trigger:"change"              }              ],    }
```



### 表单整体验证（是否发请求

ref和validate使用：

validate((validate,fields)=>{})

validate得到的值是一个布尔值，true表示验证通过，false表示验证失败

在函数方法中使用ref绑定，并用ref的方法.validate

```
<el-form label-width="0" :model="user" :rules="userInfRules" ref="ifoUser">//提交表单 <el-button style="width: 100%" type="primary" @click="doLogin">登录</el-button> methods:{        async doLogin(){          this.$refs.ifoUser.validate((validate)=>{            if(validate){              console.log(1)验证成功执行的操作            }          })        }    }
```



# table表格

el-table:最外层的容器

​		-需要固定属性======>:data="必须是数组"

el-table-column:一列

​		-label====>字段头

​		-prop====>映射字段，绑定字段。(类似v-moled)		不用带数组名，直接data数组里的index或者。如果数组索引1是对象，那么就是对象的属性名

​		-type="index"    ==>序号

​		-aglin="center"    ====>居中

### 他的作用域插槽找寻此行数据（对此行数据进行操作

配合template使用

v-solt={row,column,$index}

row:为此行的信息

column:为当前一列的信息

$index:为当前的序号（第几行，第几个）

```
<template>	<el-button v-slot="{row}" @click="delth">删除</el-button></temlate>
```



# 解耦	mixin

在单文件的前提下解耦，将一部分当前组件业务无关代码抽取到外界。

1.创建一个mixins文件夹，里面创建一个对应业务的.js文件。

![image-20210527230854502](C:\Users\大魔头的computer\AppData\Roaming\Typora\typora-user-images\image-20210527230854502.png)

2.写法和vue里的script中一模一样

```
const accountIfoLength = (rules, value, callback) => {    value.length > 8 ? callback(new Error("账号长度不能大于8")) : callback()}const passwordIfoLength = (rules, value, callback) => {    value.length > 10 ? callback(new Error("账号长度不能大于10")) : callback()}export default {    data() {        return {            userInfRules: {                account: [                    {                        required: true,                        message: '请输入账号',                        trigger: "blur",                    },                    {                        validator: accountIfoLength,                        trigger: "change"                    }                ],                password: [                    {                        required: true,                        message: '请输入密码',                        trigger: "blur",                    },                    {                        validator: passwordIfoLength,                        trigger: "change"                    }                ]            }        }    }}
```

3.使用：

**在组件内定义一个mixins属性，接收一个数组，将你的所有mixin传入数组即可**

```
//引入import formMixin（自定义名字） from './mixins/form.js'//注册export default {    mixins:[formMixin（自定义名字）],			//使用    data(){return}    }
```



# 导航栏

### 侧边导航

关于index，在一级菜单el-submenu上要绑定1、2、3之类的，下面的el-menu-item也要绑定对应的1-1、1-2等

1、el-menu		导航的最大容器

| router | 是否使用 vue-router 的模式，启用该模式会在激活导航时以 index 作为 path 进行路由跳转 | boolean | —    | false |
| ------ | ------------------------------------------------------------ | ------- | ---- | ----- |
|        | 开启了这个之后，el-menu-item	的index就可以写路由路径，不用1 |         |      |       |

​		所有的导航内容标签都在这里写

1-1、el-submenu		一级菜单

​		内部使用template容器来包裹标题相关内容，且声明插槽为title

​		1-1-1、el-menu-item-group		子菜单分组，主要区分一个系类的菜单项（子菜单标题），写法template声明插槽一样

​				1-1-1-1、el-menu-item			最基本的菜单项（可跳转的）

基本所有的标题：el-submenu、el-menu-item-group的写法都一样

```
<template slot="title">	<i class="sell-icon"></i>    <span>菜单名</span></template>
```

​		例：

```
<el-menu background-color="#304156" text-color="#fff">        <el-submenu v-for="item in 5" :key="item" :index='item'>            <template slot="title">                 <span>商品管理{{item}}</span>            </template>            <el-menu-item :index="item-1">商品管理1</el-menu-item>            <el-menu-item :item="item-2">商品管理2</el-menu-item>            <el-menu-item :item="item-3">商品管理3</el-menu-item>        </el-submenu>    </el-menu>
```





# 引入图片

### 普通图片引入

el-image标签绑定一个 :src="XX"

import XXX from '图片路径'

data里注册他: XX

### base引入（@解释

@在JS作用域下访问路径直接@。但是在HTML  CSS文件中访问加一个@就行

引入图片：

```
<img src="~@/路径">
```



# 滚轮美化el-scrollbar

el-scrollbar

使用：

```
<el-scrollbar style="height:100%;" :native="false">        需要小滚动条的标签</el-scrollbar>
```



# Map方法

new Map([ ])

```
XXX:new Map([	['key','value'],	['key','value'],	['key','value']])
```

语法：

- 设置：map.set(key,value)
- 取：map.get(key)===得到设置的值value

例：

```
      tagMap:new Map([          ['已受理','success'],          ['派送中','warning'],          ['已完成','info']      ])                   <el-tag :type="tagMap.get(row.orderState)">{{row.orderState}}</el-tag>              我们设置一个tagMap，他的键值key为"已受理"等，对应的值为'success'等。我们取就value就用  tagMap.get('已受理')等来取值
```



# 分页器

```
    <el-pagination      @size-change="handleSizeChange"      @current-change="handleCurrentChange"      :current-page="currentPage4"      :page-sizes="[5, 10, 30, 50]"      :page-size="100"      layout="total, sizes, prev, pager, next, jumper"      :total="400"    >    </el-pagination>
```



- ：total======总共多少条
- @size-change="handleSizeChange"====选择当前页显示多少条handleSizeChange（pageSize）方法
  - pageSize默认参数就是对应的**:page-sizes="[5, 10, 30, 50]"**设置的数值
- @current-change="handleCurrentChange"====翻页器---handleCurrentChange(currentChange)方法
  - currentChange默认参数就是对应的页数的值
- :current-page="currentPage4"====当前页
- :page-size="100"====默认选中每页条数
- :total="400"=====总共多少条



# 上传头像

- :on-success		======>   成功执行的函数
  - "handleAvatarSuccess"这个函数有两个参数handleAvatarSuccess（res，file）
  - res====>后台与前台交互数据
  - file====>本地选择中的图片资源

- :before-upload 	======>  上传前的钩子函数，上传前执行

```
<el-upload  class="avatar-uploader"  action="https://jsonplaceholder.typicode.com/posts/"  :show-file-list="false"  :on-success="handleAvatarSuccess"  :before-upload="beforeAvatarUpload">  <img v-if="imageUrl" :src="imageUrl" class="avatar">  <i v-else class="el-icon-plus avatar-uploader-icon"></i></el-upload>
```


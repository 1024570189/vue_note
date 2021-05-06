# Vue Router 路由

### 安装 
+ 直接下载 / CDN
+ https://unpkg.com/vue-router/dist/vue-router.js

### NPM 安装 
```
    npm install vue-router
```

####  使用
+ 创建..src/router/index.js  作为管理路由的文件  还可以配置
+ 配置路由相关信息
    - import VueRouter from 'vue-router'
    - import Vue from 'vue'

+ 引入路由vue
    - import Home from '../components/Home.vue'
    - import About from '../components/About.vue'

+  通过Vue.use（插件），安装插件
    - Vue.use(VueRouter)

 + 创建路由对象， 配置路由用到的页面
 ```
    const routes = [
     配置路由映射关系  上面已经引入路由了 ，这里地址的指向就可以指过去了
        {
            path: '',
            redirect: '/home'
            // 默认显示首页，重定向
        },
        {
            path: '/home',
            component: Home
        },
        {
            path: '/about',
            component: About
        }
    ]
    const router = new VueRouter({
        routes,
        mode: 'history',
        // 修改默认模式为history模式
        linkActiveClass: 'active'
        // 和后面的router-link标签中的active-class = "active"一个效果
    })
 ```

 ##  使用路由 App.vue

 ```
    <template>
    <div id="app">
        <!--写法1-->router-link to  to就是跳转到 的页面 这个是跳转到home 页面  
        <router-link to="/home" active-class = "active">首页</router-link> 
        <router-link to="/about" active-class = "active">关于</router-link>
        <router-view></router-view>
        <!--router-link和router-view是两个全局组件
        router-view起占位作用， 决定渲染页面的位置-->
        
        <!--写法2 start-->
        <button @click="homeClick">首页</button>
        <button @click="aboutClick">关于</button>
        <router-view></router-view>
    </div>
    </template>

    <script>
    export default {
        name: 'App',
        methods:{
            homeClick() {
                //通过代码的方式修改路由 vue-router   
                // this.$router.push('/home')  修改路由
                this.$router.replace('/home')   这个也是
                // console.log('homeClick')
            },
            aboutClick() {
                this.$router.replace('/about')
                // console.log('aboutClick')
            }
        }
    }
        // 写法2 end
    </script>

    <style>
        .active {
            color: #f00
        }
    </style>
 ```
 ## this.$router.push跳转到指定URL，点击后退会返回至上一个页面
 ## this.$router.replace跳转到指定URL，点击后退会返回至上上一个页面  就是会后退两次的感觉 但是页面也是后退一次的视感。

## this.$router.go(n)类似window.history.go(n)，向前或向后跳转n个页面，n可正（先后跳转）可负（向前跳转）

+ 使用： this.$router.go(1)  前进
+ 使用： this.$router.go(-1) 后退

### router-link属性：
+ to: 目标地址，渲染成a标签
+ tag="button" 将router-link渲染成button标签，点过之后的btn多了两个class：router-link-active和router-link-exact-active可以用来修改style
+ replace 没有值 将pushState改为replaceState（禁用返回前进按钮）
+ active-class = "active": 修改默认的router-link-active为active

# 动态路由匹配

# this.$route和this.$router的区别
+  $route 上 有query 也有 params  分别是给 利用前端vue路由 显示传值和隐藏传值而接值用的
+ $router上的 proto里 有我们常用的一些vue 路由方法 比如 push 比如 back 以及go
+ 第一个页面 
``` 
<template>
  <div id="app">
     <!-- 可以通过计算属性获取$route中的userId -->
  	<h2>{{userId}}</h2>
      <!-- 也可以直接获取 -->
      <h2>{{$route.params.userId}}</h2>
  </div>
</template>

<script>
	export default {
        name: "User",
        computed: {
            userId() {
                return this.$route.params.userId
                // 动态获取userId
            }
        }
    }
</script>

```

+ 第二个页面路由页面

```
import User from '../components/User.vue'

const routes = [
    // 5.2.2 配置路由映射关系
    {
        path: '',
        redirect: '/home'
        // 默认显示首页，重定向
    },
	{
        path: '/home',
        component: Home
    },
    {
        path: '/about',
        component: About
    },
    {
        path: '/user/:userId',  看这里  这里是 动态id  格式是/:id  拼接在path的后面
        component: User
    }
]
```
+ 第三个页面
```
    <template>
  <div id="app">
      <router-link to="/home">首页</router-link> 
      <router-link to="/about">关于</router-link>
      <router-link :to="'/user/'+userId">用户</router-link>
      
      <router-view></router-view>
  </div>
</template>

    <script>
        export default {
            name: 'App',
            data() {
                return {
                    userId: 'jack'
                }
            },
            methods: {
                ...
            }
        }
    </script>
```


# 路由嵌套 
### 一个路径映射一个组件，访问这两个路径也会分别渲染两个组件.
```
- 创建src/components/HomeNews.vue, HomeMsg.vue
<template>
	<div>
        <ul>
            <li>新闻1</li>
            <li>新闻2</li>
            <li>新闻3</li>
            <li>新闻4</li>
        </ul>
    </div>
</template>
...
```

### 下面的是路由的配置的页面  还有 懒加载的设置

```
-index.js
// 路由懒加载
const Home = () => import('../components/Home.vue') 这样写 就是用到这个路由页面的时候 就是才加载出来这个页面  节约开销 和初始化的加载时间
const HomeNews = () => import('../components/HomeNews.vue')
const HomeMsg = () => import('../components/HomeMsg.vue')

const About = () => import('../components/About.vue')
const User = () => import('../components/User.vue')

const routes = [  
    {
        path: '',
        redirect: '/home'
    },
	{
        path: '/home',
        component: Home,
        // 路由嵌套在这  
        children: [  children 就是 home 下面的类似于子节点  就是子路由
            {
              // 默认子路径
              path: '',
                redirect: 'news'  默认就是news 路径地址，如果不是 也是会redirect 重定向在这，不可能不是这个 不是这个路径地址的话，就是代码有问题了
            },
            {
                path: 'news',
                // 子路由的地址path不需要加"/"
                component: HomeNews
            },
            {
                path: 'msg',
                // 子路由不加"/"
                component: HomeMsg
            }
        ]
    },
    ...
]

```

# 传递参数
 + params的类型
    - 配置路由格式： /router/:id
    - 传递的方式： 在path后面跟上相应的值
    - 传递后形成的路径： /router/123， /router/abc

 + query的类型
    - 配置路由格式： /router 也就是普通配置
    - 传递的方式： 对象中使用query的key作为传递方式
    - 传递后形成的路径： /router?id=123， /router?id=abc


  ```
  - index.js 这个是路由的信息
...
const Profile = () => import('../components/Profile.vue')
...
{
    path: '/profile',
    component: Profile
}
- App.vue
    <template>
        ...
        <!--方式1： router-link方式-->
        <router-link :to="{path: 'profile', query:{name: 'jack',age: 18}}">档案</router-link>

        <!--方式2：btn+点击事件-->
        <button @click='profileClick'>档案</button>
    </template>
    <script>
        export default {
            name: 'App',
            data(){
                return {
                    userId: 'zhangsan'
                    
                }
            }
            methods:{
                ...
                profileClick() {
                    this.$route.replace({path:'profile', query:{name:'jack',age:18}})
                }
            }
            
        }
    </script>
    ==========================================

    - Profile.vue
    <template>
        <h2>profile页面</h2>
        <h2>{{$route.query.name}}</h2>  这个是 jack
        <h2>{{$route.query.age}}</h2>   这个是 18
    </template>
    <script>
        export default {
            name: 'Profile'
        }
    </script>
    此时的url：
    localhost:8080/profile?name=jack&age=18
  ```

# 404 页面
+   常规参数只会匹配被 / 分隔的 URL 片段中的字符。如果想匹配任意路径，我们可以使用通配符 (*)
```
    {
        path: '*',
        component: Error
    }
当所有的页面都路由匹配不到的话，就会进入到404页面  可以在404页面里面编辑一些 我们404 页面想要展示的页面
```

# 匹配优先级
### 有时候，同一个路径可以匹配多个路由，此时，匹配的优先级就按照路由的定义顺序：路由定义得越早，优先级就越高。


# 全局导航守卫

### 在spa单页面应用中，改变网页的标题

```

const routes = [
    {
        path: '',
        redirect: '/home'
    },
	{
        path: '/home',
        component: Home,
        children:[...], children 老规矩还是嵌套路由
        meta:{
            //元数据
            title:'首页'
        }
    },
    {
        path: '/about',
        component: About,
        meta:{
            title:'关于'
        }
    },
    {
        path: '/user/:abc',
        component: User,
        meta:{
            title:'用户'  title 就是网页的标题  可以在这里设置的时候就定义好
        }
    }
]
```
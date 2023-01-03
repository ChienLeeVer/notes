# Vue-router

标签（空格分隔）： vue vue-router

---

### 路由初体验
>前提：sudo npm install -g vue-cli@3
初始化vue项目：vue create vue-router-learn
安装依赖：sudo npm install --save-dev vue@2 vue-router@3


1.  在component文件夹下写好需要显示的页面组件内容
home.vue

```
<template>
    <div>
        <h1>This is your home</h1>
    </div>
</template>

<script >
export default {
    name : 'home'
}
</script>

```

about.vue

```
<template>
    <div>
        <h1>this is my email:123456789@qq.com</h1>
    </div>
</template>
<script>
export default {
    name : 'about'
}
</script>
```

2.  在router文件夹下写一个路由
index.js

```
\\1.    引入需要显示的页面路由组件和vueRouter
import home from './home.vue'
import about from './about.vue'
import VueRouter from 'vue-router'
import Vue from 'vue'

\\2.    使用Vue-router
Vue.use(VueRouter)

\\3.    配置routes路由项，说明：配置项为数组，数组元素为对象，每个对象包含两个属性path地址和component需要显示的组件
const routes = [
    { path: '/', component: home },
    { path: '/about', component: about}
]

\\4. 创建router实例，传入配置项
const router = new VueRouter({
    routes
})

\\暴露路由
export default router
```

3.  在入口文件main.js中加入路由

```
import router from './router.vue'
import Vue from 'vue'
import App from './App.vue'

Vue.config.productionTip = false

new Vue({
    render : h=>h(App),
    router //将路由添加到根vue实例中
}).$mount('#app') //将App.vue挂载到public文件夹中index.html的id为app元素上
```

4.在根组件App.vue的模板中使用路由链接

```
<template>
    <div id="app">
        <p>
            <router-link to="/">Go to Home</router-link> //自动转化为a标签，属性to转化为href,如果想让router-link转为指定类型标签，如li标签，需要添加属性tag="li"
            <router-link to="/about">Go to About</router-link>
        </p>
        <router-view></router-view> //显示对应链接的模板内容
    </div>
</template>

<script>
export default {
    name: 'app',
}
</script>

```

5.npm run build启动本地服务器

### 动态路由匹配
1.修改路由配置js

```
const routes = [
    { path: '/', component: home },
    { path: '/about', component: about},
    { path: '/user/:id', component: User},
    { path: '*', component: error } //*表示当找不到以上路由时，匹配这个路由
    //配置路径时，后面加上冒号和参数名，如:id,跳转到这个路由时，id会保存到$route.params.id，也可以从$route.params.id中获取，如果是js中则通过this.$router.paramas.id获取。我们知道在APP.vue中把route组册到了根实例中，因此可以通过$route访问到。
]


```

        注意：目的地和当前路由相同，只有参数发生了改变，那么组件将会被复用而不是销毁重建，因此生命周期函数将不会被再次调用，因此就用watch:{ $route(to, from) { /*...*/} } 或者路由守卫beforeRouteUpdate(to, from, next){}，通过watch监听或者路由守卫能够实现在动态路由参数发生变化时，异步请求响应的数据。

2.为APP.vue添加路由

```
<router-link to="/user/id">Go to user</router-link>

```

### 嵌套路由
1.修改路由配置组件index.js
在路由配置项的数组的对象中，只需要添加children属性即可，值为类似每个路由配置，需要注意的是，在子路由中path开头无需‘/’，因为‘/’代表跟路径
```
const routes = [
    { path: '/', component: home },
    { path: '/about/:id', component: about},
    {
        path: '/user/:id',
        component: User,
        children: [
            { path: '', component: userHome}, //path为‘’表示空路由即/user/:id/
            { path: 'profile', component: userProfile } //匹配/user/:id/profile,

        ]
    }
]
```

2.然后在user.vue中使用router-view,当匹配到/user/:id/profile时将会显示uerProfile组件的内容

```
<template>
    <div class="User">
        <h2>User : {{ $route.params.id }}</h2>
        <router-view></router-view>
    </div>
</template>
<script>
export default {
    name: 'user'
}
</script>
```

### 编程式导航
除了router-link创建a标签导航实现跳转外（实质也是调用$router.push（）），还可以有以下方法：

1.  this.$router.push(参数1,参数2,参数3)
方法说明：该方法等同于```<router-link to="参数"></router-link>```,点击之后/使用该方法后会转化为a标签并且在页面不刷新的情况下实现跳转。并且会将当前url推进history栈中
参数说明：参数1可以是字符串，可以是对象。参数2和参数3为回调函数，表示跳转成功/失败之后执行的方法。如果参数2和参数3没有提供那么，push方法返回一个promise对象。
使用案例：

```
this.$router.push('about') //使用该方法后跳转到/about
this.$router.push({ path:'about' })
this.$router.push({ path：`/user/${user.id}` })  //路由：user/123 ，假设user.id 为123
this.$router.push({ name: 'user', params: {id: 123} })  //路由：user/123 ，注意params只能和name一起使用
this.$router.push({ path:'user', query:{ id:123 } })//查询路由：/user?id=123

```
        注意：{ path: 'user', params: { id: 123 }}中params并不会生效，即实质匹配到/user，所以想在router.push中使用params,只能是使用命名路由

2.  this.$router.replace()
方法说明：该方法等同于```<router-link to="参数" replace></router-link>```,点击/调用该方法将会将当前页面调转到目的页面，但是不会push进history栈，并且栈中当前的url替换掉

3.  this.$router.go(整数)
方法说明：表示在history记录中，以当前url在栈中的位置为基准进行路由跳转

### 命名路由
为路由定一个名字,方便传参数
router文件的index.js
```
const routes = [
    { path: '/', component: home },
    { path: '/about', component: about},
    {
        path: '/user/:id',
        name: 'user', //为路由定义一个名字
        component: User,
        children: [
            { path: '', component: userHome},
            { path: 'profile', component: userProfile }

        ]
    }
]
```

在组件中

```
<router-link to="{name:'user', params:{ id: 123}}">Go to user</router-link> //跳转 user/123
```

### 命名视图
概念：既然可以给路由命名，自然各个路由可以直接通过name区分。现在出现一种情况，我想同时展示多个组件，而不是一个。即在访问一个路由时，同时显示多个<router-view></router-view>，那么router-view如何对应多个组件。答案是可以给router-view的name属性赋值，表示这个路由视图对应哪个路由组件，如<router-view name="xxx"></router-view>。

如何用:

```
//app.vue中
<router-view></router-view>
<router-view name="a"></router-view>
<router-view name="b"></router-view>

//router.js中
 {
    path:'/about',
    components: { //注意！！！不是component，而是components表示多个
        default : about,
        a : usera,
        b : userb
    }
}
```

### 重定向和别名
是什么：重定向即为一个路由重定向至另一个路由。别名即为一个路由定义其它名字，访问这个名字等同于访问这个路由。注意：导航守卫无法应用到重定向上
为什么用：略
怎么用：

```
router.vue中
{
    path:'/',
    component: home,
    redirect:'/about' //将路由至/home改到/about,
    alias : '/me' //为/路由起一个名字me,当访问/me时等同于访问/
}
//redirect属性值可以是字符串、命名路由、方法，如下
{
    path:'/',
    component: home,
    redirect:{ name:'about'}

}
{
    path:'/',
    component: home,
    redirect:to => {
         // 方法接收 目标路由 作为参数
         // return 重定向的 字符串路径/路径对象
    }
}

```

### 路由组件传参
是什么：通过在router.vue配置路由时，添加props选项，就可以将参数传到组件中去，而不用在组件通过```this.$route.params.xx```的方式访问动态路由的参数。
为什么：这样做的目的是防止组件和路由高度耦合，因为有些时候，组件并不需要使用this.$router.paramas。
怎么用：

```
//router.vue中
{
    path:'/user/:id',
    component :user,
    props : true //props选项设置为true后会将id传到组件中，组件通过props接收
}
//user.vue中
export default {
    name : 'user',
    props :['id']
}
//如果想为命名视图传递参数，如下
{
    path:'/user/:id',
    component :{
        default : user,
        nav : nav,
        bar : bar
    },
    props : {
        default : true,
        nav : true,
        bar : true
    }
}
```



### 导航守卫
>顾名思义，在路由跳转前后执行某些方法

1. 全局前置守卫：
（1）what:在路由跳转之前进行验证。所有导航守卫都是一个promise,只有所有守卫resolved之后，才会进行跳转
（2）how:```const router = new VueRouter({...}); router.beforeReach()```

```
    router.beforeEach((to, from, next)=>{})
    //to : 表示目标路由对象
    //from : 表示当前准备离开的路由
    //next：next是一个方法，一个钩子中只能调用一次next方法，next参数和this.$router.push()的参数一样。next()表示进行管道中的下一个钩子，所有钩子执行完之后，就resolved。next(false)表示终止路由并跳转到from
```

```
//实现用户验证
router.beforeEach((to, from, next)=>{
    if(to.name !== 'login' && isAuthenticaed ) next({name:'login'})
    else next()
})
```

2. 全局后置守卫
（1）what:实现跳转后执行某些方法，但不能改变跳转的方法
（2）how:

```
router.afterEach((to, from)=>{
    //不会改变路由结果
})
```

3. 路由独享守卫
（1）what: 即给某个路由添加守卫，只有一个beforeEach
（2）how:

```
//router.vue
{
    path: '/',
    component: home,
    beforeEach(to, from, next){
        //...
    }
}
```

4. 组件内路由

> 在组件内，像添加data那样，给组件添加beforeRouteEnter,beforeRouteUpdated,beforeRouteLeave方法

```
    const user = {
        template : '<div>home</div>',
        beforeRouteEnter(to, from, next) {
            //在全局路由守卫resolved之后，组件创建之前调用。此时组件的实例尚未创建，但是可以通过next的回调函数调用，如next((vm)=>{ //操作vm })
        },
        beforeRouteUpdated(to, from, next) {
            //当前路由被改变，但只是参数改变，组件被复用时调用。此时可以访问组件的实例
        },
        beforeRouteLeave(to, from, next) {
            //当前路由被改变之前
        }
    }
```

5.导航解析流程
> (离开)组件路由守卫->（进入前）全局路由守卫->[(复用)组件路由]->（进入）路由配置守卫->(进入)组件路由守卫->(进入后)全局路由守卫->dom更新->调用(进入)组件路由守卫中的next回调函数

（1）导航被触发。
（2）在失活的组件里调用 beforeRouteLeave 守卫。
（3）调用全局的 beforeEach 守卫。
（4）**在重用的组件里调用 beforeRouteUpdate 守卫。**
（5）在路由配置里调用 beforeEnter。
（6）解析异步路由组件。
（7）在被激活的组件里调用 beforeRouteEnter。
（8）调用全局的 beforeResolve 守卫 。
（9）导航被确认。
（10）调用全局的 afterEach 钩子。
（11）触发 DOM 更新。
（12）调用 beforeRouteEnter守卫中传给next的回调函数，创建好的组件实例会作为回调函数的参数传入。

### 路由懒加载
what:能够实现组件按需加载，即当访问到该路由时才加载该组件
why:如果所有组件从js打包后，当加载页面时，所有组件都会加载，会导致页面加载变慢
how:通过将组件封装在一个返回Promise.resolve(组件)的工厂函数中，然后暴露。

```
    foo.vue
    const foo = ()=>{ return Promise.resolve(组件对象)}
    export default foo

    router.vue
    const Foo = () => import('./foo.vue')
```

### 其它：
1.this.$route和this.$router的区别：this.$router是VueRouter的一个实例，在vue根实例注入，而this.$route是当前激活的路由对象信息,这个属性是只读的,如this.$route.params.id

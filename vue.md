# Vue

标签（空格分隔）： vue

---

### vue简介：

vue为MVVM架构，使用虚拟DOM渲染，组件化。通过new vue({ el:'', data(){return {  }}, methods:{ }, filters:{ }, directives:{ } })来控制数据的变化。el表示该vue实例需要控制的对象，#xx表示控制id='xx'标签元素区域以及数据，data为一个函数，返回一个对象，对象里面是需要的数据，可以在标签中通过{{ vue实例中的数据 }}中使用。methods的value是一个对象，对象里面可以定义方法，filters里面可以定义自由化数据格式化函数。directives可以定义私有化指令。

### vue的系统指令与用法：

1. v-bind: 即可绑定标签属性，从而动态修改属性值，如```<img v-bind:src="vue实例中data的数据或者方法返回值">```等价于```<img :src="xxx">。```

2. v-on:事件名称：可以为对应的标签添加事件如```<a href="#" v-on:click="vue实例中的方法名">``` 等价于```<a href="#" @click="vue实例中methods的方法名">```。除了click还有其它事件

3. 事件修饰符：.事件修饰符，如.prevent阻止浏览器默认行为，.stop阻止冒泡行为，.capture父元素先捕获, .self事件仅作用于自身时发生冒泡传递到自身时不触发事件，可以连用如.prevert.stop,如```<a href="#" @click.stop.prevent="vue实例中methods的方法名">```;
(1)number:自动调用parseFloat(输入值),如果无法解析则返回原始值.
>> .sync修饰符是对组件属性的双向绑定的优化。过去父组件通过子组件的属性传递数据，并绑定一个事件，然后子组件通过$emit触发事件名称，并传递数据。如今只需要在属性上添加prop-name.sync=“xxx”，在子组件中this.$emit('update:prop-name',xx)即可，无需在组件中再另外绑定事件。需要注意的是，添加sync修饰符后不能再用赋值表达式给属性赋值。


4. v-cloak:通常与{{ }}表达式一起使用，{{ xx }}是事先用占位符，等待xx数据加载，当加载完毕后替换这个占位符，因此在等在加载的时间内，该处显示空白，为标签添加v-cloak后将可以通过添加样式[v-cloak] {xxx}来控制等待数据加载的时间里的样式

5. v-text="data数据变量":作用与{{ }}相似，但是不会出现等待加载事件，如```<p v-text="vue实例中的data数据名称"></p>```'

6. v-if="判断语句"：在标签中添加该属性后，标签将会在满足条件时显示,如```<a href="#" v-if="xxx">```,可以与v-else一起使用，必须在v-if或者v-else-if后面一起使用，中间不能夹杂任何其它标签。指令为真正的条件渲染，会删除/添加dom元素，并销毁/重建内部的事件监听器和子组件，并调用组件的生命周期函数，会消耗性能。并且v-if和v-else需要为其添加key，防止复用组件

7. v-show=“判断语句”：在标签中添加该属性后，标签将会在满足条件时显示。只是单纯css的display切换，消耗性能较少，适合频繁切换。

8. v-for="item in 对象/数组变量"：在标签中添加该属性后，将会遍历该标签，此时在子标签内可以通过{{ item }}的方式访问item临时变量。如```<p v-for="item in arr" :ket="唯一id">{{ item }}</p>```此时``data(){ return { arr: ['one', 'two', 'three', 'four']}}```，会生成4个p标签,文本分别为one two three four。当然v-for="(item, index) in arr"中, index为数组索引/对象属性名。key的作用为vue提供唯一的key，方便vue追踪每个节点，以便原地更新数据而不用重新渲染，key类型一般为字符串或数值

9.  v-once: 标签中使用这个命令后，只创建一次，并缓存这个标签，重新渲染时不会创建，直接从缓存中取。

10.  自定义修饰符：修改全局配置，如 ```Vue.config.keyCodes.f2 = 113;此时就可以使用修饰符f2了```，如```<input type="button" @keyup.f2="methods中

11. 自定义系统指令：

（1）全局定义系统指令：如```Vue.directive('focus', { bind(el){},inserted(el){},updated(el){}});```注意bind函数只执行一次，并且在标签还没插入dom之前就执行了，inserted表示在插入dom后执行且只执行一次，updated方法在标签数据更新后执行，可能执行多次，el为必须的参数表示当前操作的dom对象，el等价document.getElementById()获得的对象；

（2）私有化系统指令：只能在该vue实例管控下使用的指令，如

```
new vue({
        el : '',
        data(){ return{ } },
        directives: {
            focus: {
                    bind(el){},
                    inserted(el){},
                    updated(el){}
                }
        }
    });
```

    就可以像v-if那样使用v-focus了。

12. 为标签绑定class：

```
<div id="app">
        <div class="inner" :class="classObject"></div> //class = inner active
    </div>
    <script>
        var vm = new Vue({
            el : '#app',
            data () {
                return {
                    classObject : {
                        active : true,
                        error : false //只有value为true的key会显示在class中
                    }
                    /*
                        方式二：数组
                        :class = "[ activeValue, errorValue ]"
                        data(){ return { activeValue:active, errorValue:error }}
                        结果： class = inner active errorValue
                    */
                }
            }
        })
    </script>
```

13. 为标签绑定内联样式

```
<div id="app">
        <div class="inner" :style="innerStyle"></div> //class = inner active
    </div>
    <script>
        var vm = new Vue({
            el : '#app',
            data () {
                return {
                    innerStyle : {
                        fontSize : '16px',
                        color : 'red',
                        display : ['=webkit-box', '-ms-flexbox', 'flex']
                    }

                }
            }
        })
    </script>
```
14. 计算属性：computed：{},里面的方法可以直接使用，用于对data中的数据进行加工，对执行同一种操作的数据进行抽象分离，提高复用性，并且计算属性具有缓存的作用，如果计算属性的所用到的数据没有发生变化，那么计算属性不会重新执行

15. 数组：vue规定当data中的数据类型为Array时，只能使用push、pop、shift、unshift、splice、sort、reverse方法来修改数组，才能响应式处理数据，而通过下标或者直接修改数组长度的方式无法响应式处理。如果需要使用其它方法，只能修改其引用，如this.arr = [] 或者set方法

16. 对象：只有在data中定义的对象及其属性可以响应式处理。新增的对象属性不会得到响应式处理。需要响应式处理可以修改this.odlObj = Object.assign({},this.oldObj, newObj)其引用，或者使用Vue.set / vm.$set

### vue中常见的问题

1.  如何解决table标签内使用组件，组件显示到table外或者失效的问题？ 答：table标签里面使用组件，需要为tr绑定is属性，值为组件名称。此类情况会发生在ul>li, select>option, table>tr>td上，以此类推
```
//失效情况：
<select>
    <item/>
</select>

Vue.component('item',{
    template:'<option>1</option>'
})

//有效情况
<select>
    <option is="item">
</select>

Vue.component('item',{
    template:'<option>1</option>'
})

```

### 生命周期函数流程图

<img src="http://img.smyhvae.com/20180611_2130.png">

```
    new vue({
        el : '#app',
        data(){
            return {

            }
        },
        methods: {

        },
        beforeCreate(){
        },
        created(){
        },
        beforeMount(){
        },
        mounted(){
        },
        beforeUpdata(){
        },
        updataed(){
        },
        beforeDestroy(){
        },
        destroyed(){
        }
    })
```

一个vue实例将会经过8个生命周期函数,beforeCreate表示申请为数据申请内存空间，变量尚未初始化，created表示数据已经初始化完毕，beforeMount表示模板编译完成生成虚拟DOM但还没挂载到页面中，mounted表示模板已经挂载到页面中，可以获取DOM元素，beforeUpdata表示数据已经更新，但是没有渲染到页面中，updataed表示数据已经重新渲染完毕并显示到页面中。beforeDestroy表示在实例销毁之前，destroyed表示已经解除绑定管控区域和子区域，解除事件绑定，解除事件监听器，此时无法获取DOM元素。

 （1）关于beforeUpdated说明：因为vue内部对数据的更新不是立刻执行，而是采取了异步队列。

 （2）如果在数据更新之后想执行某些操作，则this.$nextTick(()=>{
//表示在DOM更新循环之后立即执行某些操作
})。this.$nextTick()可以在created等钩子函数中使用，无论dom是否渲染，nextTick都会等到异步队列渲染完这个DOM后立刻执行，与nextTick（)处在哪个位置没关系。该方法返回一个Promise

### ajax请求
>通常使用vue-source.js, 这个插件配置了this.$http.get(),this.$http.post(),this.$http.jsonp()三种数据结构请求方式，其中jsonp支持跨域请求。

```
    this.$http.get(url + 需要传送的变量).then((response)=>{
        //接口返回的数据在response中
    });

    this.$http.post(url, {以对象的形式传送变量}, { emulatJSON: true}).then((response)=>{
        //接口返回的数据在response中
    });

    this.$http.jsonp('http://vuecms.ittun.com/api/getlunbo?id=1').then((response)=>{
        //接口返回的数据在response中
    })
```

除了以上三种还有axios请求。

### JSONP原理

JSONP的实现原理：通过动态创建script标签的形式，用script标签的src属性，代表api接口的url，因为script标签不存在跨域限制，这种数据获取方式，称作JSONP（注意：根据JSONP的实现原理，知晓，JSONP只支持Get请求）。

### 指令

1. 动态属性键值：如果使用了v-bind:[defineName]=“value”,则vue会获取data中的definename和value属性的值，注意：[]中的全部大写会被转为小写然后再找defineName，如果data中为defineName则会找不到；

2. 动态类名：元素的class = 标签中class + :class ；

3. 事件处理： （1）如何在有参数的函数中访问事件的event对象，可以这样v-on:click = "getMessage(参数1, $event)"；

### 表单输入绑定

1. 在复选框和选择框中。vue会自动将选中项的value添加到data中。用法：v-model=“xx”表示xx是vue实例中data的一个数据，我们假设为数组，而在点击选中时，vue会获取表单中value的值然后添加到xx中。如果是单个选项则v-model对应的vue实例中data初始值设置为空字符串，如果是多选则data中的初始值设置为空数组。

2. 修饰符：

（1）v-model.lazy表示数据将会在change事件之后更新，而不是input时就更新，比如搜索框；

（2）v-model.number表示限制用户输入的数据为数字；

（3）v-model.trim表示自动过滤用户输入的首尾空白字符

### 组件

1. 子组件向父组件通信的方法：在父组件控制的区域下，使用子组件的时候，可以使用自定义事件系统，具体做法如下：子组件调用父组件方法并传值,$emit为触发实例上的事件,注意事件名不存在大小写转换，只有标签名才会转换，为了防止大小写混乱，一致使用 -，而不是驼峰命名

```
<div>
    <componentDefine v-on:自定义事件名=“父组件函数(参数1...)”></componentDefine>
</div>

//子组件中：
Vue.component('componentDefine',{
    template: '<div v-on:click="$emit('自定义事件名',参数1...)"><div>'
})

```

2. 父组件向子组件通信的方法：在使用子组件的时候，将父组件的值传到子组件的属性上，然后子组件在props中接收，具体做法如下：

```
<div id="app">
    <componentDefine v-bind:title="test”></componentDefine>
</div>
<script>
    new Vue({
    el : '#app',
    data(){
        return {
            test: '123'
        }
    }
})

    Vue.component('componentDefine',{
        props:['title']
        template : '<div>{{ this.title }}</div>'
    })
</script>
//子组件不能修改props中的属性值，特别是传递的数据为对象或者数组,可以在data中拷贝或者computed中拷贝
```

3. 实现动态组件：即一个标签中绑定不同的组件

```
<component v-bind:is="currentTabComponent"></component>
//其中currentTabCompnent为自定义组件的名称
```

如果在切换不同的组件时，不想重新渲染，就需要缓存，缓存方法如下,注意：此时mounted和unmounted生命周期钩子被替换为activied和deactived ,keep-live不会出现在组件链中，而且有属性include[string|Regex]、exclude[string|Regex]、max分别表示包含哪些组件、不包含哪些组件、最多缓存多少个组件（超出则销毁之前缓存的组件)

```
<kepp-live>
    <component v-bind:is="currentTabComponent"></component>
</keep-live>

//其中currentTabCompnent为自定义组件的名称
```

4. 子组件接受属性验证，即父组件通过属性值的方式向子组件传递数据，该数据被指定通过某种验证，具体如下：

```
    Vue.component('componentDefine', {
        props : {
            title : String, //title的数据类型为String
            age : Number, //age的数据类型为数值类型，基础数值类型无法验证null和undefined
            major : {
                type : String,
                required : true //major为必填的字符串
            },
            detailsMessage : {
                type : String,
                default : function() { return { "this is a test" }} //默认值为一个字符串
                validator : function(value) {
                    //对value进行自定义验证
                    //return true or false
                }
            }
        }
    })
```

5. 组件中使用v-model：vue中，若父组件中用v-model的话，其实相当于v-bind:value='testValue'和v-on:input='testValue=$event.target.value'。即通过触发input事件修改value值。
通过配置子组件的model，更改默认传递value和默认触发input事件

```
 <template id="text">
        <div>
            <input type="text" :value="name" @input="test">

        </div>

    </template>

    <div id="app">
        <base-model v-model="name"></base-model>
        <div>{{ name }}</div>
    </div>

    <script>
        window.onload = ()=>{
            Vue.component('base-model', {
                model: {
                    prop: 'name',
                    event: 'diyFun'
                },
                props: ['name'],
                template: '#text',
                methods: {
                    test(event) {
                        this.$emit('diyFun', event.target.value)
                    }
                }
            })
            var vm = new Vue({
                el : '#app',
                data () {
                    return {
                        name : '张三'
                    }
                }
            })
        };
    </script>
```

解释如下：
父组件通过v-model将data中的name值传到了子组件，并且通过p标签来验证name是否被修改了。
子组件中修改了model,prop设置为了name,event设置为diyFun。此时表明在父组件中```<base-model></base-model>```标签绑定了一个name属性，监听了diyFun事件,这个diyFun事件需要获取一个参数来修改name值，这都是内部实现的。现在我们来看子组件，既然父组件传递了一个name属性，则需要通过props接收name属性，从而实现了父组件向子组件传递数据，同时把接收到的值用在子组件的value属性中，当真正的子组件触发了input事件时，我们调用了子组件的方法，在子组件里面通过this.$emit调用了父组件给子组件监听的事件diyFun,并且传递真正的子组件中的value值给diyFun事件，这样diyFun事件就获取到了所需要的参数，并且修改了父组件的name值。
这实际上就是父组件和子组件之间的数据传递，不同的是修改了v-model默认绑定的属性和事件。

6. 插槽：在组件模板中使用```<slot></slot>```可以将组件标签中任何内容（文本，html，其它组件）插入到模板之中，如果组件模板没有这个标签，那么组件标签中的内容将会被抛弃；如下，Your profile!文本将会被插入到a标签之中。如果slot之中原本就有内容，如```<slot>默认值</slot>```，在组件标签没有内容时将会使用这个默认值。

```
<template id="t-slot">
    <div>
        <a :href="url" class="nav-link"><slot>默认值</slot></a>
    </div>
</template>
<div id="app">
    <navigation-link url="#">
        Your profile!
    </navigation-link>
</div>
<script>
    var vm = new Vue({
        el : '#app',
        components: {
            'navigation-link' : {
                template : '#t-slot',
                props: ['url']
            }
        }
    })
</script>
```

如果想为插槽定义一个名字，那么可以这样使用,为模板中的slot的name属性赋值，然后在组件标签中使用***template模板占位符*** v-slot:name的值，就可以绑定slot了。
注意：具名插槽必须使用template

```
<template id="t-slot">
    <div>
        <a :href="url" class="nav-link"><slot>默认值</slot></a>
        <header>
            <slot name = 'header'></slot>
        </header>
    </div>
</template>
<div id="app">
    <navigation-link url="./test.txt">
        your profile
        <template v-slot:header>
            这里放头部的东西
        </template>
    </navigation-link>
</div>
<script>
    var vm = new Vue({
        el : '#app',
        components: {
            'navigation-link' : {
                template : '#t-slot',
                props: ['url']
            }
        },
        data () {
            return {

            }
        }
    })
</script>
```

如果父组件想访问子组件中插槽的数据,可以把子组件的数据绑定到插槽的属性上，然后父组件通过v-slot:插槽名=“xx”接收子组件的属性值，并且将这个接收到的属性值命名为xx,就可以在父组件中使用xx了

```
<template id="t-slot">
    <div>
        <a :href="url" class="nav-link"><slot>默认值</slot></a>
        <header>
            <slot name = 'header' :user="user">
                {{ user.lastName }}
            </slot>
        </header>
    </div>
</template>
<div id="app">
    <navigation-link url="./test.txt">
        your profile
        <template v-slot:header="slotProps">
            {{ slotProps.user.firstName }}
        </template>
    </navigation-link>
</div>
<script>
    var vm = new Vue({
        el : '#app',
        components: {
            'navigation-link' : {
                template : '#t-slot',
                props: ['url'],
                data() {
                    return {
                        user : {
                            firstName : 'li',
                            lastName : 'qian'
                        }
                    }
                }
            }
        },
        data () {
            return {

            }
        }
    })
</script>
```

最后，插槽的缩写为v-slot:xx -> #xx。同样，插槽名和类名、属性名一样可以动态确定，如：v-slot:[diyName],diyName为父组件中的数据

7. 处理边界情况：

（1）访问根实例数据：this.$root

（2）访问父实例数据：this.$parent

（3）访问子组件实例/子元素：this.$refs.xx, 前提是需要在子组件中设置一个ref属性并为赋值为xx，这样父组件就可以通过this.$refs.xx来访问

（4）子组件访问任何深层级实例的数据/方式：在父组件中设置provide属性，和data一样，返回一个对象，对象里的属性就是提供给任何后代可以访问的属性/方法。在子代组件中，通过inject:['xx']来接收属性，类似props

8. 父子之间生命周期钩子执行顺序：
父beforeCreated-created-beforeMounted-(子beforeCreated到mounted)-mounted
父beforeDestroyed-(子beforeDestroyed-destroyed)-destroyed
子组件更新：父beforeUpdated-(子beforeUpdated-updated)-updated
父组件更新：父beforeUpdated-updated

### 动画/过渡

1. 过渡：

（1）需要过渡的标签都包含在transition标签中。过渡效果总共进入和离开阶段，每个又分为开始，过程，结束三个点。

（2）进入阶段：v-enter表示在元素被插入dom之前生效,一帧后移除，v-enter-active表示元素插入时生效，插入结束后移除，v-enter-to表示元素插入后的一帧生效，插入结束后移除。注释：v-enter表示用户想让元素显示在页面之前的状态，而v-enter-active则是持续一整个过程。举例opacity,v-enter应设置opacity为0，因为我们知道，之后元素会自动设置为opacity:1,而这一个过程用v-enter-active来监听属性的变化即transition:opacity 2s。因此这样样式表示从opacity:0 到opacity：1之间的动画过渡效果。同理leave也是如此

（3）离开阶段：v-leave在元素离开前触发生效，下一帧移除效果，v-leave-active在元素离开期间生效，离开后移除效果，v-leave-to在元素离开后触发生效，一帧后移除。一般v-enter-to和v-leave用的少。

（4）如果transition标签的name属性设置为xx,那么在设置style时，就可以设置为xx-enter、xx-leave；

（5）如果过渡中style同时存在transition和amination,并且想在某个事件内完成，可以```<transition :duration="1000"></transition>```表示总时间为1秒，或者```<transition :duration="{ enter: 500, leave: 800}"></transition>```;

（6）如果想在js中控制样式,则可以

```
<transition @enter="enter" @leave="leave"></transiiton>
js:
methods : {
    enter(el ,done) {
        //通过修改el的样式，done为回调函数
    }
}
```

（7）如果需要控制不同元素之间的过渡效果先后顺序，则可以通过修改mode属性来实现，in-out表示新元素先过渡进入，旧元素后过渡离开。out-in表示旧元素先过渡离开，新元素后过渡进入。

```
<transition naem="xxx" mode="in-out">
    <div v-if="" :key="hello">Hello</div> //一定要添加key,否则vue直接复用
    <div else :key="world">World</div>
</transiiton>
```

不同组件之间的过渡，可以使用动态组件

```
<transition naem="xxx" mode="in-out">
    <component :is="">
</transiiton>
```


（8）如果需要同时渲染整个列表，如v-for，transition是行不通的，transition只能渲染一个节点/在多个节点切换来渲染一个节点，而列表不需要切换，这就需要用到transiotn-group
```
<transition-group>
    <div v-for="(item, index) in items" :key="xx"></div>
</transition-group>
//等价于
<transition>
    <div></div>
</transition>
...
<transition>
    <div></div>
</transition>
```

（9）使用animate.css动画库，设置刷新页面时，第一次也有动画
```
<transition name="fade" mode=""
    appear
    enter-active-class="animate__animated animate__swing"
    leave-active-class="animate__animated animate__shakeX"
    appear-active-class="animate__animated animate__swing"
>
</transition>
//apear表示初始渲染过度，必须设置appear属性，然后再设置appear-active-class、appear-to-class、appear-class等。
```

## 可复用性和组合
### 混入
1.定义一个对象myMixin，该对象和创建vue实例一样，包含data,methods.生命周期函数等。在组件中，配置mixins:[],该数组接收混入对象的地址，如mixins:[myMixin],接着两者的数据和方法就会合并，如果存在冲突，则组件的数据和方法优先。注意：生命周期函数不会合并，而是先执行混入对象的周期函数
2.自定义选项合并策略：自定义合并规则，而不是简单的覆盖。方法如下：····

### 自定义指令

```
//全局指令
Vue.directive('test', {
    bind(el, binding, vnode) {

    },
    inserted(el, binding, vnode) {
    },
    updated(el, binding, vnode){
    }
})

<p v-test:[defineName]="200"></p>
```

每个自定义指令都有相应的钩子函数，表示在不同的时间段执行不同的函数，bind只调用一次，发生在指令第一次绑定到元素上时使用，可以进行初始化设置。inserted表示被绑定元素插入父节点时使用。updated在数据更新时使用。
每个函数参数为el,binding,vnode,el为dom元素，可以直接草足done，bind保存了和指令相关的数据，如binding.name = defineName。这里的defineName可以动态指令。通过binding.name可以获取指令名，而binding.value可以获取指令值，这里就是200。其它选项看文档。vnode为虚拟dom节点。

### 渲染函数render
通常创建一个组件可以通过模板来绑定到相应的组件上，render函数同样可以实现

```
 Vue.component('anchored-heading', {
    render : function() {
        return createElement(
            'h' + 1,
            {
                attrs : {
                    name: 'heading',
                }
            },
            [
                '随便一点文字',
                createElement('a', {
                    attrs: {
                        name: headingId,
                        href: '#' + headingId
                    }
                }, this.$slots.default)
            ]
        )
    }
});
```

在component中有一个属性render,render函数需要返回一个elemen,所以使用createElement方法，这个方法接收三个参数，第一个参数为标签名称（可以是变量/表达式/字符串），第二个可选参数为一个对象，对象里面为标签的属性配置（包括原生dom属性和vue指令、事件、属性），第三个可选参数为数组，每个数组选项都是一个文本虚拟节点或者createElement出来的虚拟节点。
注释：this.\$slots为插槽内容

### 插件
使用插件：Vue.use();
如：```var router = require('vue-router') Vue.use(router)```

### 过滤器
用处：{{ }} 或者v-bind表达式
用法：

```
    //全局过滤器
    Vue.filter('test', function(value, a, b) {
        return xxx;
    })
    Vue.filter('test2', function(value) {
        return xxx;
    })
    {{ testData | test( value2, value3 ) | test2 }}
```

testData数据将会被传到test函数中，经过处理然后将返回值传给test2过滤器，最后显示经过test2处理的返回值。

注意test1的参数有三个，testData传给了value, value2传给了a, value3传给b

调用时机：mounted完成之前，created之后



## 深入响应式
1.数据响应： 对于vue实例的data，只有初始化时的数据会得到响应式处理，而且如果data数据中存在对象/数组，当修改对象的数据时，并不会响应式处理。

实现响应式处理对象：Vue.set(实例数据的对象, ‘属性名称’, 属性值)；

实现响应式处理对象：Vue.set(数组名称, ‘下标’, 值)；删除某个元素vm.数组.splice()

2.this.$nextTick(function(){})：由于响应式是异步处理的，使用该方法后将会在DOM更新后立刻执行function(){},并且该方法返回一个promise实例对象；

## API

### this.$on 和 this.$emit
this.$on可以为同个事件名称添加多个处理函数,也可以为不同的事件绑定同一个处理函数，this.$emit将会调用所有同名事件的处理函数
```
this.$on(handleName, handleFn1)
this.$on(handleName, handleFn1) //同个事件名称添加多个处理函数

this.$on([handleName1, handleName2], handleFn1) //不同的事件绑定同一个处理函数
```

### Vue.observable
Vue.observable()接收一个对象，对象为数据，如const state = Vue.observable({ msg : '123' }),接着可以通过state.msg获取数据，并且这个state.msg数据是响应式的。Vue.observable类似Vuex的低配版

---
## 面试系列

### Vue的理解
1.vue的特性：

（1）数据驱动：vue采用的是MVVM架构，M表示模型层，处理业务逻辑和服务端的交互。V表示视图层，把数据模型通过UI的方式展示出来。VM表示视图模型层，是模型层和视图层之间的桥梁。vue负责vm这一层

（2）组件化：即将图形、非图形的业务逻辑抽象为统一的概念。这样做的好处是***降低系统耦合度***，如果业务需求发生变化，在接口不变的情况下，能够通过更换组件实现。***调试方便***，由于组件职责分明，不同的组件有不同的分工，通过排除法移除组件能够快速定位问题出现在哪里。***提高可维护性***，组件是复用的，通过修改组件，整体得到优化升级。

（3）指令系统：可以直接在dom元素添加对应的指令，当指令绑定的表达式发生变化时dom元素发生相应的变化。无需直接操作dom.

2.与传统开发对比：页面的视图变化和事件触发是否需要直接操作dom的区别

3.与React的区别：

    相同：

        （1）都是组件化

        （2）都是数据驱动视图

        （3）都是虚拟DOM

        （4）都支持服务端渲染

        （5）都有自己的构建工具

    不同：
        （1）数据流向不同：react为单向数据流，vue为双向数据流

        （2）数据变化的原理不同：react使用的是可变数据，vue使用的是不可变数据

        （3）组件通信不同：react通过回调函数实现通信，vue子组件向父组件通信采用事件和回调函数

        （4）diff算法不同：react将需要更新的dom保存在diff队列，得到patch树再统一更新DOM。而vue使用双向指针，边对比边更新DOM。

### SPA的理解（单页与多页）
1.spa: 是一种单页网络应用程序的模型，能够实现在页面不重新加载的情况下，动态重写页面或者动态装载资源添加到页面，从而实现页面与用户的交互。

例子：spa页面就是一个杯子，能够在不更换杯子的情况下把里面的牛奶换成橙汁。

2.与mpa的区别：
map切换页面需要重新加载html,css,js文件。spa不需要重新加载，并且实现局部页面刷新（map：整页刷新），url模式采取hash模式（map:history模式），数据传递较为简单（map:cookie等），页面切换速度快，维护成本低，但是seo搜索优化较为困难，首屏加载速度慢。

3.如何给spa做seo优化：

        （1）SSR服务端渲染：将组件或者页面通过服务端生成html，返回给客户端

        （2）静态化：将外部请求的静态页面地址转为动态页面地址

        （3）phantomjs爬虫处理：通过Nginx的user-agent判读是否为爬虫，如果是就通过nodejs调用phantomjs返回完整页面信息给爬虫，不是就展示页面。

4.如何解决首屏加载慢的问题：

### Vue的生命周期
1.概念：vue实例从创建到销毁的过程，就是创建、初始化数据、编译模板，挂载dom->渲染、更新->渲染、销毁的一系列过程。这一过程中生命周期钩子this自动绑定vue上下文，因此不能使用箭头函数定义钩子方法。

2.具体：4类八个阶段，分别是beforeCreated组件实例创建之前，create组件实例数据初始化，beforeMounted组件挂载之前，mounted组件实例挂在到页面，beforedUpdated组件数据变化，视图更新之前，updated组件数据变化，视图更新完毕，beforeDestroyed组件销毁之前，destroyed组件实例销毁之后

3.具体场景：beforedCreated常用于插件初始化，created异步数据获取，mounted访问和操作dom元素，beforeUpdated获取更新前的状态，updated获取更新后的状态，beforeDestroyed解除定时器等绑定

4.数据请求在created和mounted的区别：触发时机：created在组件实例创建完成之后立刻调用，页面还没生成dom,mounted则是在页面渲染dom之后立刻调用，created的时机比mounted要早。相同：他们都可以访问组件实例的属性和方法。区别：如果在mounted请求异步数据，由于此时dom结构已经生成，可能会导致页面出现闪动，但如果在页面加载完成之前数据请求已经完毕则不会出现。如果是需要操作dom,可以在mounted中请求。

### V-if和v-for为什么不建议放一起使用
答：这涉及到两者的优先级的问题，v-for的优先级比v-if高，当同时用在同一个标签的时候，v-if将会被内置到v-for之中，也就是说进行循环再进行判断，如果要先进行v-if条件渲染，可以将v-if放在父级容器当中。而且v-if和v-for放一起会造成性能上的浪费。

### 双向数据绑定原理
基本概念：利用Object.defineProperty()和闭包机制
```
    var obj = {
        //...
        }
    Object.keys(obj).forEach(( key )=>{ //循环遍历对象属性
        let oldValue = obj[key] //闭包机制，开辟内存空间单独存此变量
        Object.defineProperty(obj, key, {
            get() {
                return oldValue
            },
            set(newValue) {
                if(newValue !== oldValue) {
                    oldValue = newValue
                    //执行重新渲染函数
                }
            }
        })
    })
```

### 首屏加载缓慢
1.首屏加载时间：用户输入地址到页面首屏内容渲染完成的过程所需的时间

2.如何获得加载时间：

方式1：document.addEventListener('DOMContentLoaded',(event)=>{})；

方式2：performance.getEntriesByName('first-contentful-paint')[0].startTime

3.加载缓慢的原因：

（1）网络延迟；

（2）加载文件体积过大；

（3）重复请求相同的加载文件；

（4）加载文件时，渲染内容被阻塞

4.解决方案：

（1）减少入口文件体积：将不同的路由分成不同的组件，并且通过```component:()=>import()```的方式动态加载路由组件

（2）静态资源缓存：利用本地缓存如http请求头设置last-modified、etag等字段，适当利用localStorage

（3）图片资源合并：将字体图片或者小图片合成一张图，使用雪碧图

（4）UI框架按需加载：如element-ui按需要的组件引入

（5）使用Gzip压缩：安装compression-webpack-plugin插件并配置

（6）组件重复打包：对多个组件引用同样的js文件，在webpack配置文件中修改CommonsChunkPlugin

（7）使用SSR


### 为什么data是一个函数而不是对象
在创建组件实例的时候，data就相当于构造函数的原型对象的一个属性，如果data设置为一个对象，那么所有组件实例将共享这个对象，会造成数据污染，而使用函数返回对象，则会为每个组件实例创建单独的对象。
因此，根组件实例data可以使用对象，而组件实例的data必须使用函数

### 给对象新增属性但视图不更新
原因：vu2中后面新增的属性不是响应式，即没有通过Object.defineProperty()设置。
解决办法：

    （1）Vue.set(target, key, value)  //适合少量属性更改

    （2）this.someObject = Object.assign({}, this.someObject, {newProperty:1...}) //适合大量属性更改

### 插件和组件的区别

1，编写方式：组件可以使用一个.vue文件或者组件注册中的template属性编写，而插件通过通过暴露一个组件.install=函数（Vue,options）编写相关配置项

2.组册方式：组件可以使用Vue.component()进行全局组件组册，也可以直接在组件内部通过components属性进行局部组件组册，而插件只能通过Vue.use（）的方式组册组件

3.使用场景：组件用于构成业务场景模块，而插件用于增强vue

### 组件之间的通信方式

1.父子之间：父组件通过在子组件标签上自定义属性传递数据，子组件通过props属性接收父组件传递过来的数据。父组件通过监听子组件标签上的自定义事件，当该事件触发时同时触发父组件事件，子组件通过$emit触发自定义事件并向函数传递数据。父组件通过给子组件标签添加ref属性，从而父组件通过this.$refs获取自组件实例。

2.兄弟之间：通过$eventBus，即定义一个事件总线类并实例化到Vue实例的原型链上，兄弟之间即可使用监听和调用。或者通过this.$parent.on() ，this.$parent.emit()/this.$root的方式传递数据
```
class Bus {
    constructor() {
        this.callbacks = {}
    }
    $on(name, fn) {
        this.callbacks[name] = this.callback[name] || []
        this.callbacks[name].push(fn)
    }

    $emit(name, args) {
        if(this.callbacks[name]) {
            this.callbacks[name].forEach(cb=>cb(args))
        }
    }
}

Vue.prototype.$bus = new Bus()
//或者
Vue.prototype.$bus = new Vue()

//其它组件上.vue
this.$bus.$on('xxx',fn)
//.vue
this.$bux.$emit('xx')
```

3.祖孙之间：祖组件定义provide函数返回数据对象，孙组件通过inject像props那样接收数据

4.复杂之间通过vuex共享数据

### 双向数据绑定原理

双向数据绑定：数据更新时视图更新，视图更新时数据也同步更新

双向数据绑定的流程：首先通过new Vue()初始化，然后通过监听器Observer,对data中数据进行响应式处理，同时使用Complier解析器对模板进行编译，找到动态绑定的数据，从data中取出数据用于初始化视图。紧接着使用Watcher和更新函数，当数据变化时，Watcher会调用更新函数更新视图。由于data中key可能会在多个地方使用，所以每个key都用一个管家dep来管理，一旦data发生变化立刻找到对应的dep，通知Watcher调用对应的更新函数

原理：数据劫持+订阅发布者模式


### nextTick

概念：vue更新DOM时是异步执行的，也就是说，当数据发生变化时，vue会开启一个异步更新队列，视图只有等队列里面的所有数据变化完成时，才统一更新视图

作用：是vue的一种优化策略，当如果没有异步更新机制，数据在短暂的时间连续变化，，视图也会不断更新，而我们只需要最后的执行结果，导致性能浪费。

应用场景：当数据变化完成并且视图更新后，想立刻获取更新后的DOM结构。```this.$nextTick(()=>{ this.$el })```

### minix

概念：混入，是一个对象，相当于组件的配置项，有data,methods,生命周期函数等

应用场景：当多个组件之间的功能相同或者代码相似时，将代码抽离出来，再以混入的形式使用。需要注意的是如果混入中的选项和组件中的选项冲突时，选择组件中的选项，而生命周期函数不会冲突，先执行minix的生命周期函数

### slot

概念：插槽，在组件模板中对外暴露出口，等待父组件进行填充

应用场景：拓展组件，对组件做细致化处理。比如不同的父组件，希望在子组件中做细微不同的处理，插槽就能实现不同的处理

分类：默认插槽、具名插槽、作用域插槽（为slot指定属性，实现子组件向父组件传递数据）

### Vue.observable

概念：能够将一个对象变成响应式数据，返回的对象可以直接在函数或者计算属性中使用。vue2中返回的对象和原对象是同个对象，而vue3返回的是一个响应代理，直接修改原对象无法响应

应用场景：非父子组件之间简单的通信，不想使用vuex或者bus通信时使用。将需要通信的数据封装在一个js中，模仿vuex的mutation之类的方法。

### key

概念：为vnode添加唯一的id,属于diff算法的优化策略，方便vue更快更准确的找到对应vnode

应用场景：v-for和强制更新值为new Date()的属性

区别：不使用key,vue将采取就地复用的策略，当数据发生变化时，将最大程度匹配/复用同类型的元素。而使用key之后，如果vue监测到数据发生变化，会将key中不再出现的vnode移除。

不使用key的diff:会从前往后一一匹配，如果数据不同/类型不同直接操作dom，最后作插入
使用key的diff:先从前往后一一匹配，如果不同，则再从后往前匹配，直至找到不同的节点，作插入处理

### 缓存组件/keep-alive

概念：keep-alive用于缓存一个不活动的组件实例，通常用在动态组件/router-view当中，由于缓存了组建实例，避免了重复渲染。

用法：keep-alive有include属性，exclude属性，分别表示匹配的组件名称和不匹配的组件名称，值可以是字符串/字符串数组/正则，但是数组和正则需要v-bind绑定。max-age表示能够缓存的组件最大数量。通常在router的路由配置中为一个路由设置meta属性，根据meta属性里的某些值和v-if搭配使用决定一个组件出现，从而决定是否缓存

应用场景：组件切换/视图返回

缓存后获得数据/更新组件的途径：在路由守卫beforeRouteEnter的next的回调函数中或者在actived钩子中获取数据。

注意：使用缓存后，新增了actived和deactivated钩子。并且除了这两个钩子外，重新访问缓存组件不会再次调用组件的生命周期钩子，而是用actived钩子。

### vue中的修饰符

概念：vue的修饰符帮助处理了许多dom细节

分类：

（1）表单修饰符：lazy、number、trim。用在v-model中

        .lazy:在输入框完成输入完成并且失去焦点之后才更新value

        .number:能够将用户输入的数据自动用parseInt()转为数字，无法处理则返回原始数据

        .trim：去除输入的首空格

（2）事件修饰符：stop、prevent、self、once、passive、native

        .stop:阻止事件冒泡

        .prevent:阻止事件的默认行为

        .self:只触发自身的事件,不触发传递过来的事件

        .once:事件只触发一次

        .passive:用在滚动事件中，等滚动完成才触发事件

        .native:组件监听事件的函数为js的原生函数，而不是methods中的自定义事件

（3）鼠标修饰符：left、right、middle。修饰click事件

（4）键盘修饰符：修饰keyup、keydown事件，有enter、keycode等修饰符。

        其中keycode可以自定义：Vue.config.KeyCodes.xx = xx

（5）属性修饰符：props、camel

        props:能够对一个属性进行隐藏，不暴露在html结构汇总

        camel：将命名转为驼峰命名


### 自定义指令

概念：像v-开头的都是指令，vue提供了自定义指令的方法；

        全局组册：Vue.directive('指令名',{

        })

        局部组册：diretives:{
            '指令名':{
                bind(el, binding, vnode, oldVnode){
                    //bind钩子用于数据初始化
                    //el唯一可修改对象，可以直接操作原生dom
                    //binding为指令的对象里面包含了指令值之类的数据
                    { name, value, oldValue, expressiong, arg, modifiers } = binding;//解析构值
                },
                inserted(){
                    //inserted钩子表示插入父节点时调用，但是此时只能鞥保证父节点存在，不能保证已经插入父节点中
                },
                updated(){
                    //vnode更新时调用，无法保证值是否更新完成，子组件也有可能还没更新完成
                },
                componentUpdated(){
                    //保证组件及其子组件内的值更新完成之后再调用
                },
                unBind(){
                    //解绑时调用
                }
            }
        }

应用场景：图片懒加载、阻止表单重复提交

### 过滤器

    filter：在不改变原始数据的情况下对数据进行加工处理，再调用处理。Vue3中已废弃

    应用场景：单位转换、时间格式化、文本格式化、数字打点

### 虚拟DOM

概念：把真实dom的抽象抽象出来，用JS对象作为基础节点构成的虚拟DOM树对应真实DOM树，其中用对象的属性来描述节点，每个对象至少包含标签名、属性、子元素对象，最终通过一系列操作映射到真实环境中。

出现的原因：真实DOM节点里有非常多属性，如果直接批量操作DOM，一个for循环里每次创建删除元素节点浏览器都会重新构建DOM树，将会非常消耗性能，造成页面卡顿。虚拟DOM最终创建成真实DOM也要经历构建DOM树，差别在于虚拟DOM能够配合Diff算法，能够实现在批量操作DOM时，只构建一次DOM树。

实现：代码略

### diff算法

概念：diff算法是一种同层树节点比较算法。

特点：深度优先，同层比较，两边向中间靠

比较过程：同层新旧虚拟DOM树节点比较，同层节点可以想象成一支球队，上赛季的球队人员站成一排，本赛季的球队队员站成一排。两个排最左边有个牌子，最右边也有一个牌子。从旧队最左边的牌子和新队左边和右边的拿着牌子的队员比较，看是否同一个人，如果不是，旧队最右边的牌子和新队左边和右边的拿着牌子的队员比较，看是否为同一个人。如果是相同的人，看看本赛季该队员的位置是否和上赛季位置一样，如果不一样则移动，然后牌子比对成功的新旧赛季队员将牌子递给下一个人，重复以上过程。当匹配不成功时，没有key新队员直接插入对应位置。否则按照以下过程执行。


如果新旧子节点都存在key，那么会根据oldChild的key生成一张hash表，用S的key与hash表做匹配，匹配成功就判断S和匹配节点是否为sameNode，如果是，就在真实dom中将成功的节点移到最前面，否则，将S生成对应的节点插入到dom中对应的oldS位置，S指针向中间移动，被匹配old中的节点置为null。

如果没有key,则直接将S生成新的节点插入真实DOM（ps：这下可以解释为什么v-for的时候需要设置key了，如果没有key那么就只会做四种匹配，就算指针中间有可复用的节点都不能被复用了）


Vnode:新节点 oldNode：旧节点

完整过程：

首先数据发生变化时会通过set方法调用Dep.notify通知所有订阅者Watcher，Watcher会调用patch函数，patch函数里传入新旧虚拟节点，首先比较两个节点是否相同（key,标签名，文本，注释节点，属性），如果不相同则获取旧节点的真实dom,以及它的父节点，将新节点真实化并插入旧节点真实DOM的父节点之中，旧节点真实DOM之前并删除旧节点dom。如果相同，则会比较新旧节点是否为同一个对象，如果是同一个对象就会直接返回，如果不是同一个对象就会判断他们的子节点是否相同。如果旧节点没有子节点，那么直接真实化DOM新节点的子节点并插入，如果新节点没有子节点，则直接删除旧节点的子节点和对应DOM,如果新旧节点都有子节点则开始比较。
链接：<https://juejin.cn/post/6844903607913938951>


### axios的封装

概念：

1.配置config.js文件，内容为根据不同的环境（process.env.NODE_ENV === 'pro or dev'）设置baseURL及其其它基础路径。并将其放在一个对象中暴露出来

2.配置service.js文件，内容为引入axios并创建axios对象（axios.create()），传入配置信息如请求头信息、请求时间、跨域设置。接着调用axios对象的interceptor.request.use()和interceptors.response.use()方法设置请求拦截器（token验证）和请求响应器，最后将这个对象暴露出其


### vue项目划分结构和划分组件

原则：

1.  文件夹和内部文件语义一致，即路由模块文件夹应该只包含路由模块

2.  单一出口/入口，如每个模块应该有一个index.js文件作为模块下其他文件统一对外暴露的出口

3.  就近原则，紧耦合文件放在一起并以相对路径引用

4.  公共文件以绝对路径的方式引用

5.  /src以外的文件不该被引入

单页面目录结构：
project
│  .browserslistrc
│  .env.production
│  .eslintrc.js
│  .gitignore
│  babel.config.js
│  package-lock.json
│  package.json
│  README.md
│  vue.config.js
│  yarn-error.log
│  yarn.lock
│
├─public
│      favicon.ico
│      index.html
│
|-- src
    |-- components
        |-- input
            |-- index.js
            |-- index.module.scss
    |-- pages
        |-- seller
            |-- components
                |-- input
                    |-- index.js
                    |-- index.module.scss
            |-- reducer.js
            |-- saga.js
            |-- index.js
            |-- index.module.scss
        |-- buyer
            |-- index.js
        |-- index.js

多页面目录结构：
```
my-vue-test:.
│  .browserslistrc
│  .env.production
│  .eslintrc.js
│  .gitignore
│  babel.config.js
│  package-lock.json
│  package.json
│  README.md
│  vue.config.js
│  yarn-error.log
│  yarn.lock
│
├─public
│      favicon.ico
│      index.html
│
└─src
    ├─apis //接口文件根据页面或实例模块化
    │      index.js
    │      login.js
    │
    ├─components //全局公共组件
    │  └─header
    │          index.less
    │          index.vue
    │
    ├─config //配置（环境变量配置不同passid等）
    │      env.js
    │      index.js
    │
    ├─contant //常量
    │      index.js
    │
    ├─images //图片
    │      logo.png
    │
    ├─pages //多页面vue项目，不同的实例
    │  ├─index //主实例
    │  │  │  index.js
    │  │  │  index.vue
    │  │  │  main.js
    │  │  │  router.js
    │  │  │  store.js
    │  │  │
    │  │  ├─components //业务组件
    │  │  └─pages //此实例中的各个路由
    │  │      ├─amenu
    │  │      │      index.vue
    │  │      │
    │  │      └─bmenu
    │  │              index.vue
    │  │
    │  └─login //另一个实例
    │          index.js
    │          index.vue
    │          main.js
    │
    ├─scripts //包含各种常用配置，工具函数
    │  │  map.js
    │  │
    │  └─utils
    │          helper.js
    │
    ├─store //vuex仓库
    │  │  index.js
    │  │
    │  ├─index
    │  │      actions.js
    │  │      getters.js
    │  │      index.js
    │  │      mutation-types.js
    │  │      mutations.js
    │  │      state.js
    │  │
    │  └─user
    │          actions.js
    │          getters.js
    │          index.js
    │          mutation-types.js
    │          mutations.js
    │          state.js
    │
    └─styles //样式统一配置
        │  components.less
        │
        ├─animation
        │      index.less
        │      slide.less
        │
        ├─base
        │      index.less
        │      style.less
        │      var.less
        │      widget.less
        │
        └─common
                index.less
                reset.less
                style.less
                transition.less
```

### 权限管理以及按钮级别的权限管理实现

接口权限：利用axios请求拦截在给请求头添加上token,然后再axios响应拦截上根据后端返回的状态码决定路由跳转

路由权限：初始化时挂载全部路由，每次路由跳转前利用路由的meta字段做权限判断

菜单权限：

按钮权限：

（1）用v-if判断，即获取用户的权限和路由表的权限，如果存在则显示

（2）通过自定义指令判断，首先路由配置里meta配置了按钮权限角色，然后定义一个全局指令has，在bind钩子里获取路由表里的权限然后跟用户权限比较，如果没通过则通过则删除该el实例

### vue中跨域

跨域的概念： 浏览器有一个同源策略，在一个网站中规定只有协议、主机、端口相同的称之为同源，非同源请求称之为跨域。浏览器会限制跨域请求。

解决方案：JSONP、CORS、Proxy

CORS: Cross-origin resource sharing跨域资源共享，指的是后端在响应头中将Access-Control-Allow-Origin字段值设置为目标服务器主机，这样在跨域请求时，服务器返回的响应携带这一字段，浏览器识别后才会对其作出处理。

Proxy: 代理，即将请求转到中间服务器，通过中间服务器转发请求到目标服务器，将得到的结果转发到前端。

1.  vue-cli脚手架搭建的项目，在vue.config.js中设置如下代码，这种做法必须设置中间服务器和应用程序在同一个主机上否则仍然会产生跨域
```
devServer: {
    host: '0.0.0.0', //中间服务器主机
    port: 8080, //端口
    open: true, //项目启动自动打开网站
    proxy : {
        '/api': {
            target: 'xxx', //目标服务器地址
            changeOrigin: true, //是否跨域
            pathRewrite : {
                '^/api' : 'xx' //将访问/api 改写为 /xx
            }
        }
    }
}
```
2.建议使用nginx代理

### 从本地部署到服务器提示404的原因

1.如何部署： 通过在终端输入将打包文件传输到服务器的静态资源文件夹中，再配合nginx代理即可
```
//user为主机登录用户 host为主机外网ip
scp. dist.zip user@host:xxx/xxx/xxx
```

2.history模式下出现问题：vue为单页应用，是一种网站的模型，所有用户交互通过动态重写页面来实现。由于nginx代理的配置，在访问www.website.com时，会代理访问服务器下/data/dist/index.html文件，但是vue的路由会自动跳转到www.website.com/login登录页面并执行刷新操作，nginx如果没配置是无法访问的

3.hash模式下没问题：因为hash模式下，hash值为#/login，这些hash内容不包含在请求当中，因此不会重新加载页面

4.解决方案：如果是history模式，只需要将任意文件的访问重定向到index.html即可，将路由交给前端处理。具体做法是在nginx的配置文件中配置```try_files $uir $uri/ /index.html```,同时在vue路由跳转中设置*路由，当访问不存在的路由时显示404组件

### 如何处理vue项目中的错误

1.后端接口错误：通过axios响应拦截器处理后端返回的错误

2.代码逻辑错误：设置全局错误处理函数，
```
vue.config.errorHandler = function (err, vm, info) {

}
```
vue2.5新增了errorCaptured钩子用于捕获子孙组件的错误
```
errorCaptured(err, vm, info) {
    return boolean //是否将错误继续向上传递给父组件或者全局处理函数
}
```

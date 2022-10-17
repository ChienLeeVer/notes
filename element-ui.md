#Element-UI

### 引入方式
手动引入：

全局引入:在main.vue中
```
    npm install element-ui -D
```
```
import element from 'element-ui'
import 'elemen-ui/lib/theme-chalk/index.css'
Vue.use(element)
```
按需引入:
```
npm install babel-plugin-component
```
babel.config.js中
```
plugins:[
    ['component',{
        'libraryName' : 'element-ui',
        'styleLibraryName' : 'theme-chalk'
    }]
]
```
main.js中
```
import {Button} from 'element-ui'
Vue.component(Button.name, Button)
```

自动引入：
```
vue add element
```

### 表单校验
```
<template>
    <div id="app">
        <el-form inline :model = 'data' :rules='rules' ref="form"> // :rules为表单添加校验规则  ref表示可以从$refs中获取该组件实例
            <el-form-item label="审批人" prop="user"> //prop属性锁定哪个规则，对应rules对象中的属性
                <el-input v-model="data.user" placeholder="审批人"></el-input>
            </el-form-item>
            <el-form-item label="活动区域">
                <el-select v-model="data.region" placeholder="活动区域">
                    <el-option label="区域一" value="上海"></el-option>
                    <el-option label="区域二" value="北京"></el-option>
                </el-select>
            </el-form-item>
            <el-form-item>
                <el-button type="primary" @click="onSubmit">查询</el-button>
            </el-form-item>
        </el-form>
    </div>
</template>

<script>
export default {
    name:"app",
    data() {
        const userValidator = (rule, value, callback) => {
            if(value.length > 3) {
                callback()
            }else {
                callback(new Error('用户名长度必须大于3'))
            }
        }


        return {
            data: {
                user: '',
                region: ''
            },
            rules: {
                user:[
                    {required: true, trigger: 'change', message: '用户名必须录入'}, //添加规则，message表示消息
                    {validator: userValidator, trigger: 'change'} //添加校验函数，trigger表示校验触发的事件
                ]
            }
        }
    },
    methods: {
        /* eslint-disable */
        onSubmit(){
            console.log(this.data)
            this.$refs.form.validate((isValid, errors)=>{   //每个表单组件实例都会有一个validate方法，表示触发当前表单下所有校验规则，
                                                            //该方法接收一个回调函数，函数参数表示是否通过，出错的位置
                console.log(isValid, errors);
            })
        }
    }
}
</script>
```
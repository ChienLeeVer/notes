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
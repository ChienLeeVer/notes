# ES6

标签（空格分隔）： es6

---

### 数组新增扩展
1.扩展运算符：```...```，能够将数组拆分为序列，经常用在数组复制，将具有iterator接口对象/类数组转为真数组，解构赋值上，需要注意的是数组复制只是简单的浅拷贝。用在解构赋值上只能放在最后一个。

2.数组构造函数：Array.from()；和Array.of()；
Array.from()能够将类数组/具有iterator接口对象/setmap对象转为真数组；
Array.of()能够将一组数值转为数组；

3.数组的方法：
copyWith(开始覆盖位置，[读取数据开始位置]，[读取数据结束位置])
find(callback)找到符合回调函数return值的数；
findIndex(callback)找到符合回调函数return值的数的索引
includes(value)判断数组中是否存在某个数值
fill(param)用param填充数组
entries(),keys(),values()分别返回数组/对象的键值对，键名，键值
flat(层数)数组扁平化，默认1层
flatMap(callback)对每项数组元素map，再对返回的数组执行flat()

4.数组空位默认undefined

5.sort排序方法


### 函数新增扩展

1.参数：参数可以设置默认值，并且可以使用解析构值；

2.属性：f.length返回设置了默认值的参数之前的参数个数 ，f.name返回具名函数的名字

3.作用域：参数设置默认值，如value = 2 等同于 let value = 2

4.严格模式：函数使用默认参数，解析构值，剩余运算符，函数内部不能使用严格模式

5.箭头函数：本身没有this，this指向外部定义对象，不能当作构造函数，不能使用arguments,不能使用yield命令


### set和map
set是一种叫做集合的数据结构。map是一种叫做字典的数据结构

集合是一堆无序、相关联且不重复的内存结构的组合。字典是元素的组合，每个元素都有一个称之为key的域，每个元素的key互不相同。

区别：都存储不重复的值。不同在于set是以[值，值]的形式存储，而map是以[键，值]的形式存储（能以对象作为键名）

set的方法有：add、delete、has、clear 属性为size

map的方法有：set、delete、has、clear 属性为size

### proxy

概念：对象的代理，实现对象的基本操作的拦截和自定义事件

使用：

        new Proxy(对象/对象数组, {
            get(target, propName, proxy){
                //能够拦截对目标对象的某个属性的读取，执行某些操作
                //target为目标对象，propName为属性名，proxy为proxy对象实例
                //Reflext能读取对象的默认行为，如Reflect.get(对象，属性)就能获取对象属性值
                return Reflect.get(target, propName)
            },
            set( target, propName, newvalue, proxy ){

            },
            ...
        })

### modules

概念：modules即模块，能够独立完成某个功能和单独命名的数据结构和程序代码的集合

使用：ES6的模块通过export命令对外暴露接口，允许外部文件通过import命令的方式使用暴露的变量，一般需要为引入的变量命名，如果不需要命名，则需要export default，一般情况下引入的变量不能修改，除非是对象则可以修改它的属性。import命令会提升到顶部，并且支持动态引入模块。

比较：AMD、CMD、CommonJS、ESM

<https://juejin.cn/post/6844903955282165773>

### Generator

概念：在不改变原有类的属性和方法的情况下，用于动态扩展类属性和类方法的函数


用法：
```
//类的装饰，函数接受一个类
function action(target) {
    target.xx = xx
}

@action
class Person {}

//类的属性装饰
function readOnly(target, name, descriptor) { //target为对象，name为属性名，descriptor为属性配置
    descriptor.writable = false
    return descriptor
}
class Person {
    @readOnly
    name() { } //实现了对Person.name只可读的限制
}
```



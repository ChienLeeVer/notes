# ES6

标签（空格分隔）： es6

---
### let 和 const命令
1.  let：
    1.  不存在变量提升：只有声明之后才能用
    2.  暂时性死区：只要块级作用域内存在let命令，其所声明的变量绑定在这个区域，不受外部影响。特殊：函数参数设置默认值后，该参数表示为let声明
    3.  不允许重复声明：相同的作用域内，不允许重复声明同一个变量
    4.  块级作用域
2.  const:
    1.  const表示常量，一旦赋值不可改变（指向的内存地址不可改变）。声明即要初始化。
    2.  其它特点和let命令一样
3.  顶层对象：let和const、class命令声明的变量不属于顶层对象，var和function依旧是顶层对象,ES2020可以通过globalThis获取任何环境下的顶层对象

### 变量的解构赋值
1.  解构赋值：ES6允许以一定的模式从对象/数组中取值，数组的解构赋值按位置一一对应取值，否则取undefine,例如let [a,b,c] = [1,2,3]，等效于a=1,b=2,c=3。赋值表达式的右边要求是一个具有Iterator接口的数据结构，即可以遍历的数据结构，比如数组/对象，如果是基础类型数据会尝试装箱转为对象/数组。（undefined和null不可转）
2. 默认值：和函数 参数一样，解构赋值中的变量允许默认值，写法一样 let [a,b,c=='3'] = [1,2],等效于a=1,b=2,c=3。需要注意的是，当表达式右边数组中的值为undefined时，ES6认为这个值不存在，解构赋值会尝试使用默认值。
3.  对象的解构赋值:变量名和对象的属性名一一对应取值。例如let {name,age} = {name:'test', age:25},如果想为变量重新另起名字，可以使用冒号let {name: anotherName, age} = {name:'test', age:25}，此时name只是形式上存在，不能直接使用name。
4.  对已声明的变量解构赋值,必须使用圆括号扩起来：如 let x; （ {x} = {x : 123}）,因为x已经声明，加上圆括号，这样做的目的在于防止JS将{}识别为代码块而不是解构赋值。
5.  函数参数也可使用解构赋值


### 字符串新增的方法
    1. 查找字符串：includes(str:String,[startIndex]), startsWith(str:String, [startIndex]), endsWith(str, [startIndex])
    2. 重复字符串：repeat(number:Number)
    3. 填充字符串：padStart(maxStrLength:Number, padStr:String), padEnd(maxStrLength:Number, padStr:String),该方法一般用在数值补全指定位数
    4. 消除空格：trimStart(), trimEnd()
    5. 匹配与替换字符串：matchAll(), replaceAll(被替换字符串, 替换字符串/函数返回值)等价于g模式正则下的replace(）,不过replaceAll使用正则也必须用上g模式，第二个参数可以是$`,$',$&, $n

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

### 数值的扩展
    1.  二进制和八进制的表示方法：0b 、 0o, 转十进制Number(),转其他进制toString()
    2. isNaN(number: Any) => Number.isNaN(number:Number)仅接受数值类型的参数
    3. Number.isInteger(): 判断是否是一个整数, 此方法并非完全奏效，精度超过限度时判断不准确
    4. BigInt: 123456789n, 可以表达任意位数的整数，是一种特殊的值，不等价于普通整数

### 函数新增扩展

1.参数：参数可以设置默认值，并且可以使用解析构值，通唱此类参数放在函数的尾参数位置；

2.属性：f.length返回形参个数 ，但不返回设置了默认值的参数和剩余参数及其它们之后的参数，f.name返回具名函数的名字

3.作用域：参数设置默认值，如value = 2 等同于 let value = 2，并且设置了默认值的参数括号内会暂时形成一个作用域，只能访问外部作用域，不能访问函数内部。如果参数默认值设置为一个函数，那么这个函数的作用域在定义时确定，即只能访问临时形成的作用域或者外部作用域（这个情况也仅限于这个函数使用某个变量，但这个函数未定义，临时作用域也没定义才会访问外部作用域）

4.严格模式：函数使用默认参数，解析构值，剩余运算符，函数内部不能使用严格模式

5.箭头函数：本身没有this和argument（this指向定义时上层作用域中的this），不能当作构造函数，不能使用yield命令。 不适合的场景：动态this以及对象内部定义方法

6.尾递归：将所有要用到的内部变量改写成函数的参数

### 数组的新增
    1.  克隆数组： 浅拷贝[...arr] / Array.from(arr) / [].slice.call(arrayLiek)
    2.  任何定义了遍历器接口的对象都可以用扩展运算符(三个点...)转为真正的数组
        1.  ``` Number.prototype[Symbol.iterator] = function *() { let i = 0; let num = this.valueOf(); while (i < num) { yield i++; } }``` 具体含义是，generator函数生成了以恶个遍历器对象，内部值为1， 2，3，4，5，而在Number原型上添加该函数后，任何数字都可以使用扩展运算符，并且其返回值为遍历器对象的所有值
    3.  Array.from(arrayLike, [func], [thisOfFunc]);Array.from能将一个具有length属性的对象转为数组，func为加工函数，其返回值可以作为数组的值，thiOfFunc可以指定func的this
    4.  数组扁平化：arr.toString().split(',').map(n=>+n) / arr.flat(Infinity) 均不会对原数组产生影响,

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



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
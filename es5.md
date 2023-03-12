### 数据类型

1.  基本数据类型：Number、String、Boolean、Object、特殊值Undefined、null

2.  检测数据类型的方法：typeof、instanceof、Object.prototype.toString()
    1. typeof: 返回值类型为字符串，‘number’、‘string’、‘boolean’、‘undefined’、‘function’、‘object’。注意：[]和null识别为object。因为数组是一种特殊的对象，而null在设计之初归类于object特殊值，js底层代码设定typeof null返回值为‘null’.
    2. instanceof
    3. Object.prototype.toString()

### null、undeifned和布尔值

1.  null表示空值、undefined表示未定义
2.  布尔值在前置逻辑运算符！、相等运算符==/!==、比较运算符>=，会返回布尔值。如果某个位置应当是布尔值，比如if判断句，JS会将undefined、null、0、‘’、false、NaN转为false，其他都转为true。

### 数值

1， 前提：JS内部所有数字都是以64位（8个字节）浮点数形式存储，意味着整数实质也是小数，需要使用整数时转为32位整数。（符号1位 + 指数部分11位 + 小数部分52位），指数决定大小、小数决定精度，计算结果 = (-1)^符号位 * 1.xx...xx * 2^指数部分。

2. 精度问题：二进制2^53以内可以保持精度，多出来的一位是JS规定的1.xx...xx的第一个1， 2^53 = 9007199254740992 ,计算结果这个数字以内的计算都不会出现计算错误。

3. 范围问题：2^11 = 2048, 0需要一位，因此指数部分表示数值大小的只有2047位，留出一半给负数，因此最大范围为2^1024~2^-1023,超出这个范围无法表示。超出2^1024返回Infinity，小于2^-1023返回0.

4.  表示方法：十六进制0x开头，八进制0o开头/0开头且只有0-7数字，二进制0b开头。

5.  特殊数字：
    1.  -0 === +0 === 0 ，内部比较不比较符号位。只有 （1 / +0） !== （1 / -0）转化为+Infinity和-Infinity
    2.  NaN : 将非数字的字符串解析为数字出错时的返回值，NaN不等于自身，转为布尔值为false,并且在任何比较运算或相等运算中出现NaN，判断结果均为false。在加减乘除中均返回NaN。

6.  parseInt(param)： 将参数转为字符串，如果一个字符串前面有空格会自动清除，如果遇到第一个非空格字符不是数字字符，则直接返回NaN，否则将字符串中的数字字符转为数字，直至遇到非数字字符，将已经转过的数字返回。注意：parseInt的第二个参数表示进制，代表第一个参数是几进制范围为2-36，通常第二个参数不写也没关系，parseInt()会自动识别第一个参数的进制，并将最终结果转换为十进制输出。如果故意输入0/null/undefined，parseInt()会忽略第二个参数。如果出现指定几进制，但是第一个参数出现了不该出现的数字，此时从最高位开始转，直至遇到非法数字，只返回可以转换的数值，否则返回NaN

7.  isNaN(): 用于判断一个值是否为NaN，返回true/false。参数类型为数值型，如果不是则用Number()转换。

### 字符串

1. 字符串一旦赋值即确定，无法通过下标修改某个字符，也无法修改length属性。
2. 字符串里的每个字符都是2个字节表示。这将导致某些特殊单个字符的length值为2，因为内部需要4个字节才能存储。
3. Base64转码：能将任意字符转为0-9、A-Z、a-z、+、/这64个字符。 btoa()转为Base64编码，atob()转为正常值。该方法不适合非ASCII码，如中文，除非使用encodeURIComponent()和decodeURIComponent()

### 对象

1.  基本概念：对象是无序键值对的集合
2.  键名：内部均转换为字符串
3.  歧义：单独的{}会被视为代码块，({})圆括号包裹起来被解释为表达式，里面的{}被视作对象
4.  属性的查看： Object.keys(obj)
5.  属性的删除： detelete obj.xx ,该方法删除一个不存在的属性不会报错，因为该操作不会对程序产生任何影响，且返回true，因为读取不存在的属性值为undefined
6.  判断一个属性是否存在： prop in obj ,使用in运算符即可, 缺点在于无法识别是否是继承属性还是自身属性，可以另外使用obj.hasOwinProperty(‘属性名’)
7.  属性的遍历： for in 循环遍历可遍历的属性，返回属性名（属性可能继承/自身）
8.  with语句：with (obj) { xx = XX }, 自能对已定义的属性进行操作，否则创建全局变量

### 函数

1.  声明方式：
    1.  function命令 function fnc(){}
    2.  匿名函数function() {},这种函数只能赋值给其他变量，如果给一个变量赋值一个有名函数，这个函数名只能在函数内部调用
    3.  Function()构造函数：Function(‘参数1’，‘参数2’， ‘参数3‘, ... , '函数体')
2. 第一等公民：函数本质是一个可执行的值，与基本类型值地位一样。因此函数名才会被提升到代码的头部，但是函数提升不适用于函数表达式。
3. 函数的属性和方法：
   1. func.name: 返回具名函数的名或者函数表达式变量名
   2. func.length: 返回剩余参数之前的形式参数个数，如果需要判断实际参数，使用argument.length
   3. func.toString(): 返回函数源码
4. 函数的作用域：
   1. 局部作用域：函数内部定义的变量称之为局部变量，只能在函数内部读取，除非提供外部访问接口（闭包）
   2. 函数内部的变量提升：函数内部用var定义的变量被提升到函数头部
   3. 函数本身的作用域： 在函数内部访问的变量在定义时就会确定访问的变量是哪一个作用域的，与运行时无关。（该知识点容易混淆：函数的作用域仅限于定义时，只有this是运行时确定，箭头函数中的this也是定义时确定）
5. 闭包： 闭包就是一个函数能够读取其他函数内部变量
   1. 优点：闭包能够延长变量的生命周期和封装对象的私有属性和方法。因为我们知道函数内的作用域是运行时确定的，当JS引擎发现，一个函数运行完毕后，仍然有其他函数需要该函数的运行环境时（比如使用当前运行环境的某个变量），不会对其环境和变量进行垃圾回收。
   2. 缺点：内存消耗大，每次运行一次函数都会产生一个新的闭包
6. eval命令：接受一个字符串作为参数，并当作语句执行，如果不是字符串原样返回。并且eval没有自身作用域。

### 数组

1.  概念：按次序排列的一组值。属于特殊的对象。因此可以赋值非数值 键名
2.  length: 该属性只返回最大整数数值键名 + 1
3.  in运算符： 数组是一种特殊的对象，in运算符也可用于数组。
4.  for in循环：可以遍历数组中非数值的键值，因此不推荐使用for in遍历数组，而是forEach方法/for of。
5.  空位与undefined: [,,,]与[undefined,undefined,undefined]不同，前者称之为空位，访问空位值为undefined,在使用forEach/Object.keys/for of时会跳过前者的空位，但是不会跳过后者。
6.  类数组对象： argument/dom元素集/字符串，可以使用Array.prototype.slice.call(arrayLike)转为真正的数组/[...arrayLike]

### 运算符

1.  加法运算符： 如果双方没有字符串，则隐式转换为数字相加。其中一方有字符串则转为字符串相连接。如果其中一方为对象，则必须转为原始类型数值(valueOf => toString)，一般来说valueOf总是返回对象自身，而toString返回表示数据类型的字符串
    1.  其他运算符（- 、  * 、%）：一律转为数值，余数运算符的正负号由第一个数决定
    2.  数值运算符(+): 能够将任何值转为数值，类似Number()。如：+[] => 0
    3.  指数运算符（**）：2 ** 3 ** 2 = 2^(3^2) = 512

2.  比较运算符：如果比较大小，先看左右是否都是字符串，是字符串则按Unicode编码比较，否则转为数值比较大小。如果比较是否相同，严格相等会判断类型，不同则false，相同则比较值/地址。相等运算符比较，若类型不同则将数据进行类型转换，均转换为数值(如果双方都是字符串则不用转换)。显然对象回调用valueOf和toString方法
    1.  原始类型值相等比较特例：undefine和null之间任意相等比较返回true, undefined或者null和其他类型值比较时返回false。

3.  布尔运算符
    1.  取反运算符：！能够将布尔值取反或者将非布尔值转为布尔值再取反（如!undefined、!null、!0、!’‘均先转为布尔值再取反为true）。只有!!连续两个才是将非布尔值转为布尔值
    2.  &&： 返回遇到的第一个运算子的布尔值为false的运算子的值。如果都是true,则返回最后一个运算子的值，并且只要遇到false,之后的运算都不进行。
    3. ||: 返回第一个为运算子的布尔值为true的值，如果都是false,则返回最后一个运算子的值，并且只要遇到true,之后的运算子都不进行
4.  位运算
    1.  ～～： 可以实现取整
    2.  互换两个变量的值的三种方法：a= a+b; b=a-b; a = a-b; 以及a^=b;b^=a;a^=b;还有解构赋值一一对应取值。不过第一个存在溢出风险。
    3.  开关作用：分别设置变量A、B、C、D、flags为1（0001）、2(0010)、4(0100)、8(1000)、5(0101)，现在需要检测开关C是否打开，则可if( flags & C )即可得到（0100）为true。


5. 标准库
   1. Object
      1. 判断一个某个值是否是对象 typeof / instanceof / xx === Object(xx)返回true
      2. 判断一个对象的可遍历属性个数：Object.keys(obj).length 该方法不包括继承属性
      3. 判断一个对象的属性个数（包含不可遍历属性）：Object.getOwnPropertyNames().length
      4. 判断一个变量的数据类型：Object.prototype.toString.call(xx).match(/\[object (.*?)\]/)[1].toLowerCase()
      5. 拷贝一个对象的属性： function copyProperties(to, from) { for (var property in from) { if(!from.hasOwnProperty(property)) continue; Object.defineProperty(to, property, Object.getOwnPropertyDescriptor(from, property));}}
      6. 如何为使用Object.freeze(obj)的对象添加属性： 通过原型对象，除非原型对象也冻住
   2. Array
      1. 判断一个值是否是数组： Array.isArray(arr) / instanceof
      2. 浅拷贝方法：arr.concat(),slice()
      3. 改变原数组的方法：pop(),push(),shift(),unshift(),reverse(),splice(start,count,newelement...),sort()
      4. 不改变原数组的方法：slice(),concat(),map()返回新数组,filter(),forEach()无法返回值且跳过空元素
      5. 将类数组转为真数组：Array.prototype.slice.call(arrLike) / [...arr]
      6. 判断一个值是否在数组中： arr.indexOf(xx) >= 0 ,该方法无法确认NaN
   3. Number
      1. 进制转换： (十进制数字).toString(2/8/16); 将十进制转为2进制。将其它进制转为10进制:parseInt()
   4. String
      1. 查找某个字串在字符串中出现的位置：String.prototype.indexOf(str, [起始位置])
      2. slice、substring以及substr的区别：都是截取一段字符串，区别参数不同，substring起始位置小于结束位置时自动调换，如参数小于0则返回空串，而slice不能调换位置，且参数小于0默认为倒数位置，substr的第二个参数为截取字符个数而非结束位置
      3. match和search的区别：match返回数组，search返回匹配的第一个位置
   5. Math
      1. 返回任意范围的随机数：function getRandomArbitrary(start, end) { return Math.random()*(end - start) + start}
   6. Date
      1. 起始时间：1970年1月1日00:00:00
      2. Date() : 普通函数，返回当前时间的字符串
      3. new Date(): 返回日期对象的实例，参数可以是多种日期格式
      4. 日期对象之间相减得到两个日期对象之间的时间戳之差，两个日期对象相加得到日期字符串相加
      5. Date.now(): 返回当前时间距离起始时间的时间戳
      6. Date.parse(str): 解析日期字符串
   7. RegExp
      1. 量词符(?、+、*)后加问号(？) = 非贪婪模式匹配，表示匹配成功即可返回而无需再往下检查。如'aaa'.match(/a+?/),结果输出 ['a']
      2. 如果需要获取全局匹配字符串的捕获组，使用str.match(/xx/g)只能获取全部匹配的子串，而reg.exec(/xx/g)配合循环可以获取所有的捕获组
6. 面向对象编程
   1. new原理：新建一个空对象，并将空对象的原型对象指向构造函数的prototype属性，将空对象赋值给this,最后返回该对象（需要判断返回值是否是对象）
   2. 检测是否是new命令的函数调用：函数内部使用new.target，如果非new调用构造函数，该属性返回undefined
   3. 找出数组对大的元素： Math.max.apply(null, arr)
   4. instanceof判断失真：Object.create(null) instanceof Object , 因为instanceof会检测右边构造函数的是否在左边对象的原型链上，由于左边指定原型为null因此出现判断失真
   5. 获取实例对象的原型：Object.getPrototypeOf(实例对象),不建议用__prototype__.
   6. this一般指向执行上下文，特例：事件监听中指向dom元素，定时器指向window, 箭头函数指向定义时的环境
7. Promise对象
   1. 图片懒加载：```var preloadImage = function (path) { return new Promise(function (resolve, reject) { var image = new Image(); image.onload = resolve; image.onerror = reject; image.src = path;})}```
8. 浏览器对象模型
   1. 浏览器加载js代码的方式：
      1. script元素内部插入：直接在script内部嵌入代码
      2. script标签加载外部指定文件代码：通过src属性，如果src内存在非英文字符，需要设置charset,如果想防止外部脚本代码被篡改，可以设置integrity属性设置唯一hash值
      3. 事件属性：如onclick="xxx"，可以直接执行js代码，间隔符为；
      4. URL协议：如<a href="javascript: xx;">文本内容</a>, href属性为url协议，允许执行JS代码，如果JS代码返回的是字符串将会替换文本内容，如果不想替换，则需要添加javascript:void 或者末尾添加void 0;
   2. script元素
      1. 工作原理：浏览器渲染引擎边下载html文档，边顺序解析，遇到script标签如果是内部代码直接执行，如果是外部文件则暂停解析，由网络引擎下载完脚本后交给JS引擎负责解析执行（因为JS可以修改dom）。因此通常script标签放在body的尾部，可以防止页面堵塞。如果脚本中引用了样式表，也会暂停执行脚本，等待样式表加载解析才会继续执行脚本。同时浏览器也限制同一域名的资源下载数量：6-20个，因此静态文件也通常放置在不同的域名之下。
      2. defer属性：html解析遇到带defer的script外部脚本，继续解析html并并行下载外部脚本，延迟外部脚本的执行，直至html解析完毕，再按书写顺序执行脚本，解决阻塞效应。（正事先做完，杂事还在路上，即使到了我也得先做完正事）
      3. async属性：html解析遇到带async的script外部脚本，继续解析html并并行下载外部脚本，解析期间如果外部脚本下载完毕会暂停html解析来按下载顺序执行JS脚本。（正事先做，杂事还在路上，杂事到了就先做杂事）
      4. 动态加载：通过createElement创建script标签实现，需要设置async为false来保证脚本的执行顺序。
   3. 浏览器的组成
      1. 渲染流程：解析-合成-布局-绘制，这四部无需等待顺序执行，即可以解析一部分即可开始合成布局
      2. 重流和重绘： 优化特性：尽量修改局部dom, 累积修改dom/一次性操作dom，比如使用window.requestAnimationFrame(回调函数)来累积执行。
      3. window.requestAnimationFrame()该方法将会延迟回调函数的执行，直至在下一次回流之前执行。
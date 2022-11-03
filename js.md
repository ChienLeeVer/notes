 # JS

标签（空格分隔）： javascript

---
### JS简介
    1.js由ECMAscript、DOM、BOM组成
    2.JS特点：解释型语言（边解析边执行）、单线程（任何时刻都只有JS引擎控制的主线程在执行）、ECMAScript标准;
    3.引入JS的三种方式：行内式、内嵌式、外链式
    4.js的两种注释方式：\\ or \**\ ，html的注释方式：<!---->，css的注释方式：/**/
    5.js的输出语句：alter()、confirm()、prompt();

### ECMAScript
    1.常量：数字常量、字符串常量、自定义常量

    2.定义变量的三种方式：var 、 let 、const，变量名由数字、字母、下划线、美元符号组成，不能以数字开头;（中文也可以，一般不用）

    3.数据类型：基本数据类型和引用数据类型，基本数据类型有Number、String、Undefined、Null、Symbol、bigInt、Boolean,引用数据类型有Object,如array、function、date、error···。基本数据类型存储在栈空间，引用数据类型存储在堆空间；

        （1）number:js内部所有数字存储为64bit（8字节）浮点数，需要整数时转为32bit整数。第一位为符号位，2-12位为指数位（11个）决定数值大小，后面全为小数点位决定精度大小。而第一位默认为1，因此64-11 = 53位为js提供的有效数字范围。因此15位以内的十进制数字都可以保持精度。数字 = (-1)^符号位 * 1.xx...xx(53位) * 2^指数部分

        （2）string:字符串base64转码，支持将ASCII码（0-31）无法显示的字符转为0-9、a-z、A-Z、+、/（共64位）能够显示的字符，btoa()将ASCII码转为base64，atob()将base64转为ASCII码。如果要将非ASCII码转为base64，则btoa(encodeURIComponent(str)),如果需要转回来则 decodeURIComponent(atob(str))

        （3）对象：
            a.delete命令只能删除自身属性，不能删除继承属性，只有设置属性为不能删除时才返回false，否则无论属性是否存在都返回true.

            b.with语句用于统一操作一个对象的属性，如var obj = {a:1,b:2} with(obj){ a = 2; b = 3}，在with语句内只能操作obj已有的属性，如果操作不存在的属性，等价于全局定义一个变量，并不会给对象赋值，如 var obj = {a:1,b:2} with(obj){ a = 2; b = 3; c =4} obj.c; //undefined c; // 4

    4.typeof获取变量的数据类型，注意，typeof null -> 'object'; typeof undefined -> 'undefined';

    5.流程控制语句分为：顺序结构、选择结构、循环结构；

    6.对象：内置对象、宿主对象（BOM/DOM）、自定义对象；

        (1)Object:
            a.Object.keys(obj);与Object.getOwnPropertyNames(obj);均返回自身属性，但是后者还可以返回不可枚举的属性.

            b.除了原始对象外，其它对象类型Array,Function,Date都重写了toString()方法，数组返回自身值...因此如果想通过toString()判断数据类型，需要调用Object.prototype.toString.call()

    7.基本包装类型：基本数据类型不能绑定属性和方法，但是引用数据类型可以，比如var a = '123sf';不可以，但是var a = new String('123sf');可以绑定属性和方法。还有Numebr()、Boolean()也是包装类可以将基本数据类型的数据转换为对象。

    8.内置对象：
        （1）String:
                a.indexOf(需找的字符串)返回查找到的字符串第一次出现索引的位置，找不到为-1，当出现indexOf(字符串，起始位置)返回值为索引位置，这个索引位置是从起始位置开始计算的。

                b.search(正则/需找的字符串)，返回第一次找到的位置索引，在正则表达式中会忽略g，即全局查找

                c.includes(需找的字符串, [起始位置=0])返回true or false；

                d.startWith(需找的字符串, [起始位置=0]);默认从0开始到结尾。

                e.endWith(需找的字符串, [结束位置=length]);默认结束位置为字符串尾
                f.charAt(索引)获取指定位置的字符

                g.charCodeAt(索引);获取指定位置的字符的Unicode编码，注意一个中文2个占位，英文一个占位

                h.slice(开始位置，结束位置)获取字符串中[开始位置，结束位置)的内容，负数表示倒数

                i.substring(开始位置，结束位置)获取字符串中[开始位置，结束位置)的内容,不接受负数，但是可以自动调整参数的位置，如果第二个参数小于第一个，则自动交换。

                j.substr(开始位置, 长度)不被ECMScript接纳

                k.String.fromCharCode(Unicode编码)将Unicode编码转为字符串

                l.split(分隔符);将字符串拆分成数组；

                m.replace(被替换的字符串，替换的字符串);

                n.repeat(重复次数n);返回一个将字符串重复n次的字符串

                o.trim()返回一个去掉前后空白的字符串

        （2）Number：
                a.Number.isInteger(整数);返回一个布尔值

                b.toFixed();保留多少个小数点

                c.encodeURIComponent();把字符串作为url进行编码

                e.decodeURLCimponent():解码

        （3）Date:
                a.getTime()；返回时间戳；

                b.getFullYear();返回执行代码时的年份

                c.getMonth()；返回执行代码时的月份；

                e.getDate();返回执行代码时的日子

                f.getDay();返回今天是星期几

                g.getHours()；返回现在多少点

                h.getMinutes()；返回现在多少分

                i.getSeconds（）；返回现在多少秒

        （4）Array：
                a.创建数组的三种方式：var arr = []; var arr = nwe Array(参数); Array.of(参数) new Array一个参数时表示数组长度，而Array.of表示数组的值。arr.length可修改;

                b.判断是否为数组：Array.isArray()；

                c.Array.from（伪数组）;能够将伪数组转为真数组，从而可以调用数组的方法

                d.将数组转为字符串的三种方式：数组.toString(); String(数组); 数组.join(',');

                e.arr.slice(开始位置，结束位置)返回[开始位置，结束位置)的子数组，负数表示倒数第几个

                f.arr.splice(开始位置，删除的个数，替补元素1,替补元素2);该方法删除原数组内的元素，返回被删除的元素

                g.数组的合并：arr.concat(newArr1，newArr2...)或者 [1,2,3,...arr];

                h.将数组转为字符串：arr.join(连接符);连接符默认为‘,’

                i.将数组反转：arr.reverse()；

                j.将数组排序：arr.sort((a,b)=>a-b); return>0 a<=>b,change array

                k.获取数组元素的位置：arr.indexOf(item,[开始位置]); return index;并且执行全等判断，无法查找NaN

                l.判断一个数组中是否有该元素：arr.includes(item, [开始位置]); retur true or false;可以查找NaN；
                m.找到以一个满足条件的元素：arr.find((item, index, arr)=>{
                    return item == 6;
                });
                n.找到以一个满足条件的元素索引：arr.findIndex((item, index, arr)=>{
                    return item == 6;
                });

                o.遍历一个数组：arr.forEach((item, index, arr)=>{
                    //这种情况下修改item是否改变原数组，如果原数组为对象数组，那么item表示数组中的一个元素，此时修改item.属性，会修改其内容，但是直接修改item = newObject 则不会改变原数组；终止迭代的方法：使用try{ throw error} catch(e){};
                }); no return

                p.遍历一个数组：arr.map((item)=>{});修改情况和forEach一样,no return

                q.返回所有符合条件的元素：arr.filter((item, index, arr)=>{
                    return item == 4;
                });

                r.累加器：arr.reduce((上次函数的返回值，这次函数的item,这次函数的index,)=>{},初始值);
                s.添加元素： arr.unshift(a,b,c) => [a, b, c] 而不是[c, b, c]
                            arr.push(a,b,c); => [a,b,c]

                t.判断数组中是否存在有符合条件的元素：
                            arr.some(function(){return condition}); return true or false;
                            arr.every(()=>{return condition}); return true or false

                u.关于forEach()/filter()/reduce()/every()/some()中数组存在空值的情况:ES5中，map()会跳过空位，但会保留这个值，join()/toString()会默认undefined,如果值为undefined/null，当作空字符串，而ES6中，所有方法都会默认空位为undefined;

    9.函数：
            （1）函数的定义方式：命名函数、匿名函数、构造函数。

            （2）函数的参数：当实参数量多余形参时，多余的会被忽略。argument是一个伪数组，代表实参，argument.callee表示当前的函数名，arugment可以修改里面的元素，但是不能改变其长度。

            （4）this：命名函数、立即执行函数、定时器函数、匿名函数直接调用时指向window, 作为对象的方法时指向该对象，作为构造函数时指向实例对象，作为事件绑定对象的函数时指向该事件绑定对象。注意：箭头函数继承外部函数的this

            （4）call、apply、blind: call(需要传递的this,参数1，参数2)；apply(需要传递的this,[参数数组])，call和apply很相似到那时apply可以传递数组都会调用方法，而blind(需要传递的this,参数1，参数2)只改变this不调用方法。当传递的this为null/undefined时，非严格模式下默认指向window

            （5）闭包：有权访问另一个作用域的变量的函数称之为闭包，如一个函数中参数为函数，返回值为函数等。闭包函数中的内部函数访问的外部函数的局部变量将会被一直保存，直至该内部函数被调用结束。闭包的作用能够延长作用域的范围

            （6）构造函数：构造函数默认返回一个对象，如果返回值不是一个对象，那么会自动返回一个空对象。

            （7）作用域：函数本身也有作用域，函数的作用域在定义时确定，而不是运行时确定，即函数定义时所用的变量及其变量的作用域在定义时已经确定，而不是和this一样运行时确定。

            （8）length属性：返回形式参数个数（在剩余参数符之前的形参个数）

            （9）argument:返回实参个数，严格模式下修改argument不会影响函数参数



    10.对象：
            （1）定义方式：对象字面量obj ={}这种方式没有constructor, new关键字obj = new Object(),class关键字

            （2）判断一个对象构造函数是否有该属性：实例对象.hasOwnProperty('属性名')
                 判断一个对象是否有该属性：'属性名' in 实力对象；包括原型链上的属性；

            （3）浅拷贝：Object.assign(需要拷贝的对象)；

            （4）深拷贝：function deepCopy(oldObj, newObj) {
                    for(let key in oldObj) {
                        let item = oldObj[key];
                        if(item instanceof Array) {
                            let newArr = [];
                            argument.callee(item, []);
                        } else if(item instanceof Object) {
                            let newObject = {};
                            argument.callee(item, newObject);
                        }else {
                            newObj[key] = item;
                        }
                    }
                }
                例子：深拷贝做两件事，for循环遍历旧对象的属性名和值，如果该属性是数组或者对象则创建一个新的空数组或者空对象然后将当前遍历到的属性和空数组（对象）作为参数调用自身函数，否则将属性正常赋值给另一个拷贝对象。
            （5）冻结对象：Object.freeze(obj)；冻结后无法修改属性

            （6）继承：所有对象都继承自Obejct，所有对象都有一个原型链（Prototype），每个实例对象（object）都有一个私有属性（称之为 __proto__ ）指向它的构造函数的原型对象（prototype），访问属性时，首先访问自身属性，找不到才会在原型链上找.而且只有构造函数才有prototype,实例对象只有__proto__.比如：let s = new String('abc'); s.__proto__ == String(构造函数).prototype(String的原型对象) == Obejct;
            （7）

    11.正则表达式：慎用 正则.test()方法，该方法在正则添加全局g模式之后，每次只会查找一次，找到后修改lastIndex为找到的下一个位置,然后下一次继续从lastIndex开始查找，如果没找到就设置为0
    var reg = /(123)(456)\1\2/g; \1表示重复第一个括号的内容，\2表示重复第二个括号的内容
       等价于 /(123)(456)(123)(456)/g

    12.DOM:
        （1）创建节点：document.createElement('标签名');

        （2）添加节点：父节点.append(新节点); or 父节点.insertBefore(新节点，参考节点);

        （3）删除节点：父节点.removeChild(子节点)

        （4）获取节点：父节点.children();该方法返回值获取子节点，不包括文本节点。子节点,parentNode()返回父节点

        （5）获取节点属性：节点.getAttribute('属性名') or 节点['属性名'] 两者的差别在于，getAtrribute能够获得标签自定义的属性

        （6）设置节点属性：节点.setAttribute('属性名',属性值);

    13.event: 对象绑定事件的方式有 对象.onclick = ()=>{} or 对象.addEventListener('click',()=>{},false);差别在于addEventListener能绑定多个同名事件而不被覆盖。通过event.target可以找到父元素的子元素

    14.事件传播与冒泡：
        （1）以下事件不冒泡：blur、focus、load、unload、onmouseenter、onmouseleave。
        （2）阻止冒泡的方法:元素.stopPropagation(); ie中 元素.cancleBubble = true;
        （3）事件委托：父元素.addEventListener('事件名', (event)=>{
            if(event.target && event.target.className == xx){
                //执行...
            }
        });能够将父元素中的子元素全部绑定相同的事件，而不同通过遍历每个子元素来添加相同的事件，有效提高性能

    15.class:
        （1）作用域：class与let、const一样存在块级作用域，不存在变量提升，并且存在暂时性死区（在一个块级作用域中存在class时，在该变量声明之前都不可用，外部同名变量将被屏蔽）
        （2）set/get: 这两个方法必须同时设置，单独设置一个时会报错，并且函数初始化不会进行
        （3）本质：class本质就是function,因此typeof class ; return function


    16.运算符
        （2）！:取反运算符，如果是布尔值直接取反，如果不是则先转为布尔值再取反
        （2）&& ：且运算符，返回第一个false的表达式的值，如果都为true,则返回最后一个表达式的值
        （3）|| : 或运算符，返回第一个为true的表达式的值，如果都为false，则返回最后一个表达式的值

        （4）｜、&、～ : 会舍去小数，其中～～是取整最快的方法。取整方法：~~、正则匹配、字符串截取、parseInt、Number、Math.Fixed()。通常可以用作开关；

        （5）^ ： 异或运算，核心思想先让其中一个”加起来“，另一个再用和“减去”自身得到对方，最后“和”再减去对方的到交换值
            a.在不使用临时变量的情况下交换两个变量，a ^= b ; b^=a; a^=b;
            原因如下：a=x, b=y; a=a^b=x^y; b=a^b=x^y^y=x; a=a^b=x^y^x=y;
            b.a = a+b; b=a-b=a; a=a-b=b；但存在溢出风险



### 数据类型转换
    数据类型转换只有转换为字符串、数字、布尔值。
    1.显示类型转换：
        （1）toString(X):出了null和undefined之外基本其它数据类型变量都有toString()，因为浏览器会临时将基础数据类型变量进行包装变为String对象，这个对象的方法继承自Object.toString()，当计算完毕后转转为基础数据类型;
            说明：X可写可不写，默认十进制，可写 2，8，16进制；null和undefindm没有此方法。

        （2）String():可以将null -> "null", "undefined" -> "undefined";

        （3）Number(): 字符串转数字（全为数字则数字，否则NaN)。布尔值转为1、0。null转为0。undefeined 转为NaN, 空数组转为0，对象/数组转为NaN）;
            说明：+a、-a:首先会调用Number(a)，这是一个将变量a转换为数字类型的技巧；

        （4）parseInt(string, radix): 首先将变量转换为字符串，然后再进行处理。第二个参数而2-36表示选定的进制，默认根据第一个参数选择。
            说明：  a.如果开头为数字则取数字直至遇到第一个不是数字截止，再将截取的转化为数字；
                    b.如果字符串开头不是数字，全部转化为NaN；

                    c.parseInt(string, radix) 解析一个字符串并返回指定基数的十进制整数，radix 是 2-36 之间的整数，表示被解析字符串的基数。

        （5）Boolean():数字中0和NaN转为false,字符串中空串转为false,null和undefined转为false；
            说明：  a.对象转为true,包括空数组[]、空对象{}；

    2.隐式类型转换：
        （1）-、+、*、/、% ：在+中，有字符串时，全部通过String（）转换为字符串，否则通过Number()转换为数字，然后再进行计算。
        （2）++、-- :先调用Numbe()再进行计算；

        （3）isNaN():先调用Number()，然后再判断是否是数字，返回布尔值；

        （4）||、&&:
            a.在||运算中，先计算左边表达式，再调用Boolean()判断是否true,左边为true则不计算右边的表达式，返回值为左边的表达式，如果左边为false，则会继续计算右边的表达式，直至遇到true,返回值也为最后一个遇到true的表达式，如果全部为false则返回最后一个true的表达式;
            b.在&&运算中，先计算左边表达式，再调用Boolean()判断是否true,左边为false则不计算右边的表达式，返回值为左边的表达式，如果左边为true，则会继续计算右边的表达式，直至遇到false,返回值也为最后一个遇到false的表达式,如果全部为true则返回最后一个true的表达式;

        （5）!a : 实质Boolean(a)与+a有异曲同工之妙；都是做类型转换

        （6）<=、>=、==、!=、===等：会将数字比较，非数字将会调用Number进行计算，但当左右两边都是字符串的时候不会进行转换，NaN和任何数据类型比较都是false。
            说明：==  先检查两个操作数数据类型，如果相同， 则进行===比较， 如果不同， 则愿意为你进行一次类型转换， 转换成相同类型后再进行比较， 而===比较时， 如果类型不同，直接就是false.==比较规则：如果两边都是基本类型且同数据类型直接比较，不同数据类型转为数字。如果有一边是对象，一边是基本数据类型，则对象先调用valueOf(),如果仍然不是同数据类型，则调用toString();
            特例：undefined和null相互比较返回true,undefined/null与其他数据类型比较都会返回false

### 函数名、函数体、函数加载的问题

```
function fn() {
    alter();
}

console.log(fn); /*打印function fn() {
    alter();
}*/

```

上示例中，fn()表示调用fn函数体即执行相关行为，而函数名fn指整个函数体内容，即函数对象。
函数加载时只加载函数名，不会加载执行函数体，如果想使用函数体内的成员变量只能调用函数体。

```

function fn(a, b) {
    console.log(fn.length); //打印形参个数,即2
    console.log(arguments.lenth);//打印实参个数,即3
    alter();
}

console.log(fn); /*打印function fn(a, b, c) {
    console.log(fn.length);
    console.log(arguments.lenth);
    alter();
}*/
fn(1,2,3);
```

arguments对象为函数的内置对象，保存了函数的实参，属于伪数组，可以修改里面的变量，但是不可以添加删除，除此之外还有arguments.callee，等同于函数名，arguments.callee()等同于在函数体内调用自身；

### 字符串
    1.字符串的值保存在栈中;
    2.模版字符串不但可以使用变量，还且可以调用函数、使用表达式、嵌套模版字符串;

### 数值型
    1.NaN是一个特殊的数字，任何数字和Undefined计算结果都是NaN，NaN和NaN并不相等，判断一个数是否是NaN，可以调用isNaN(number)来判断number是否是NaN，返回值是布尔值；
    2.浮点数运算：由于1/3在计算机中以二进制的形式无法精确的表示，因此会产生经度丢失，较好的做法是直接调用函数库或者将小数点和整数分别提取出来然后再进行计算。

### NULL
    1.null本身就是一个特殊的对象（即可隐式转换为对象），但是是一个地址为空的对象，因此归类为八大基础数据类型之一；任何数字和null计算，null都会变成0；

### Undefined
    1.undefined衍生自null,因此也属于一个特殊的对象（即隐式转化为对象类型），也是八大基础数据类型之一。通常产生于变量未声明、未赋值、函数无返回值等；任何数字和Undefined计算都是NaN

### Math
    1.Math.random() : 生成[0,1)之间的随机数，Math.random()*X生成[0,x)之间的随机数，Math.random()*(y-x) + x生成[x,y)之间的随机数，Math.floor(Math.random()*(max - min + 1)) + min;生成[min, max]之间的随机数。

    说明：
        Math.random() -> [0,1)
        Math.random()*x -> [0, 1)*x = [0, x);
        Math.random() + y -> [0,1) + y = [y, 1+y);
        Math.random()*x + y ->[0, 1)*x + y -> [0, x) + y -> [y, x+y);
        Math.random()*(max-min) +min ->[0, 1)*(max-min) +min -> [0, max-min) + min ->[min,max)
        Math.random()*(max - min +1) ->[0, max - min +1);
        例子：max=10, min=5; Math.random()*(10 - 5）表明只能取到[0，5)之间的数，而
        Math.random(10 - 5 + 1)表明只能取到[0,6)之间的数，但是我只想要0-5之间的数怎么办呢？可以这样做,当随机数大于5时，使用Math.floor向下取整，如果想要0-5之间的整数，可以不加判断直接全部Math.floor()向下取整。因此
        Math.floor(Math.random*(max - min + 1))表明取到[0, max-min+1]之间的整数；
        Math.floor(Math.random*(max-min+1)) + min 表明取到[min, max+1)之间的整数,即[min,max];
        以数轴为例子：------------min---------max--->,
        min表示下限提高了起点，max表示上限，max-min表示范围，Math.random()*（max-min）+ min表示在min起点的基础上，加上[min,max)这个范围内随机一个数但不包括max，Math.random()*(max-min+1)+min则会包括max,但不包括max+1，如果不想要max以上的数可以向下取整。

### 伪数组转为真数组
>```Array.from(arr)```,这种方法经常用在只有length的伪数组上，如dom操作时获取的标签集合，参数里的arguments等。

### 清空数组

```
var array = [1, 2, 3, 4, 5, 6];

array.splice(0); //方式1：删除数组中所有项目
array.length = 0; //方式2：length属性可以赋值，在其它语言中length是只读
array = []; //方式3：推荐
```

### 作用域与变量提升

1. 凡是用var定义的变量将得到变量提升，在代码执行之前都会被浏览器事先声明并赋值为undefined，即在var之前使用该变量将会的到undefined。任何变量，如果未经声明就赋值，此变量是属于 window 的属性，而且不会做变量提升。因此未经声明的变量不可在没赋值之前访问该变量，否则报错。

2. 全局作用域不可访问局部变量，局部作用域可以访问全局变量，全局变量属于window对象，window.xx;，因此函数内部可访问window对象，全局变量和方法将会在窗口关闭后消失，而局部变量使用完毕后就消失。

例子：全局变量和局部变量相当于公共物品和私人物品，用var定义一个变量相当于提前告诉所有人这是私人的还是公家的，在函数内部相当于在家，在家可以用公家的东西，一般人不能进到你家用你的东西，但是如果你家的东西没提前证明这是私人物品，那么就是公家的（内部没用var）。如果你没用var，别人也不知道你家的东西是不是公家的，因此只有你用过了之后（函数定义后），别人才知道你的这个东西是公家的（没var的变量没有变量提升，无法在定义之前访问）。

### 预编译
    在JS代码执行之前，会对代码进行语法分析、预编译、解释执行
    1.语法分析：检查有没有明显的语法错误
    2.预编译：
        (1)创建AO对象（执行期上下文），如AO = {

        }
        (2)找到函数内的形参和参数变量声明，作为AO的属性并赋值为undefined，如
        AO = {
            形参1:undefined,
            形参2:undefined,
            局部变量1:undefined,
            局部变量2:undefined
        }
        (3)形参和实参统一，实参的值赋给形参，如
        AO = {
            形参1:'123',
            形参2:'456',
            局部变量1:undefined,
            局部变量2:undefined
        }
        (4)找到函数内函数声明，函数名为AO的方法名，函数体为AO的值，如
        AO = {
            形参1:'123',
            形参2:'456',
            局部变量1:undefined,
            局部变量2:undefined,
            getName:function getName(){
                return this.name;
            }
        }
        因此分析时，首先先分析变量的声明，其次实参赋值，然后才是函数声明，最后是声明之后的其它运算；
    3.代码执行优先级：同步任务-> 微任务 -> 异步任务

### 获取元素样式的三种方式

```
    var box = document.querySelector(".box");
    /* 获取borderHeight + paddingHeight + contenHeight */
    var boxHeight = box.offsetHeight;

    /* 如果行内样式没有事先设置则为空 */
    var innerHeight = box.style.height;

    /* 获取外部样式 */
    var outHeight = window.geiComputedStyle(box, null)["height"];
```

### 纯JS动画实现

```
    function animation(element, target) {

        //清除element身上的定时器，避免定时器之间冲突
        clearInterval(element.timer);
        //判断target在元素的左边还是右边决定向左还是向右,假设每次移动10px;
        let steep = element.offsetLeft < target ? 10 : -10;
        //给element设置定时器
        element.timer = setInterval(()=>{
            //获取当前元素与目标的距离
            let val = element.offsetLeft - target;
            //如果当前该元素与目标的距离小于每次移动的距离10px,则将该元素位置直接设置为target
            if(Math.abs(val) < Math.abs(speed)) {
                element.style.left = target + 'px';
                //此时已经说明这是最后的移动，关闭定时器；
                clearInterval(element.timer);

            }else{
                element.style.left = element.offsetLeft + speed + 'px';
            }
        }, 10);

    }
```

### 滚动条经验
当某个元素满足scrollHeight - scrollTop == clientHeight时，说明垂直滚动条滚动到底了。

当某个元素满足scrollWidth - scrollLeft == clientWidth时，说明水平滚动条滚动到底了。

这个实战经验非常有用，可以用来判断用户是否已经将内容滑动到底了。比如说，有些场景下，希望用户能够看完“长长的活动规则”，才允许触发接下来的表单操作。

例子：假设一个长长的规则占满了屏幕，内容分为A+B两个区域，当用户滑到底部时，此时恰好A区域被滑到了上面，能看到的区域为B,那么此时scollTop为滚轮滑动的高度恰好等于A区域的高度，而clientHeigh表示可视区域内的高度，此时可视区域内只能看到B，因此clientHeight = B高度，由于scrollHeight为内容高度即A+B的高度，因此 scrollHeight = scrollTop + clientHeight时，用户滑到了底部。
小孩例子：假设有一卷纸，卷纸上很多花纹，小孩玩耍抽了好多出来，需要把卷纸卷回去，当你把卷纸卷回去一半的时候，只能看到一半的花纹，另一半在里面。此时卷纸滚动了多少就是你不能看到的多少。
 scrollHeight（全部卷纸） = scrollTop（卷纸滚动了多少即看不到的那部分） + clientHeight（看得到的那部分纸）
### 封装scroll兼容性写法

```
// 作用：获取滚轮滚动的距离
function scroll() {
    return {
        top: window.pageYoffset || document.documentElment.scrollTop || document.body.scrollTop,
        left: window.pageXoffset || document.documentElement.scrollLeft || document.body.scrollLeft;
    }
}
```

### 封装client兼容性方法

```
\\获取可视区域内的宽高
function client() {
    return {
        width: window.innerWidth || document.documentElement.clientWidth || document.body.clientWidth,
        height: window.innerHeight || document.documentElement.clientHeight || document.body.clientHeight
    };
}
```

### ES6/5
    1.ES5严格模式：在脚本顶部/函数顶部添加'use strict'; 即可启用严格模式，严格模式下限制：
                （1）禁止同名变量，同名属性；
                （2）变量必须先声明，函数声明必须在顶层；
                （3）禁止this指向window，非严格模式下指向window改为undefined；
                （4）创建eval作用域，eval语句内的变量只能在自身作用域使用；
                （5）构造函数必须通过new实例化对象，否则无法使用this（this = undefined）；
                （6）无法通过delete删除变量

    2.ES5扩展：
                （1）JSON对象/JSON数组 <-> JSON字符串；
                （2）Object.create(原型对象,自定义属性，例{
                        属性名1：{
                            value: 属性值,
                            writable: 是否可修改,
                            configurable: 是否可删除,
                            enumerable: 是否可通过for in 枚举
                        }
                    });
                （3）Obejct.defineProperties(原型对象,自定义属性);
                （4）数组.indexOf/lastIndexOf/forEach/map/filter;
                （5）函数.blind();

    3.let/const ：ES6模式下新增了块级作用域{}，需要配合let和const使用，使用let定义的变量不允许重名(即使后面用var定义的)，必须在声明之后才能使用let变量，并且不会得到变量提升，每个{}之间的都是一个作用域，不同作用域之间的let变量可以同名并且不冲突，局部作用域可以访问外部作用域的let变量，但是外部无法访问局部作用域。const同理，但是const变量保证指向的内存地址不发生变化。
    例子：{}就像一层保护墙，保护墙里面可能还有保护墙，外层保护墙有个人叫张三，外层保护墙里面的保护墙也有个叫张三，两个张三是不同的人，当外层的张三被拉去当兵的时候，是不关里面张三的事的，因为里面的张三被保护了起来，不可见。但是当里层的张三不存在时，里层是可以使用外层的张三的。

```
var a = 1;
{
    let a = 2;
}
console.log(a); // 1

let a = 1;
{
    let a = 2;
}
console.log(a); // 1

let a = 1;
{
    var a = 2;
}
console.log(a); // 不会执行，直接报错,因为var 将变量声明提升了，相当于 let a=1;var a=2;产生了重名冲突；而第一个之所以能执行，是因为let是局部变量，只作用于局部作用域；
```

    4.变量的解析构值：
        （1）a.定义变量时解析构值： let [a, b, c] = [1, 2, 3]; 这里的[]不是数组，能够一次性给a,b,c赋值为 1,2,3；
                            let [a, b, c] = [1, 2]; 注：此时c = undefined（默认）;
                            let [a, b, c = 5] = [1, 2]; 注：此时给c设置默认值；
                            let [a, b, c] = [1, undefined, null]; 注：此时b = undefined c = null;
            b.已经定义的变量/对象解析构值：
                            let a = 123, b = 2, c = 5;
                            ([a, b, c] = [1, 2, 3]); 注： 需要加上圆括号
        (2) 对象解析构值：
                    let obj = {
                        name : '章三',
                        age : 24,
                        sex : '男'
                    };

                    let {name, age, sex} = obj; 注：只要变量名一一对应对象的属性名即可

                    let {name : myName, age : myAge, sex} = obj; 注：自定义新的变量名，只需要加上冒号；

        （3）字符串解析构值： let [a, b, c] = 'jkl'; 此时 a = 'j', b = 'k', c = 'l';

    5.箭头函数： ()=>{};
                 (a = 1, b =2)=>{ console.log(a,b);} 注：给a,b设置默认值
                 (a, b)=>a+b; 注：函数只有一个return语句时可以这样写；
                 a =>{....}; 注：只有一个参数时可以省略参数括号
                 ()=>{ console.log(this)}注： 箭头函数本身不绑定this，this 指向的是箭头函数定义位置的 this；

    6.剩余参数运算符和扩展运算符：
                注：...作为定义变量时使用为剩余参数运算符，作为使用变量时能够扩展变量
                function fun(param1, ...rest) {} 注：rest代表剩余的实际参数；
                var arr = [1, 2, 3, 4];
                let [str1, ...str2] = arr; 注：此时 str2 = [2, 3, 4]; 此时作为定义str2变量使用；
                console.log(...arr);   注：此时作为扩展运算符，将数组扩展为1，2，3，4;


    7.set数据结构：
                let set = new Set(['张三', '李四', '王五', '张三']);
                console.log(set); //{"张三", "李四", "王五"} ，注：set结构能够去重
                console.log([...set]); 注：能够转为数组

### Ajax

```
const getJSON = function( url ) {
    const promise = new Promise((resolve, reject)=>{
        var ajax = new XMLHttpRequest();
        ajax.open('GET', url);
        ajax.responseType = 'json';
        ajax.setRequestHeader('Accept', 'application/json');
        ajax.send();
        ajax.onreadystatechange = function() {
            if(ajax.readyState !== 4) {
                return;
            }
            if(ajax.status == 200) {
                resolve(ajax.response);
            } else {
                reject(new Error( ajax.statusText ));
            }
        };
    });
    return promise;
}

getJSON('/posts.json').then(function(json) {
    console.log(json);
},
function(error) {
    console.log(error);
});
```

### 全等判断
a === b和Object.is(a, b):

1. Object.is()和 ===都是全等判断，但是Object(NaN, NaN) return true, Object.is(-0, +0) return false;
2. NaN === NaN return false; +0 === -0 return true;
3.

### 操作符
    1.delete:删除不可删除的变量时，严格模式下抛出异常，非严格模式下代码默认无效

### 对象和原型链

1. 创建对象的几种方式： 字面量{}，构造函数new，Obejct.create(原型)；
2. 三个概念：<br>```function Person(){  }; var p = new Person()```原型对象为Object{constructor, prototype},构造函数为Person,实例为p,```p.__proto__ =  Person.prototype = Object{constructor, prototype};```其中Object原型对象的constructor和prototype都是默认属性，constructor指向构造函数本身函数体，Object.prototype为原型对象的原型；每个实例都会继承原型对象的方法和属性；
3. new运算符发生了什么：```var obj = {}; obj.__proto__ = Obejct.prototype; var result = Object.call(obj); return typeof result == 'object' ? result : obj;```
4. 继承的几种方式： <br>（1）构造函数继承：在子类中调用```父类.call(this)```,修改this指向，但是无法继承父类的原型对象；<br>（2）原型链继承：```子类.prototype = new 父类()```，但是如果父类的属性存在引用数据类型，那么这种继承方式会导致子类实例1修改该属性时，子类实例2也会被修改；<br>（3）组合继承：```父类.call(this); 子类.prototype = Object.create(父类.prototype); 子类.prototype.constructor = 子类；```


### 防抖：对于一个事件，用户不断触发事件发生的条件，我们希望在第一次事件延迟发生之前，后续触发都会覆盖之前的事件。

```
function debounce( fn, delay ) {
    var timer = null;
    return function() {
        var context = this;
        var args = arguments;
        clearTimeout(timer);
        timer = setTimeout( ()=>{
            fn.bind( context, arguments );
        }, delay);
    }
}
box.onclik = debounce(function(){
    console.log('1');
}, 2000);
```

这样为box绑定了一个click事件，并且函数为debounce函数返回的函数，当用户第一次触发事件时，先清空定时器，然后设置了一个延时定时器，并且将编号赋值给了timer,事件将会在delay时间后执行，如果用户在规定的时间内再次触发了事件，那么timer将会被清除，此时fn函数尚未执行。此时又重置了一个延时定时器。核心思想：闭包存储定时器编号，规定时间内重复调用函数会清除定时器并且重置。
### 节流:对于一个事件，用户不断触发事件发生的条件，我们希望用户在规定的时间内事件只调用一次

```
function throttle( fn, delay) {
    let oldTime
    let timer
    return function(...args) {
        let context = this
        let newTime = new Date()
        if(!oldTime) oldTime = newTime
        clearTimeout(timer)

        if( newTime - oldTime > delay) {
            oldTime = new Date()
            fn.apply(context, args)
            clearTimeout(timer)
            return
        }
        timer = setTimeout(function(){
            oldTime = new Date()
            timer = null
            fn.apply(context, args)
        }, delay)
    }
}
```

### Promise

```
    var p1 = new Promise(( resolve, reject ) => {
        if(/**/) {
            resolve('123');
        } else {
            reject(new Error);
        }
    });
    var p2 = p1.then(function(parm) {
        return parm;
    }).then(function(parm1){
        console.log(parm1); // 123
    });
```

promise里面是同步代码，当执行resovle之后，这个promise的状态就会被修改为resolved,并且resolve(参数1)里的参数1将会传送到promise.then(function(参数2){})里的参数2。then方法返回的是一个新的promise,这个新的promise的状态由then方法里面的函数的返回值决定。
如果then方法里面的函数返回值是一个值，那么这个新的promise状态将会被修改为resolved，并且这个返回值也会被传送到更新的promise.then方法中的函数参数。
如果then方法里面的函数返回值是new Error，那么这个新的promise状态将会被修改为rejected。
如果then方法里面的函数返回值是一个promise,如new Promise.resolve(1)姑且叫p2,那么这个新的promise状态将由p2的状态决定，此时会在微队列加入p2.then(()=>{完成p1的状态并返回resolve传过来的值})任务,当微队列执行到此处时执行then方法，但里面的函数不会执行而是会重新在微队列末尾加上，即()=>{完成p1的状态并返回resolve传过来的值}任务。一次事件轮询后执行()=>{完成p1的状态并返回resolve传过来的值}，p1的状态被完成，执行p1之后的then方法，并且之后的then方法参数为p2.resolve传过来的值。
如果promise.then(parm1),会自动转换为promise.then(function(parm1){ return parm1 });
注意：Promise.resolve()方法允许调用时不带参数，直接返回一个resolved状态的 Promise 对象。如果resolve没有参数，那么then方法将会在本轮事件循环结束前、下轮事件循环开始前执行

### Symbol

1. symbol用来描述对象唯一属性名，用法

```
    let s = Symbol() // 不能使用new,因为symbol是基本数据类型
    let t = Symbol('abc') // Symbol参数仅用于描述这个symbol值,没有其它意义。
    t.toString() // “Symbol(abc)”
    if(t) // true ，除了toString和布尔值转换均不能参与其它计算
    t.description // 返回Symbol描述值'abc'
    //在对象中使用symbol值
    let obj = {
        [s] : 'sadasd'
    }
    //访问该属性 obj[s]，不能使用点运算符，点运算符总是访问字符串
    //使用symbol作为属性名的对象，该属性不会出现在for in,for of, Object.keys()中，如果想获取该属性名，只能用Object.getOwnPropertySymbol(obj)会返回一个数组，数组里为对应的属性名。如果想同时获得普通属性名和symbol，则需要用Object.ownKeys(obj)

```

2.Symbol.iterator:对象的Symbol.iterator属性，指向该对象的默认遍历器方法。

### Set和Map
>
>1. ES6 提供了新的数据结构 Set。它类似于数组，但是成员的值都是唯一的，没有重复的值。
>
2. Set函数可以接受一个数组（或者具有 iterable 接口的其他数据结构）作为参数，用来初始化。

```
//去除数组重复数据
[...new Set(oldArr)]
//去除字符串重复字符
[...new Set('aaabbbccc')].join('')
//在Set中，NaN 等于 NaN,两个空对象不相等
let set = new Set();
set.add(NaN)
set.add(NaN)
set.size // 1
set.add({})
set.add({}) // 3
//操作set对象方法如下
set.add(1).add(2) //增加某个数据
set.delete(1) //删除某个数据
set.has(2) //判断是否存在，true
set.clear() //清除所有数据
set.size // 0
//遍历set结构的实例方法如下
Set.prototype.keys() //返回键名遍历器
Set.prototype.value() //返回键值遍历器，注：Set.prototype[Symbol.iterator] == Set.prototype.value,因此set结构对象可以使用for in方法
Set.prototype.entries() //返回键值对遍历器
//如下使用方法，遍历顺序为插入顺序,set的键名和键值是一样的
let set = new Set(['red', 'green', 'blue']);

for (let item of set.keys()) {
  console.log(item);
}
// red
// green
// blue

for (let item of set.values()) {
  console.log(item);
}
// red
// green
// blue

for (let item of set.entries()) {
  console.log(item);
}
// ["red", "red"]
// ["green", "green"]
// ["blue", "blue"]
```

### Map
map数据结构类似对象，但是不同之处在于map数据结构的属性名除了字符串之外，还可以是其它对象

```
 //以前给对象属性名赋值为对象
 let obj = {}
 let obj2 = {}
 obj[obj2] = '123'
console.log(obj) // {"[Object object]" : '123' }

//使用map
let map = new Map()
map.set(obj2, '123') //参数一为属性名内存地址，参数二属性值
map.get(obj2) // '123'
map.set('far', true)
map.size // 2 返回成员个数
map.has(obj2) // true
map.delete('far')
map.clear() //清除所有成员

//遍历map实例方法如下
const map = new Map([
  ['F', 'no'],
  ['T',  'yes'],
]);

for (let key of map.keys()) {
  console.log(key);
}
// "F"
// "T"

for (let value of map.values()) {
  console.log(value);
}
// "no"
// "yes"

for (let item of map.entries()) {
  console.log(item[0], item[1]);
}
// "F" "no"
// "T" "yes"

// 或者for of 解析构值
for (let [key, value] of map.entries()) {
  console.log(key, value);
}
// "F" "no"
// "T" "yes"

// 等同于使用map.entries()
for (let [key, value] of map) {
  console.log(key, value);
}
// "F" "no"
// "T" "yes"

//map转为数组
const myMap = new Map()
  .set(true, 7)
  .set({foo: 3}, ['abc']);
[...myMap] // [ [true,  7], [ {foo:3}, ['abc'] ] ]
```

---
### var-let-const区别
1.变量提升：var定义的变量会变量提升，在声明之前使用，值为undefined
2.暂时性死区：let和const的作用域内在声明之前都不允许使用
3.块级作用域：let和const存在块级作用域
4.重复声明：let和const不允许重复声明
5.变量更改：const变量声明时必须初始化，并且一旦初始化化后所指向的数据内存地址不可更改

### 判断一个属性是否在对象上
1.  Object.prototype.hasOwnProperty() 判断一个对象是否具有实例属性
2.  attr in obj 判断一个属性是否在对象的实例/原型链上
3.  obj[attr] !== undefined 缺点在于无法判断对象属性值为undefined的属性是否存在

### 类型转换机制

1.显示转换：

Number(any): 纯数字字符串、false=>0、true=>1、null=>0、undefined=>NaN,symbol=>error、只有一个值的数组

parseInt(string)：同上，但字符串识别'123abc'为123

String()：任意

Boolean()：null、‘’、+0、-0、undefined转为false

2.隐式转换

转换时间点：比较运算符、布尔判断语句、算数运算符

布尔判断：单个数据类型直接调用Boolean()转为布尔值

算数运算：除了+运算符可能转为字符串，其它都转为数字

比较运算：

    （1）基本数据类型比较 同类型直接比较，不同类型转为数字。
    （2）只有一边是对象比较：先调用对象valueOf()，类型不同再调用toString()
    （3）null和undefined之间比较返回true，只有一边是null/undefined返回false
    （4）NaN和任意比较返回false
    （5）除了在比较对象属性为null或者undefined的情况下，我们可以使用相等操作符（==），其他情况建议一律使用全等操作符（===）

### 浅拷贝和深拷贝
浅拷贝： Object.assign()、array.slice()、array.concat()、[...array]

深拷贝：

（1）调用第三方库，如jQuery.extend()

（2）使用JSON.parse(JSON.stringify(obj)),但是无法拷贝undefined、symbol、函数

（3）循环递归：
```
function deepClone(obj, hash = new WeakMap() ) {
    if(obj === null ) return obj
    if(obj instanceof Date ) return new Date(obj)
    if(obj instanceof RegExp ) return new RegExp(obj)

    if(typeof obj !== 'objec') return obj
    if(hash.get(obj)) return hash.get(obj)

    let cloneObj = new obj.constructor()

    hash.set(obj, cloneObj)

    for(let key in obj) {
        if(obj.hasOwnProperty(key)) {
            cloneObj[key] = deepClone(obj[key], hash)
        }

    }

    return cloneObj
}
```
分析过程：首先第一次调用该函数，hash值为空，因此hash = new WeakMap(),然后经过判断，调用let cloneObj = new obj.constructor()。其中obj为实例化对象，而obj.constructor指向其构造函数，因此这里是指new一个和obj同样原型的实例化对象依次作为返回对象，接着调用hash.set(obj, cloneObj)这一步需要做的是将obj作为键名暂时存到map中，将来通过hash.get(obj)判断是否存在，防止循环引用。接着进入循环，判断obj的属性是否为自身属性，如果是则为cloneObj同名属性赋值，值为调用deepClone()，向里面传递obj的属性和已经创建过了的hash.

第二次调用该函数时，会判断传进来的参数是否是null、date、regexp,还是基础数据类型，如果是直接返回对应数据类型数据，否则参数就是一个对象，然后判断这个obj参数是否已经存在过hash中，如果是，直接从hash中返回这个，紧接着对这个参数obj属性遍历，进行新一轮的递归调用。

直到第一次调用该函数传进去的第一个属性复制完，再回到第一轮遍历传递第二个属性，层层遍历和递归调用，直至所有属性复制完成。


### 闭包
闭包：在一个函数内部使用其它作用域的变量称之为闭包
使用场景：创建私有变量、延长变量的生命周期
缺点：消耗性能，内存泄漏

### 作用域链
作用域：分为全局作用域、局部作用域、块级作用域
作用域链：当在一个作用域中使用一个未再改作用域下定义的变量时，javascript引擎会试图向外层作用域寻找直至全局作用域

### JS继承方式
原型链继承：实例化一个父类对象赋值给子类的原型对象，缺点所有子类共享同一个实例化的父类。

构造函数继承：在子类构造函数内调用父类.call()，缺点无法继承父类原型对象上的属性和方法

组合继承：在子类调用父类.call，同时将父类实例化对象赋值给子类原型对象，再修改子类原型对象上的constructor为指向子类

原型继承：子类= Object.create(父类)实现浅拷贝，然后单独给子类添加属性，缺点引用类型内存地址指向同个地址空间

寄生继承：和原型继承一样，只不过用一个函数封装起来，并且在函数内部为子类添加方法，缺点一样

寄生组合继承：最优继承，所有继承的组合,类似extends
```
function clone(parent, child) {
    child.prototype = Object.create(parent.prototype)
    child.prototype.constructor = child
}

function Parent() {}

function Child() {
    Parent.call(this)
}

clone(Parent, Child)
```
![](./cssImage/extends.png)

### this

概念：this指向函数被调用时所处的执行上下文(运行时绑定)，而箭头函数没有this,this为定义该函数时所在的作用域指向的对象，而不是使用时所在的作用域指向的对象（编译时确定）

绑定方法：默认绑定（全局定义的函数）、隐式绑定（全局定义的函数赋值给对象）、new绑定、显式绑定

优先级：new > 显式 > 隐式 > 默认

### 事件模型

概念：原始事件模型（DOM0事件，onclick之类），标准事件模型（OM2事件，addEventListener），ie事件模型。注：DOM1标准没有定义事件相关的内容

原始事件模型：绑定速度快（可能页面尚未加载完就绑定完事件）,只支持冒泡，并且同类事件只能绑定一次，多次绑定会覆盖。移除方式将事件监听设置为null

标准事件模型：有事件捕获、事件处理、事件冒泡三个过程，支持捕获和冒泡，同类事件可以绑定多个，第三个参数表示是否在捕获阶段执行。

ie事件模型：只有事件处理和事件冒泡阶段，事件绑定方式attachEvent(eventType, fn)，事件移除方式detachEvent(eventType)

### typeof 和 instanceof

概念：typeof判断一个变量的数据类型，返回'number'，'string','symbool','function','boolean','undefined','object'。instanceof判断一个构造函数的prototype属性是否出现在实例化对象的原型链上。

区别：
    1.typeof返回一个字符串类型的值，值表示变量的数据类型，instanceof返回一个布尔值

    2.typeof只能判断基础数据类型（null和function特殊除外），instanceof只能判断复杂数据类型，不能判断基础数据类型。

解决办法：Object.prototype.toString.call()。
```
function getType(obj) {
    let type = typeof obj
    if(type !== 'object') {
        return type
    }

    return Object.prototype.toString.call(obj).replace(/^[Object (\S+)$]/, '$1')
}
```

### 事件代理
概念：将一个元素的响应事件函数委托到另一个元素上。

应用场景：为ul里的li项添加同样的事件函数，可以在ul处添加事件函数，通过event.target.nodeName判断是否为li项。另一个是li项动态增加或减少，如果为每个li项添加，则删除时又要移除事件，而通过事件代理的方式则不用。

注意：focus,blur,mousemove,mouseout不能同事件代理。前者没有冒泡，后者通过定位计算性能消耗过大。

### new操作符

 过程：先创建一个空对象，然后将构造函数的原型对象绑定在新建对象的原型链上，接着调用构造函数并将修改this为新建对象，如果构建函数返回的结果是一个对象则返回该结果，否则直接返回新建对象。

```
function mynew(Func, ...args) {
    let obj = {}
    obj.__proto__ = Func.prototype
    let result = Func.apply(obj, args)
    return result instanceof Object ? result : obj
}
```

### Ajax

原理：通过XMLHttpRequest对象向服务器发送异步请求，从服务器获得数据，然后用js来修改dom。例子：老板通知秘书去找小李报告工作，接着老板做其他事，秘书找到小李后告诉他，小李接到通知后来到老板的办公室，老板处理完事情后听小李报告。秘书就相当于XMLHttpRequest对象

实现过程：

1.新建一个XMLHttpRequest对象

2.使用XHR对象的open(method, url[,async][,name][,password])方法与服务器建立连接

3.使用send(data)方法向服务器发送数据,如果是get方法则不用

4.为xhr对象的onstatuschange添加一个监听函数，xhr.status表示服务器返回的状态，xhr.readyState表示服务器的通信状态（0表示open尚未被调用，1表示open被调用，send尚未调用，2表示send被调用，响应头和响应状态已返回，3表示已经接收到部分数据，4表示数据接收完成）。每当readyState变化时将会调用监听函数。xhr.responseText表示接收到的数据

```
//封装ajax
function ajax(option) {
    let xhr = new XMLHttpRequest()
    option = option || {}
    option.type = (option.type || 'GET').toUpperCase()
    option.dataType = option.dataType || 'json'
    const params = option.data

    if(option.type === 'GET') {
        xhr.open('GET', option.url + '?' + params, true)
        xhr.send(null)
    } else if(option.type === 'POST') {
        xhr.open('POST')
        xhr.send(params)
    }

    xhr.onreadyChanged = function() {
        if(xhr.readyState === 4) {
            let statis = xhr.status
            if(status >= 200 && status < 300) {
                option.success && option.success(xhr.responseText, xht.response)
            } else {
                option.fail && option.fail(status)
            }
        }
    }
}
```

### apply、call、bind

区别：

    相同：都能改变函数运行时的上下文，第一个参数都传递this，如果为空/undefined/null则默认为window

    不同：apply第二个参数传递的是一个数组，call第二个参数传递的是一个参数列表,bind可以多次传递。apply和call会立即执行函数，而bind不会立即执行。


```
//实现一个bind
Function.prototype.bind = function(context) {
    if(typeof this !== 'function') return new TypeError('Error')

    const args = [...arguments].slice(1)
    const fn = this

    return function Fn(){
        return fn.apply(this instanceof Fn ? new fn(...arguments) : context, args)
    }
}
```

### 事件循环

概念：JS为单线程，同一时间只能做一件事。JS里的任务分为同步任务和异步任务，异步任务又分为微任务和宏任务，JS会按代码顺序执行，遇到同步任务立刻执行，遇到异步任务则加入异步队列中，等待同步任务执行完毕，检查异步队列，先执行微任务，每执行完一个微任务都会检查是否有同步任务加入，所有微任务执行完之后执行宏任务，每个宏任务执行完之后会检查是否有同步任务和微任务加入。

分类：

    1.微任务：promise.then、process.nextThick、MutationObsever
    2.宏任务：setTimeout/setInterval、script、postMessage..

async/await:

    async用于声明一个异步方法，返回一个promise对象，而await用于等待异步方法执行，如果await跟着promise对象，则返回该对象的结果，如果是其它值则返回其它值，await会阻塞后面的代码，会将后面的代码加入异步队列中，当await等待的方法执行完之后，await后面的代码不会立刻执行，而是检查外部是否有同步任务，等待同步任务执行完之后才执行。


### BOM

概念：BOM(浏览器对象模型)，是浏览器窗口与页面内容交互的接口对象。

区别于DOM:

    1.DOM是文档对象模型，BOM是浏览器对象模型
    2.DOM的作用是操作DOM元素，BOM是提供页面和浏览器窗口交互的接口
    3.DOM的顶级对象是document, BOM的顶级对象是window。
    4.DOM是W3C定义的标准规范， BOM是浏览器厂商定义的

分类：

1.  window: JS中的window既是全局对象也是BOM的顶级对象，浏览器控制窗口的方法有：

    1.  moveTo(x, y)/moveBy(x, y):moveto表示将窗口移动到距离屏幕（x, y）处，moveBy表示相对窗口移动（x， y）

    2.  resizeTo(x, y)/resizeBy(x, y) ：resizeTo表示将窗口大小缩放到（x, y）,而resizeBy表示相对于窗口缩放(x, y),当x,y为负值时表示缩放。

    3.  scrollTo(w, h)/scrollBy(w, h) ： scrollTo表示滚动条移动到指定宽度和高度，scollBy表示滚动条相对当前位置滚动宽度和高度

    4.  window.open() ：打开一个指定页面并跳转/打开一个新的窗口。

2.  location: 获取url的信息的对象

    1.  location.href :返回完整的URL

    2.  location.protocol: 返回协议

    3.  location.host: 返回域名和端口号

    4.  location.hostname: 返回域名

    5.  location.port: 返回端口号

    6.  location.pathname: 返回文件夹路径

    7.  location.search: 返回参数

    8.  location.hash : 返回地址中#后面的内容

            注释：除了hash外，修改其它属性都会重新加载页面

    9.  location.reload(boolean): 重新加载页面，参数为true时表示忽视缓存重新加载页面，参数为false（默认值）时表示当页面内容没有发生时使用缓存

3.  navigator: 获取浏览器的属性，区分浏览器的类型

    1.  navigator.userAgent:返回用户代理字符串

4.  screen: 获取用户屏幕设备的信息，常见方法如下

    1.  screen.width/height：获取屏幕的宽度/高度

    2.  scrren.top/left: 获取当前窗口距离屏幕顶部/左部的距离

5. history: 获取浏览器URL的历史记录，可以通过参数前后跳转

    1.  history.go(): 参数为整数表示相对于当前地址，整数表示前进几个地址，负数表示后退几个地址，参数为字符串时表示跳转到URL中包含字符串的地址

    2. history.forward()：向前跳转

    3. history.back(): 向后跳转

    4. history.length: 获取历史记录


### 尾递归

概念：尾递归指在函数尾部调用自身，并且下一个函数不需要上一个函数的执行环境，或者说上一个函数不需要保存自身执行环境来处理下一个函数提供的返回值，在最深层的函数中，返回所有函数的最终结果(只递不归)。这样做的好处是弥补了普通递归可能会造成的栈溢出的问题，因此相比较普通递归只多出了一个参数。

实现：在调用下一次函数的时候，将本次函数的结果作为参数传到下一次函数中。

应用场景：

数组求和
```
function sumArray(arr, total) {
    if(arr.length === 1) return total
    return sumArray(arr, arr.pop()+total)
}

sumArray([1,2,3,4,5], 0) // 15
```

数组扁平化

```

function flat(arr = [], result = []) {
    arr.forEach(v => {
        if(Array.isArray(v)) {
            result = result.concat(flat(v, []))
        } else {
            result.push(v)
        }
    })
    return result
}
```

### 内存泄露

概念：所谓的内存泄露是指由于疏忽或者泄漏造成未能正确释放未使用的内存空间。比如为一个数组分配堆内存，并用一个变量指向该内存空间，但是后续程序并没使用这个变量，但是变量指向该内存空间，无法释放该空间。

解决：JS提供了垃圾回收机制，分别是引用计数和标记清除，会周期性的找出不使用的变量并释放其内存

引用计数：JS引擎保存了一张引用表，里面存着所有资源的引用次数，如果引用次数为0，就将这个内存空间释放。缺点在于无法解决两个对象之间相互循环引用。

标记清除（最常用）：当变量进入执行环境的时候会被标记为“进入环境”，进入环境的变量的内存不能被释放，当变量离开执行环境时会被标记为“离开环境”。当垃圾回收程序运行时，会将内存中所有的变量进行标记，接着将执行上下文中所使用到的变量及其被上下文变量引用的变量的标记去除掉。剩下仍存在标记的变量视作不可达，垃圾回收程序将会回收这些变量并释放其内存。

常见情况：函数内部给未定义的变量赋值（严格模式解决）、定时器（clear）、闭包（设置为null解除引用）、DOM的引用（设置为null）


### 浏览器本地存储的方式、区别、应用场景

cookie: 为了识别用户身份而保存在本地的数据，为了解决http协议无状态的问题，特点是4KB，每次发送请求都会携带cookie，因此需要对cookie进行加密，常用属性有exprise/max-age/domain/path/secure，分别表示过期日期，过期剩余秒数，目标主机，请求必须携带path才能携带cookie,是否使用https才能传输。可以直接通过document.cookie = 'key=value'设置。

localStorage(html5新增):本地持久化存储，需手动删除，5M（不同浏览器厂商不同），同域共享，本质是字符串读取。方法有setItem()/getItem()/removeItem()/clear()/key()

sessionStorage:和localStorage差不多一样，但是页面关闭就会自动消失

区别：

存储大小：cookie4KB，sotrage5M或者更大

过期时间：cookie设置的过期时间之前有效，localStorage手动删除前有效，sessionStorage页面关闭前有效

是否发送到服务器：cookie会自动发送到服务器，服务器也可以发送到客户端。localStorage和sessionStorage不会发送到服务器而是本地保存

### 函数式编程

纯函数：相同的输入相同的输出，不能修改全局变量/引用传递的参数

高阶函数：函数作为输入/输出值，只关注结果，存在使用闭包缓存的特性

柯里化：把一个具有多个参数的函数转为一个嵌套的一元函数
```
function curry(fn) {
    return function curriedFn(...args) {
        if(args.length < fn.length) {//判断实参数是否小于形参数
            return function() {
                return curriedFn(...args.concat([...arguments])) //如果小于则传递本次参数并递归
            }
        }

        return fn(...args)
    }
}
```

组合和管道：将多个函数组成一个函数,管道从左到右执行，组合从右到左执行，以下为函数代码
```
const compose = (...fns)=>val=>fns.reverse().reduce((acc, fn)=>fn(acc), val)

const pipe = (...fns)=>val=>fns.reduce((acc, fn)=>fn(acc), val)
```

### 如何实现函数缓存

函数缓存：将函数的结果缓存，本质是用空间换时间

实现方式：闭包+函数柯里化+高阶函数
```
const memorize = function (func, content) {
    const cache = Object.create(null)
    const content = content || this
    return (...keys) => {
        if(!cache[key]) {
            cache[key] = func.apply(content, key) //保存执行结果
        }
        return cache[key]
    }
}
```


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

    4.typeof获取变量的数据类型，注意，typeof null -> 'object'; typeof undefined -> 'undefined';

    5.流程控制语句分为：顺序结构、选择结构、循环结构；

    6.对象：内置对象、宿主对象（BOM/DOM）、自定义对象；

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

## 防抖和节流
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
    let oldTime = new Date()
    return function() {
        let context = this
        let args = aguments
        let newTime = new Date()
        if( newTime - oldTime > delay) {
            oldTime = new Date()
            fn.apply(context, args)
        }
    }
}
```

## Promise

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
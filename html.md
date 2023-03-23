# HTML

标签（空格分隔）： html

---

### HTML5结构
    1.DTD:DOCTYPE文档类型声明，该指令告诉浏览器以哪个版本的html进行编写。通常html5只有一种写法<!DOCTYPE html>，而htnk4.01有三个书写版本
    2. lang: 网页的语言，en表示英语, zh表示中文
    3. head: 网页的配置信息
    4. meta: 元标签，表示网页的基础配置
    5. charset:字符集，告诉文档的编码方式
    6. title: 网页的标题，显示在浏览器的标题栏上，同时也是SEO搜索的结果
    7. keywords： 网页的关键词
    8. description: 网页的描述
    9. X-UA-Compatible：告诉IE8采用何种IE版本去渲染页面
    10. Edge: 告诉IE以最高模式渲染文档

### html5新增语义化标签
1. header
2. nav
3. footer
4. aside
5. section
6. article

### h5新增表单控件
color、date、range、file、search、url、time、datalist
### 特殊的内联元素（块级、内敛元素本质都是浏览器内核display初始化决定的）
    一般情况下，内联元素无法设置宽高，由内容撑开，但是以下内联元素特殊可以设置宽高同时不独占一行,亦称行内块元素图片、音频、表单输入和下拉及文本框都是
    img（图片）、audio、video（音频）、**input（表单输入）** 、select（表单下拉框和选项）、 option、textarea（表单文本框）
### 标签细节点

1.p标签为文本级标签的块级元素，里面只能放文本、图片、表单元素
2.a标签为文本级标签的内联元素，里面只能放文本、图片，href属性为javascript:;页面不会跳转，href属性可以为空，表示一个占位符，可以是绝对路径/相对路径/锚点。target属性可以是_blank、_self、iframenaem等，rel属性可以有多个值用“ ”分隔；
3.img标签中的align属性表示的是图片与环绕文字的相对位置，a标签为行内块元素，也可以算上行内元素
4.caption标签,表格的标题。使用时和tr标签并列
5.行内元素不允许设置宽高、margin和padding的top和bottom、border
6.button标签必须指明type
7.ul中只有li标签时```</li>```可以省略
8.如果一个标签有id属性，那么这个id属性名将会称为window对象的属性值

### 设置和获取标签的class

```
    var node  = document.querySelector('xxx');
    node.classList.add('active');
    node.classList.remove('active');
    node.classList.taggle('active');//有则移除，无则添加
    node.classList.contains('active');//检测是否含有该class
```

### href和src的区别
>href表示超文本引用，指向目标网络资源，在浏览器遇到href时会边解析文档边并行下载该资源，这就是css用link加载而不用@import的原因。
src表示将**资源下载到html页面并替换**，在浏览器遇到src时，**会下载该资源/加载脚本并同时阻止其他资源的下载或处理**，直至下载完该资源（因为script可能会重新构建DOM树，这就是scirpt放在页面底部的原因）。

例如：你要到超市买东西，正好需要很多商品，碰到两个导购员，link导购员会告诉你要的东西在哪里，你可以边逛超市边看其它商品，然后帮你拿过来，而src导购员也会告诉你，但是你要自己去拿，拿到这个商品后才能继续区看其他商品

### input和datalits

```
/*实现方式：input通过属性list值为datalist的id，从而实现两者的绑定
  效果：实现输入框搜索下拉框选项
*/
<input type="text" list="data">
<datalist id="data">
    <option >研究生</option>
    <option >本科生</option>
    <option>专科生</option>
    <option >高中学历</option>
</datalist>
```

![效果如下][1]

  [1]: http://img.smyhvae.com/20180206_1845.gif

### 播放音频的兼容性写法

```
/* controls属性表示用户可控制播放，loop表示循环*/
<audio controls loop>
    <source src=""/>
    <source src=""/>
    <source src=""/>
    无法时会弹出这句话
</audio>
```

### 获取用户位置
> 浏览器中有个navigator对象，通过这个对象的属性和方法可以获取到用户的位置，浏览器可能会通过ip地址，网关等多种方式获取，在获取位置时浏览器通常要经过用户的同意才能获取位置。常见的方法有： **navigator.getLocation.getCurrentPosition(successBack, errorBack, other);**
当获取用户位置成功返回**successBack函数**，失败则返回**errorBack函数**，而**successBack(position){}**里的**position**参数保存了用户的位置，经度 = **position.cords.altitude**,维度 = **position.cords.latitude**

```
/*获取用户位置API*/
function getLocation() {
    if(navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(
            (position) => {
                console.log(position.coords.altitude, position.coords.latitude);
                return {
                    "altitude":position.coords.altitude,
                    "latitude":position.coords.latitude
                };
            },
            (err) => {
                return "获取用户位置失败"
            }
        );
    }else{
        return "用户拒绝获取位置信息";
    }
}
```

例如：你是一个爱豆，有个叫navigator的粉丝非常喜欢你，无论你去哪里都会跟着你，但是他很有礼貌，每次跟着你之前，他会问（getlocation）你我可以跟着你吗，经过你的同意之后,他这两天就会记录你去过的地方（getCurrentPosition）,如果没跟丢你，他就在粉丝群（successBack）里把你去过的地点都写在了一张表（position）上有一行（cords）,上面有两列写着你的经度和纬度，如果跟丢了，他就淡淡在另一个粉丝errorBack群里告诉大家：我跟丢了。

----------

### 网页结构

```
<!DOCTYPE html> //文档类型声明DTD，告诉浏览器文档类型是什么，以何种协议解析文档，html4.01有严格版和传统版，传统版包括展示性和废弃的元素和属性。此非标签而是指令
<head>//网页的配置
    <meata charset="utf-8"/>//元标签,告诉浏览器采用utf-8的字符集，一个汉字3个字节
    <meta http-equiv="X-UA-Compatible" content="IE=edg"> //IE下的特定标签，告诉IE浏览器以当前版本最高级模式渲染页面
    <title></title> //网页的标题，显示在窗口上方和SEO搜索出来的结果
    <meta name="keywords" content=""> //网页的关键词显示在搜索中
    <meta name="description" content=""> //网页的描述信息，显示在搜索中
    <meta name="viewport" content="width=device-width;intial-scale=1.0"> //移动端适配，宽度为设备实际宽度，缩放比例为1：1，即不缩放
</head>
```

### 标签
>1.h1标签：一个页面只能有一个h1标签，通常首页logo用h1标签制作。列表页中h1为分类名称,h2为子分类名称，h3为页面标题链接。
2.img : src的本质是引入图片而不是插入图片； height/width只设置一个属性将会按比例缩放，pc端必须写src,width,height,alt四个属性；
3.a : 在html5中```href="#top"```就可以回到页面顶部,如果是文件格式则变为下载链接。如果是邮件则为 ```href=mailto:791469353@qq.com```自动打开本地软件发送邮件。如果是电话则为```href=tel:123456789```自动打开手机拨号

4. audio/video: controls是否显示控制，loop循环播放，auto自动播放，src=“”音频/视频文件源，当无法识别src时将会显示audio标签中的文字内容，
5. table: tr>th/td , caption为表格标题，应该为table的第一个元素，td属性colspan/rowspan表示当前单元格占据多少个行/列，一般thead>tr>th, tbody>tr>td, tfoot>tr>td

### HTML5特性
1.空白折叠现象：文字之间的空格/换行会被折叠成一个空格，标签内壁和文字之间的空格会被忽略；
2.新标签：ul/ol/dl 、mark高亮文本标签
    >ol属性：**type="1"/"A"/"a"/"I"/"i"分别表示有序列表序号表示方式；**
            **start="起始序号"，如start="2"；
            reversed,html5新属性，是否倒序显示；**
    dl说明：
    >
    ```<dl>
        <dt>数据项头1</dt>
        <dd>数据项说明1:1</dd>
        <dd>数据项说明1:2</dd>
        <dd>数据项说明1:2</dd>
        <dt>数据项头2</dt>
        <dd>数据项说明2:1</dd>
        <dd>数据项说明2:2</dd>
        <dd>数据项说明2:2</dd>
    </dl>```

### @import和link的区别
相同 :都是引入css样式的方式
1.不同：
    1.从属关系：@import是css提供的语法规则，只能用于引入css。而link是XHTML提供的标签，不仅能够引入CSS，还能定义RSS等。
    2.加载顺序：@import必须等到html页面加载完毕后才能加载，而且是同步加载，必须要等到加载解析完才能渲染，因为@import提供了一个url用于连接外部样式，而不是直接在样式表中的。而link是异步加载，在引入css的同时和html一同边加载边解析渲染。如果存在多个css文件，css文件会同时加载，先完成加载的先解析
    3.兼容性：@import只有ie5+以上版本才支持，而link属于html标签没有兼容性问题
    4.dom操作：JS可以通过操作dom的方式，在head标签中引入link标签，但是不能在js中操作@import引入样式。
    5.覆盖问题：两者之间存在根据选择器而导致的样式覆盖问题,非权重，权威指南中指出import只适合放在样式表的最前面，**在同一样式表中import放在末尾会被浏览器忽视**。
    >@import引入css的弊端：1.引发阻塞，必须等待css文件加载完成才能加载其它css文件；
    2. IE 中，@import 会引发资源文件的下载顺序被打乱，排列在 @import 后面的 js 文件先于 @import下 载，并且打乱甚至破坏 @import 自身的并行下载。

### script中defer和async的区别

定义

1. defer：此布尔属性被设置为向浏览器指示脚本在文档被解析后执行。
2. async：设置此布尔属性，以指示浏览器如果可能的话，应异步执行脚本。

>对于defer，我们可以认为是将外链的js放在了页面底部。js的加载不会阻塞页面的渲染和资源的加载。不过defer会按照原本的js的顺序执行，所以如果前后有依赖关系的js可以放心使用。

>对于async，这个是html5中新增的属性，它的作用是能够异步的加载和执行脚本，不因为加载脚本而阻塞页面的加载。一旦加载到就会立刻执行在有async的情况下，js一旦下载好了就会执行，所以很有可能不是按照原本的顺序来执行的。如果js前后有依赖性，用async，就很有可能出错。

3. 相同点：
加载文件时不阻塞页面渲染;
使用这两个属性的脚本中不能调用document.write方法;
有脚本的onload的事件回调;

4. 不同点：
    html的版本html4.0中定义了defer；html5.0中定义了async浏览器兼容性

>执行时刻每一个async属性的脚本都在它下载结束之后立刻执行，同时会在window的load事件之前执行。所以就有可能出现脚本执行顺序被打乱的情况；每一个defer属性的脚本都是在页面解析完毕之后，按照原本的顺序执行，同时会在document的DOMContentLoaded之前执行。

同时使用这两个属性会有三种可能的情况：
>如果async为true，那么脚本在下载完成后异步执行。

>如果async为false，defer为true，那么脚本会在页面解析完毕之后执行。

>如果async和defer都为false，那么脚本会在页面解析中，停止页面解析，立刻下载并且执行。

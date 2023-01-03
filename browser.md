# 浏览器

标签（空格分隔）： 浏览器

---

## SSR和CSR（服务端渲染和客户端渲染）
### SSR：Server side render
将组件或页面通过服务器生成html字符串，再发送到浏览器，简单理解下来，发了一个请求，服务器返回的不是接口数据，而是一整个页面的HTML结构，再结合界面之前定义的CSS把页面展示出来；VUE 服务器渲染文档

SSR优点:

1. 例如SEO–因为访问一个请求，返回的就是页面全部的HTML结构，包含所需要呈现的所有数据，于是例如搜索引擎或者爬虫的数据抓取；
2. 目前使用MV*架构的项目，大都是前后端分离，数据都是动态生成，不利于SEO优化;
3. 利于首屏渲染性能高–首屏的页面加载来自于服务器，不依赖与服务端的接口请求再数据处理；

SSR缺点:

1. 性能全都依赖于服务器;
2. 前端界面开发可操作性不高

### CSR：Client side render
通过接口请求数据，前端通过JS动态处理和生成页面需要的结构和页面展示
CSR 优点:

1. FP最快;
2. 客户端体验较好，因为在数据没更新之前，页面框架和元素是可以在dom生成的

>在CSR的FP术语之间，和FP相类似的术语还有：FCP和FMP；
FP：仅有一个 div 根节点。以VUE为例，div#app 注册一个空的div
FCP：包含页面的基本框架，但没有数据内容。以VUE为例，每个template中的div框架，对应VUE生命周期的mounted
FMP：包含页面所有元素及数据。以VUE为例，通过接口更新到页面的数据后完整的页面展示；对应VUE的生命周期中的updated

CSR的缺点:

1. 不利于SEO–爬虫数据不好爬呀~~;
2. 整体加载完速度慢;

## 优化首屏加载，减少白屏时间，提升加载性能

1. 加速或减少HTTP请求损耗：使用CDN加载公用库，使用强缓存和协商缓存，使用域名收敛，小图片使用Base64代替，使用Get请求代替Post请求，设置Access-Control-Max-Age减少预检请求，页面内跳转其他域名或请求其他域名的资源时使用浏览器prefetch预解析等；
2. 延迟加载：非重要的库、非首屏图片延迟加载，SPA的组件懒加载等；
3. 减少请求内容的体积：开启服务器Gzip压缩，JS、CSS文件压缩合并，减少cookies大小，SSR直接输出渲染后的HTML等；
4. 浏览器渲染原理：优化关键渲染路径，尽可能减少阻塞渲染的JS、CSS；
5. 优化用户等待体验：白屏使用加载进度条、loading图、骨架屏代替等；

>以上优化方案的中的技术术语
1.强缓存和协商缓存:
**强缓存**:是利用http头中的Expires和Cache-Control两个字段来控制的，用来表示资源的缓存时间
**协商缓存**：浏览器第一次请求一个资源的时候，服务器返回的header中会加上Last-Modify，Last-modify是一个时间标识该资源的最后修改时间；当浏览器再次请求该资源时，request的请求头中会包含If-Modify-Since，该值为缓存之前返回的Last-Modify。服务器收到If-Modify-Since后，根据资源的最后修改时间判断是否命中缓存；其中Etag：web服务器响应请求时，告诉浏览器当前资源在服务器的唯一标识
Access-Control-Max-Age：缓存可以被缓存的时间。
2.DNS 预解析：浏览器会在加载网页时对网页中的域名进行解析缓存，这样在你单击当前网页中的连接时就无需进行DNS的解析，减少用户等待时间，提高用户体验。```<link rel="dns-prefetch" href="www.ytuwlg.iteye.com" />```
3.Gzip页面压缩，像服务器发送压缩文件，同时服务器需要设置解析

### 模块化规范

1.服务器端规范：CommonJS规范：是 Node.js 使用的模块化规范。
2.浏览器端规范：
    （1）AMD规范：

```
- 异步加载模块；

- 依赖前置、提前执行：require([`foo`,`bar`],function(foo,bar){});   //也就是说把所有的包都 require 成功，再继续执行代码。

- define 定义模块：define([`require`,`foo`],function(){return});
```

    (2)CMD规范:

```
- 同步加载模块；

- 依赖就近，延迟执行：require(./a) 直接引入。或者Require.async 异步引入。   //依赖就近：执行到这一部分的时候，再去加载对应的文件。

- define 定义模块， export 导出：define(function(require, export, module){});
```

### HTTP

1. 特点：
（1）简单：访问图片等资源直接使用url即可；
（2）灵活：通过请求头的数据类型，可以请求不同类型的数据；
（3）无连接：一次请求后就与服务器断开链接；
（4）无状态：服务器端接收到http请求并处理完后不会记录请求信息的身份，下次请求需要重新验证身份；

2. 组成：
（1）分为请求报文和响应报文。
        （2）请求报文包含请求行，请求头，空行，请求体；其中请求行有请求方法get/post,请求的url，http协议和版本；请求头包含请求的数据类型，缓存等配置；空行表示下一个就是请求体；请求体里面就请求附带的数据；
        （3）响应报文包含状态行，响应头，空行，响应体；其中状态行包含状态，http协议和版本；响应头表示返回数据类型等；响应体包含了从服务端返回的请求数据；

3. http方法：get请求资源，post传输资源，put更新资源，delete删除资源，head获取报文首部。

    get和post的主要区别：
（1）浏览器回退时，get方法不会再次请求，而post会再次请求；（性能-请求次数）
（2）get方法浏览器会主动缓存，而post不会；(性能-缓存)
（3）get方法请求的参数会直接暴露在url中，而post包含在请求体中；（安全）
（4）get方法请求的参数会保存在浏览器的历史记录中，而post不会；（安全）
（5）get方法参数一般限制2KB,而post不会；(大小)

4.常见的状态码：
（1）1xx:表示服务端已接收请求，正在处理；
（2）2xx:表示服务端已处理完毕请求；
（3）3xx:表示请求需要进一步处理，页面需要重定向；
（4）4xx:表示客户端请求不合法；
（5）5xx:表示服务端出现无法处理合法请求的错误；
    详情：略

5.持久链接/长链接：在请求头里设置Connection:keep-alive，表示客户端只请求一次，服务端保持连接，虽然不同再次链接，但是每次请求仍然要发送header；

### Cookie

1. 特点：Cookie每次都会随浏览器请求发送到服务器；且最大数据限制为4KB。Cookie在浏览器关闭后自动清除，如果单个域名下的cookie超出限制后，以前的cookie就会被清除
2. 属性：
    （1）Name & value : 纯英文```document.cookie="name=zhangsan";```    中文：```document.cookie=‘name=${encodeURIComponent('张三')}’```
    （2）exprise:cookie失效时间，值为Date类型；
    （3）max-age:cookie失效时间，值为数字，单位为秒；
    （4）domain:cookie可以访问的域名；
    （5）path:cookie可以访问的路径；
    （6）httpOnly:禁止js访问；
    （7）secure:只有https可以发送到服务器端

### sessionStorage/localStorage

1. 区别：都能存储数据，不随http请求发送到服务器端。但是sessionStorage在关闭浏览器后清除，而localStorage保存在本地，只能手动清除
2. 方法：setitem(name, value);getItem(name);removeItem(name);clear();length;

### Ajax

```
/*原生Ajax*/
function getAjax(methods, url) {
    var xhr = new XMLHttpRequest() //构造函数,ie为new window.ActiveXObject('Microsoft.XMLHTTP');

    xhr.open(methods, url, true) //设置请求类型，请求地址,是否异步
    xhr.send() //发送请求
    xhr.onreadystatechange = function(){ //监听readystate变化
        if(xhr.readyState == 4 && xhr.status == 200) {
        //readyState:0未初始化，1调用open()，2调用send(),3接收到部分响应数据，4接收到全部响应数据。
        //status(200-2999) or 304 均表示请求成功
            console.log(xhr.responseText) //相应数据保存在responseText中，类型为JSON字符串
        }
    }

    /*
        xhr.setRequestHeader(name, value) //设置请求头信息，一般用于告诉服务器，浏览器发送的数据格式
        xhr.setRequestHeader('Content-type', 'application/x-www-form-urlencoded') //表单默认的提交数据格式
        xhr.setRequestHeader('Content-type', 'application/json') //json数据格式
    */
}
```

3.事件触发的条件
<img src="http://img.smyhvae.com/20180307_1443.png">
readystatechange:xhr.readystate改变时触发
loadstart:顾名思义，在load之前触发，而load是在请求成功并完成时触发，因此loadstart应该在send()之后**立即触发**。
load:请求成功后触发；
loadend:请求结束后触发；
abort:调用xhr.abor()请求中断触发；
timeout:请求超时触发；
error:网络层出错上传未完成会触发xhr.upload.onerror然后触发xhr.onerror。如果上传完成只触发xhr.onerror；
progress:上传阶段：send()之后，readystate=2之前每隔50ms触发一次。下载阶段：readystate=3时每隔50ms触发一次

4.事件触发的顺序
<img src="http://img.smyhvae.com/20180307_1445.png">
正常执行顺序：loadstart-(上传阶段：upload.onloadstart-upload.onprogress-upload.onload-upload.onloadend)-（下载阶段）progress-load-loadend

### 跨域解决方案

1. CORS:本质也是一种ajax,能够跨域资源共享，需要后端配合设置 Access-Control-Allow-Origin 字段，允许指定的域名跨域请求数据;如跨域时，浏览器会拦截Ajax请求，并在http头中加Origin。

```
fetch(url,options).then(function(res){}).catch(function(err){})
```

2. JSONP:利用浏览器不限制 script 标签跨域的特性，通过 script 标签的 src 属性加载跨域文件；
3. Websocket：一种网络协议，以往只能客户端向服务端发送请求，服务器不能主动向客户端发送信息，现在websocket采用双工通信的方式实现双向请求。websocket建立在 TCP 协议之上，握手阶段采用HTTP协议，可以发送文本和二进制，没有同源限制。
4. Hash：url的#后面的内容就叫Hash。Hash的改变，页面不会刷新。实现方法就是获取iframe节点修改其src属性，添加上#后面的内容，然后在另一个页面中监听window.onhashchange事件就能获取到window.location.hash；
5. postMessage：h5新方法，利用window.postMessage(data, url)来发送信息，window.onmessage(function(event){})接收信息；
6. ngix代理：

### Axios

1. 说明：基于Promise的HTTP库（网络请求库）
2. 使用步骤：
（1）安装axios ```npm install axios```
（2）创建axios实例，设置拦截器，相应请求器，发送真正的请求

```
    import axios from 'axios'
    export function request(config) {
        const service = axios.create({
            baseUrl : process.env.BASE_URL, //请求地址
            timeout : 500
        })

        service.interceptors.request.use(config=>{
            //验证config/token等信息
            return config
        },err=>{
            return err
        })

        service.interceptors.response.use(res=>{
            //响应拦截
            return res.data
        },err=>{
            return err
        })

        return service(config) //发送真正的请求
    }

    //config 为{ url : '', method : post/get }
```

## 安全问题
### CSRF

1. Cross-site-request-forgery:跨站请求伪造
2. 原理：用户登录了网站A->网站A验证用户信息并返回cookie->用户在未退出网站A登陆的情况下访问危险网站B->网站B请求用户访问网站A,并发出一个request->网站A收到了用户的请求并返回了相关信息；
举例子：网站B就是一个骗子，利用用户在获得网站A的信任让用户去网站A取自己想要东西，然后给他
3. 防御措施：token验证、隐藏令牌（token携带在http头中）、Referer验证（服务器指定站点请求）

### XSS

1. cross-site-script:跨域脚本攻击
2. 原理：不需要验证登录，直接在url、评论框等输入方式向页面注入脚本（js,html）
3. 结果：盗取cookie、破坏页面结构插入广告、D-dos攻击
4. 类型：
（1）反射型XSS:通过url，作为输入提交到服务器，服务器解析后响应，XSS代码随着响应数据返回给浏览器，浏览器解析执行XSS代码
（2）存储型XSS:会将XSS代码存储在服务器，下次请求时不用携带XSS代码
5. 防御措施：
（1）编码：对用户输入的数据进行编码，如对一些字符进行转义；
（2）过滤：
        a.移除用户输入节点style、iframe、script;
        b.移除用户输入的属性避免直接对HTML Entity进行解码。使用DOM Parse转换，校正不配对的DOM标签。

## 区别：

1. CSRF需要Cookie验证，XSS不需要
2. CSRF是利用网站的漏洞，请求网站的API,而XSS是向网站注入脚本，执行里面的JS代码，篡改页面内容

## 排序
### 快排
<http://data.biancheng.net/view/117.html>

## 异步和运行机制
1.同步和异步：简单来说就是同步任务先执行，异步任务某一刻后执行。
2.应用场景：延时定时器、循环定时器、promise、ajax请求、事件绑定、图片懒加载
3.运行机制：JS是单线程，同一时间内只能做一件事，JS将需要执行的任务分为了同步任务和异步任务，其中异步任务又分为了宏任务（setTimeout/setInterval）和微任务(Promise)。在解析执行这些任务时，根据事件循环机制，遇到同步任务时加入到主线程任务中排队，而异步任务会挂载到任务队列中，等待主线程同步任务全部完成再将异步任务加入到主线程中执行。

## 页面性能优化
### 一.资源优化

1. 多张图片合成一张图片，css和js文件合并，css和js文件压缩
2. 图片使用懒加载

### 二.非核心代码异步加载

1. 动态生成script标签
2. script标签加上defer或者async，defer和async都表示异步加载js文件，因为js的加载会阻塞dom的解析和渲染。defer和async的一个最大区别在于，defer会按照顺序加载js，而async的加载顺序是不固定的。

### 三.使用缓存
缓存：将用户请求的资源存储在本地，再次请求相同资源时使用本地缓存
分类：

1. 强缓存：不需要和服务器协商，直接使用本地缓存
    原理：利用http请求的请求头中的expires和cache-control字段来对比缓存时间是否过期是否使用缓存。
    步骤：（1）expires:服务器在用户第一次请求资源后会在响应头携带expires字段，expirse的值是服务器的绝对时间，表示资源过期时间。当用户再次请求相同资源时，会在缓存找到这个资源，判断当前时间是否在expires规定的时间之内，如果在就命中缓存。这种做法的缺点在于客户端的时间是可以修改的，会影响命中结果。
        （2）cache-control:由于expires的缺点，cache-control返回的时间的服务器的相对时间，在接下来的相对时间之内都可以使用本地缓存。
        （3）expirse和cache-control可以同时使用，但是cache-control的优先级更高

2. 协商缓存：需要和服务器协商资源是否发生变化，是否使用本地缓存
    原理：利用请求头中的last-modified和if-modified-since字段以及etag和if-none-match字段。
    步骤：(1)last-modified和if-modified-since：当用户第一次请求服务器资源时，服务器会在响应头中返回last-modified字段表示上次资源修改的时间。用户再次请求相同的资源时会将last-modified存在请求头的if-modified-since字段中，服务器接收到请求后会验证资源是否发生变化，没发生变化就返回304，发生变化就当作第一次访问资源的操作。缺点在于1s内修改两次资源，但last-modified字段可能还没更新。
        （2）etag和if-none-match：当用户第一次访问资源时，服务器会根据资源的内容生成唯一标识，并设置在响应头中的etag，再次请求相同的资源时，将etag中的值存在请求头中if-none-match，服务器会将它和资源的etag对比是否发生变化，如果没发生变化就返回304，否则返回资源和新的etag。

### 四、使用CDN
目的：在用户第一次请求资源时无法使用缓存
CDN:content-dispatch-network(内容分发网络)
原理：
使用方法：

### 五、使用DNS预解析
DNS预解析：表示客户端可能在将来的某个时间访问某个url的资源，在此之前尽快完成对该URL的DNS解析
使用方法：```<meta http-equiv="X-dns-prefetch-control" content="on"></meta>```
        表示强制使用dns预解析，大多数浏览器对a标签进行预解析，但是在https协议中，预解析将会被关闭，以上代码为强制使用预解析。

## 质量保证/错误检测
1.分类：运行时代码错误和资源加载错误

2.运行错误：

    （1）try...catch

    （2）使用window.onerror = function(){}

   > 注意：如果想检测跨域文件的错误在引入的文件中添加Access-Contril-Allow-Origin或者crossorigin属性。再用console.trace()打印堆栈信息

3.资源加载错误：

    （1）通过为img、script标签添加onerror事件

    （2）通过performance.getEntries()获取已成功加载的资源和通过document.getElementsByTagName获取的DOM标签对比即可

    （3）通过window.addEventListener事件采取捕获方式

4.错误上报方式：Ajax通信或者(new image()).scr = 'url'，其中url为错误上报的路径

### http和https的理解，区别
http:超文本传输协议，除了能传输文本之外，还能传输图片、视频等文件，传输时对数据进行明文传输，不加密

特点：

    （1）简单快速，http请求只需发生请求地址和请求方法，因此http协议体积小，传输速度快

    （2）灵活：支持传输任意类型的数据，通过请求头content-type字段设置

    （3）无连接：每次连接只处理一次请求，请求完之后立刻断开链接

    （4）无状态：不能根据上一次请求的结果处理新的请求

https:在http协议的基础增加了ssl/tsl安全认证，对传输的数据进行加密。首先客户端输入url发出http请求，服务端返回ssl安全证书和公钥，客户端认证通过后产生一个随机会话密钥，然后用服务端发送过来的公钥进行加密，再发送回给服务端，服务端使用私钥解密获得会话密钥，两者通过会话密钥传输数据。

区别：https更安全，使用的端口是443，http端口是80，https需要多次握手，性能不如http,https需要购买安全认证证书，要钱

### https如何保证安全

1.https是什么：http + (ssl/tsl)，是http和tcp层之间的协议，在这一层协议之上，能够实现对数据加密、完整性校验、服务器身份验证；

2.https如何实现：

数据加密：

    （1）对称加密：客户端用密钥对数据进行加密，服务端对加密后的数据再用密钥解密

    （2）非对称加密：客户端用公钥加密，服务器用私钥解密

    （3）混合加密（https采纳）：当客户端访问服务端，服务端向客户端发送证书和公钥，客户端对证书验证，然后生成对称密钥，再用公钥对对称密钥加密，然后发送到服务器，服务器用私钥解密获得对称密钥

摘要算法：解决完整性校验

    通过散列函数、哈希函数，对数据进行压缩生成一个独一无二的摘要字串，然后附加在原文后面，服务器拿到原文和摘要后也对原文计算获得摘要，就能验证数据是否被篡改。

数字签名：解决身份验证，确认消息由发送发签名并发送出来的。

    客户端将生成的摘要用私钥加密生成数字签名，然后跟随原文一起发送给服务端，服务端用与私钥配对的公钥进行解密获得摘要。

    如何判断公钥是对应的私钥：CA验证机构。

### 浏览器事件循环

1.  前提：
    1.  浏览器进程模型：浏览器是一个多进程多线程的应用程序，进程主要包括浏览器进程、网络进程、渲染进程等，各个进程之间不会互相干扰，比如网络进程崩坏时，如果资源已经下载完毕并不会影响网页的渲染。浏览器进程负责浏览器界面的展示、用户交互、子进程管理等。每个标签页都会开启一个渲染进程，每个渲染进程都会开启一个渲染主线程，渲染主线程负责执行html、css、js的代码。

    2.  渲染主线程：负责解析html、css并构建dom树、css树，计算样式、布局、处理图层，每秒把页面画60次、执行JS代码等，渲染主线程一次只能执行一个任务，任何新增的后续任务只能排队，每当渲染主线程执行完一个任务时，会到消息队列（事件队列）里取出一个新任务，如果没有新任务则休息。这个过程发生在一个无限循环当中，因此称之为事件循环。

2.  异步：无法立即执行的任务称之为异步任务。js是一门单线程的语言，运行在渲染主线程中，而每个页面只有一个渲染主线程，为了防止阻塞渲染主线程，需要异步执行这些任务，异步任务例如：计时任务setTimeout、setInterval,网络通信完成后执行的任务fetch、XHR,用户操作完成之后执行的任务addEventListener。异步执行：对于计时任务，当渲染主线程遇到时将会启用计时线程计时，同时继续执行下一个任务，当计时线程计时完毕将该计时任务的回调函数包装成任务放入消息队列末尾中等待主线程调度执行。形象地说，在主线程遇到计时任务后延迟将执行代码放入消息队列中。

3.  js阻塞渲染：js的执行影响到dom时，将会导致产生一个新的绘制任务，这个绘制任务将会在js执行完毕之后才会执行，页面才会更新。因此当js执行时，即使修改dom的代码执行完毕，只要当前js任务后续的js代码未执行完毕，后续的绘制任务就不会执行，视图就不会更新。

4.  消息队列的优先级：消息队列分为多种类型，同一种任务放在同一个队列。一般至少有延时队列、交互队列、微队列，优先级依次升高。

5.  小结：浏览器是一个多进程多线程的应用程序，主要有浏览器进程、网络进程、渲染进程，浏览器会给每个页面开启一个渲染进程，每个渲染进程只有一个渲染主线程，渲染主线程主要负责解析html、css,计算样式，布局，绘制图层，执行js代码等。由于js执行在渲染主线程中，每个页面只有一个渲染主线程，因此js是单线程语言。在单线程中，为了合理的调度各种任务而不阻塞，浏览器渲染主线程采取异步的方式执行任务。在一个无限循环中，渲染主线程一次只执行一个任务，其他任务放在消息队列中排队，当渲染主线程执行完毕就会到消息队列中取出任务并执行，而消息队列有多种，大致可分为微任务队列、交互任务队列、延时任务队列，消息队列优先级依次降低。

6.  阐述一下js的事件循环 ：事件循环又叫消息循环，是浏览器渲染主线程的工作方式，在Chrome的源码中，它开启一个无限for循环，每次循环从消息队列中取出第一个任务执行，而其他线程志需要在合适的将任务加入到队列末尾即可。+消息队列优先级解释

7.  浏览器能否做到精确计时：不能，计算机硬件没有原子钟、JS计时器依赖的OS的计时函数有偏差、W3C规定浏览器实现计时器时嵌套层级超过5层有4ms的延迟、计时器回调函数只能在主线程空闲时执行而非时间到即执行
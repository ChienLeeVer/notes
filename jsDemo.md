# JS事例

标签（空格分隔）： js案例

---

### 放大图片

```
<!DOCTYPE html>
<html>
<head lang="en">
    <meta charset="UTF-8">
    <title></title>
    <style>
        * {
            margin: 0;
            padding: 0;
        }

        .box {
            width: 350px;
            height: 350px;
            margin: 100px;
            border: 1px solid #ccc;
            position: relative;
        }

        .big {
            width: 400px;
            height: 400px;
            position: absolute;
            top: 0;
            left: 360px;
            border: 1px solid #ccc;
            overflow: hidden;
            display: none;
        }

        /*mask的中文是：遮罩*/
        .mask {
            width: 175px;
            height: 175px;
            background: rgba(255, 255, 0, 0.4);
            position: absolute;
            top: 0;
            left: 0;
            cursor: move;
            display: none;
        }

        .small {
            position: relative;
        }

        img {
            vertical-align: top;
        }
    </style>

    <script src="tools.js"></script>
    <script>
        window.onload = function () {
            //需求：鼠标放到小盒子上，让大盒子里面的图片和我们同步等比例移动。
            //技术点：onmouseenter==onmouseover 第一个不冒泡
            //技术点：onmouseleave==onmouseout  第一个不冒泡
            //步骤：
            //1.鼠标放上去显示盒子，移开隐藏盒子。
            //2.老三步和新五步（黄盒子跟随移动）
            //3.右侧的大图片，等比例移动。

            //0.获取相关元素
            var box = document.getElementsByClassName("box")[0];
            var small = box.firstElementChild || box.firstChild;
            var big = box.children[1];
            var mask = small.children[1];
            var bigImg = big.children[0];

            //1.鼠标放上去显示盒子，移开隐藏盒子。(为小盒子绑定事件)
            small.onmouseenter = function () {
                //封装好方法调用：显示元素
                show(mask);
                show(big);
            }
            small.onmouseleave = function () {
                //封装好方法调用：隐藏元素
                hide(mask);
                hide(big);
            }

            //2.老三步和新五步（黄盒子跟随移动）
            //绑定的事件是onmousemove，而事件源是small(只要在小盒子上移动1像素，黄盒子也要跟随)
            small.onmousemove = function (event) {
                //新五步
                event = event || window.event;

                //想要移动黄盒子，必须要知道鼠标在small小图中的位置。
                var pagex = event.pageX || scroll().left + event.clientX;
                var pagey = event.pageY || scroll().top + event.clientY;

                //x：mask的left值，y：mask的top值。
                var x = pagex - box.offsetLeft - mask.offsetWidth / 2; //除以2，可以保证鼠标mask的中间
                var y = pagey - box.offsetTop - mask.offsetHeight / 2;

                //限制换盒子的范围
                //left取值为大于0，小盒子的宽-mask的宽。
                if (x < 0) {
                    x = 0;
                }
                if (x > small.offsetWidth - mask.offsetWidth) {
                    x = small.offsetWidth - mask.offsetWidth;
                }
                //top同理。
                if (y < 0) {
                    y = 0;
                }
                if (y > small.offsetHeight - mask.offsetHeight) {
                    y = small.offsetHeight - mask.offsetHeight;
                }

                //移动黄盒子
                console.log(small.offsetHeight);
                mask.style.left = x + "px";
                mask.style.top = y + "px";

                //3.右侧的大图片，等比例移动。
                //如何移动大图片？等比例移动。
                //    大图片/大盒子 = 小图片/mask盒子
                //    大图片走的距离/mask走的距离 = （大图片-大盒子）/（小图片-黄盒子）
//                var bili = (bigImg.offsetWidth-big.offsetWidth)/(small.offsetWidth-mask.offsetWidth);

                //大图片走的距离/mask盒子都的距离 = 大图片/小图片
                var bili = bigImg.offsetWidth / small.offsetWidth;

                var xx = bili * x;  //知道比例，就可以移动大图片了
                var yy = bili * y;

                bigImg.style.marginTop = -yy + "px";
                bigImg.style.marginLeft = -xx + "px";
            }
        }
    </script>
</head>
<body>
<div class="box">
    <div class="small">
        <img src="images/001.jpg" alt=""/>
        <div class="mask"></div>
    </div>
    <div class="big">
        <img src="images/0001.jpg" alt=""/>
    </div>
</div>
<script>
    //显示和隐藏
function show(ele) {
    ele.style.display = "block";
}

function hide(ele) {
    ele.style.display = "none";
}

function scroll() {  // 开始封装自己的scrollTop
    if (window.pageYOffset != null) {  // ie9+ 高版本浏览器
        // 因为 window.pageYOffset 默认的是  0  所以这里需要判断
        return {
            left: window.pageXOffset,
            top: window.pageYOffset
        }
    }
    else if (document.compatMode === "CSS1Compat") {    // 标准浏览器   来判断有没有声明DTD
        return {
            left: document.documentElement.scrollLeft,
            top: document.documentElement.scrollTop
        }
    }
    return {   // 未声明 DTD
        left: document.body.scrollLeft,
        top: document.body.scrollTop
    }
}

</scritp>
</body>

</html>


```

关键思路：左边小图，右边高清大图

1. 鼠标进入小图时，小图幕布和大图显示，鼠标离开小图则小图幕布和大图消失；
2. 鼠标在小图上移动时，幕布跟随鼠标移动，同时大图等比例移动；
3. 鼠标在小图移动时，移动幕布即需要控制幕布在小图内的位置，幕布需要绝对定位，并且需要获取幕布距离小图左边框和右边框的距离。

关键实现：

1. 这个左边距离x可以通过当前鼠标距离页面左边距离（event.clientX） - 小图距离页面左边的距离（元素.offsetLeft）获得，这个距离只是鼠标距离小图左边框的距离，我们需要获得的是幕布距离小图左边的距离，我们规定幕布移动时，鼠标要位于幕布的中心点，因此只要这个距离减去幕布宽度的一半（元素.offsetWidth/2）就是幕布距离小图左边框的距离x。幕布距离小图顶边框的距离同理。
2. 如果x小于0，那么说明鼠标快要移动到小图左边框了，那么规定x = 0；限制幕布不要出框。怎么判断鼠标快要移动到右边框呢，假设幕布移动到最右边，那么此时x在哪，此时x = 小图的宽度 - 幕布的宽度，因此超过这个距离则设置为这个距离。
3. 如果控制右边大图移动？此时右边大图设置了溢出隐藏，因此，只能显示左上角部分，当幕布在小图中向右边移动时，大图也应该向右边移动，向右移动就是让大图marginLeft设置为负值，那么当大图向左边移动时，左边会被溢出隐藏，右边得以暴露。那么移动的比例是多少，就是x距离的（大图宽度/幕布宽度）倍；

### 异步加载图片

```
function loadImage(file, success, fail) {
    const img = new Image();
    img.src = file;
    img.onload = ()=>{ //等待加载完成
        success(img);
    };

    img.onerror = ()=>{
        fail(new Error('img loaf fail'));
    };

}

loadImage(
    'img/1.jpeg', //通常是请求地址
    img =>{
        console.log('图片加载成功');
        document.body.appendChild(img);
        img.style.border = '1px solid red';
    },
    error =>{
        console.log('图片加载失败');
        console.log(error);
    }
);
```

### Promise.all()举例：多张图片上传
比如说，现在有一个图片上传的接口，每次请求接口时只能上传一张图片。需求是：当用户连续上传完九张图片（正好凑齐九宫格）之后，给用户一个“上传成功”的提示。这个时候，我们就可以使用Promsie.all()。

```
const imgArr = ['1.jpg', '2.jpg', '3.jpg', '4.jpg', '5.jpg', '6.jpg', '7.jpg', '8.jpg', '9.jpg'];
const promiseArr = [];
imgArr.forEach((item) => {
    const p = new Promise((resolve, reject) => {
        // 在这里做图片上传的异步任务。图片上传成功后，接口会返回图片的 url 地址
        //  upload img ==> return imgUrl
        if (imgUrl) {
            // 单张图片上传完成
            resolve(imgUrl);
        } else {
            reject('单张图片上传失败');
        }
    });
    promiseArr.push(p);
});
Promise.all(promiseArr)
    .then((res) => {
        console.log('图片全部上传完成');
        console.log('九张图片的url地址，组成的数组：' + res);
    })
    .catch((res) => {
        console.log('部分图片上传失败');
    });
```

### Promise.race()举例：图片加载超时
现在有个需求是这样的：前端需要加载并显示一张图片。如果图片在三秒内加载成功，那就显示图片；如果三秒内没有加载成功，那就按异常处理，前端提示“加载超时”或者“请求超时”。

```
// 图片请求的Promise
function getImg() {
    return new Promise((resolve, reject) => {
        let img = new Image();
        img.onload = function () {
            // 图片的加载，是异步任务
            resolve(img);
        };
        img.src = 'https://img.smyhvae.com/20200102.png';
    });
}

// 加载超时的 Promise
function timeout() {
    return new Promise((resolve, reject) => {
        // 采用 Promise.race()之后，如果 timeout() 的 promise 比 getImg() 的 promise先执行，说明定时器时间到了，那就算超时。整体的最终结果按失败处理。
        setTimeout(() => {
            reject('图片加载超时');
        }, 3000);
    });
}

Promise.race([getImg(), timeout()])
    .then((res) => {
        // 图片加载成功
        console.log(res);
    })
    .catch((err) => {
        // 图片加载超时
        console.log(err);
    });
```

### 使用 Promise 封装 SetTimeout 定时器

```
// 方法：XX秒后执行指定的代码。这个方法，就是在宏任务（定时器）的执行过程中，创建了一个微任务（resolve）
function delaySeconds(delay = 1000) {
    return new Promise((resolve) => setTimeout(resolve, delay));
}

delaySeconds(2000)
    .then(() => {
        console.log('qiangu');
        return delaySeconds(3000);
    })
    .then(() => {
        console.log('yihao');
    });
```

### JS实现动画

```
    function animation(element, location) {
        /** element: 目标元素， location : 目标位置
        */
        //清除目标元素上的定时器
        clearInterval(element.timer);
        //判断目标位置在元素的左边还是右边，speed决定向左还是向右的一步距离
        var speed = element.offsetLeft < location ? 10 : -10;
        //设置定时器
        element.timer = setInterval(()=>{
            //比较当前元素距离是否足够一个步距,不够则直接将目标元素位置设置为目标位置并清除定时器，否则目标元素继续移动
            var value = element.offsetLeft - location;
            if(Math.abs(value) < Math.abs(speed)) {
                element.style.left = location + 'px';
                clearInterval(element.timer);
            }else {
                element.style.left = element.offsetLeft + speed + 'px';
            }
        }, 10);
    }
```

### 轮播图实现原理
1.HTML+CSS基本布局逻辑：
标签：所有图片用ul>li包裹并将ul装在div容器中，另外左右按钮用一个div装着，显示图片的序号也用ol>li包着装在div中，并将这个三个div包一个大的div中。这个大的div外再包一个div.
布局：大的div相当于相机，而ul相当于需要拍摄的景色，因此大的div宽度只需要一个图片的宽度即可，而ul的宽度则根据实际图片张数决定，通过移动ul位置来实现图片向左滑动。大的div设置相对定位，其内的三个div设置绝对定位，ul>li则设置浮动，超出部分隐藏。

```
    <style>
        * {
            margin: 0;
            padding: 0;
            list-style: none;
        }
        .all {
            position: relative;
            width: 500px;
            height: 300px;
            overflow: hidden;
            margin: 0 auto;
            margin-top: 50px;
            padding: 10px;
            border: 1px solid #000;
        }

        .screen {
            position: relative;
            width: 500px;
            height: 300px;
            overflow: hidden;

        }

        .screen ul {
            position:absolute;
            font-size: 0;
            height: 300px;
            overflow: hidden;
            left: 0;
            top: 0;
        }

        .screen ul li {
            float: left;
            font-size: 0;
        }

        .screen ol {
            position: absolute;
            right: 5px;
            bottom: 10px;
            overflow: hidden;
        }

        .screen ol li {
            float: left;
            width: 20px;
            height: 20px;
            line-height: 20px;
            text-align: center;
            margin-left: 5px;
            border: 1px solid #000;
            background-color: rgba(0,0,0, .4);
            color:rgba(255,255,255, .4);


        }

        .screen ol li.current {
            background-color: rgba(255, 255, 0, .4);
        }

        #arrow {
            display: none;
        }

        #arrow span {
            position: absolute;
            left: 5px;
            top: 50%;
            transform: translateY(-50%);
            color: #fff;
            background-color: #000;
            text-align: center;
            width: 40px;
            height: 40px;
            line-height: 40px;
            font-size: 20px;
            opacity: .4;
        }
        #arrow span#right {
            left: auto;
            right: 5px;
        }
    </style>

    <div class="all">
        <div class="screen">
            <ul>
                <li><img src="img/1.jpeg" width="500" height="300" alt=""></li>
                <li><img src="img/2.jpeg"  width="500" height="300" alt=""></li>
                <li><img src="img/3.jpeg"  width="500" height="300" alt=""></li>
                <li><img src="img/4.jpeg"  width="500" height="300" alt=""></li>
            </ul>
            <ol>

            </ol>
            <div id="arrow">
                <span id="left"><</span>
                <span id="right">></span>
            </div>
        </div>
    </div>
```

2.JS控制逻辑：
1.动画实现滑动，默认不断循环移动，因此需要**全局变量timer保存循环定时器**。
2.同时为了记录点击移动图片的位置，需要**全局变量key记录第几张图片从此控制距离，square控制序号样式**。
3.当鼠标悬停在div/序号上时，清除定时器，离开div/序号时，重新添加定时器自动播放。
4.为了实现动画**平滑过渡**，需要在ul末尾添加和第一张图片一样的图片,这样当最后一张图片显示时，实际就是第一张图片的样式，再继续向右滑动时，先设置位置回到第一张图片,此时样式瞬间上没发生变化，再向右滑动到第二张图片。同理，向左滑动到第一张图片再继续向左滑动，此时设置位置取到最后一张图片，样子同第一张图片一样，再向左滑动到倒数第二张图片。

```
window.onload = ()=>{
            //获取标签元素
            let all = document.querySelector('.all');
            let screen = all.firstElementChild || all.firstChild;
            //获取屏幕的大小
            let imgWidth = screen.scrollWidth;
            let ul = screen.firstElementChild || screen.firstChild;
            let ol = screen.children[1];
            let div = screen.lastElementChild || screen.lastChild;
            let arrow = div.children;

            //通过定时器实现自动滑动图片
            let timer = setInterval(autoPlay, 1000);
            //key记录当前是第几张图片，用于控制滑动的距离
            let key = 0;
            //square记录当前图片的序号，用于控制序号样式
            let square = 0;

            //克隆第一张图片并放到ul的最后面，平滑过度需要
            let newLi = ul.children[0].cloneNode(true);
            ul.appendChild(newLi);

            //动态控制ul的宽度
            ul.style.width = ul.children.length * imgWidth + 'px';
            //为ol添加li标签，显示图片序号
            for(let i = 0; i < ul.children.length - 1; i++) {
                let newLi = document.createElement('li');
                newLi.innerHTML = i + 1;
                ol.appendChild(newLi);
            }
            //默认显示第一张图片，则显示第一个序号
            ol.children[0].className = 'current';
            var olArrLi = ol.children;

            //当鼠标悬停在序号上时，图片自动滑至那个位置
            for(let i = 0; i < olArrLi.length; i++) {
                olArrLi[i].index = i;
                olArrLi[i].onmouseover = function() {
                    //先清除默认播放
                    clearInterval(timer);
                    //清楚序号样式
                    for(let i = 0; i < olArrLi.length; i++) {
                        olArrLi[i].className = '';
                    }
                    //将当前序号设置为对应样式
                    olArrLi[this.index].className = 'current';
                    //记录下当前的序号和第几张图片
                    key = square = this.index;
                    //向左移动ul就可实现滑动, 显示第二张图片就要key ==1，只需要移动一个imgWidth即可
                    animate(ul, -imgWidth * key);
                };
                //鼠标离开序号列表就重新自动播放
                olArrLi[i].onmouseleave = function() {

                    timer = setInterval(autoPlay, 1000);
                }
            }

            //当鼠标悬停在screen时，左右箭头显示并暂停自动播放
            all.onmouseover = ()=>{
                div.style.display = 'block';
                clearInterval(timer);
            };
            //当鼠标离开在screen时，左右箭头小时并自动播放
            all.onmouseleave = ()=>{
                div.style.display = 'none';
                timer = setInterval(autoPlay, 1000);
            };

            //左箭头点击
            arrow[0].onclick = function(){
                key--;
                //如果是第一张图片，把ul的位置设置为倒数第1张图片，通过倒数第一张图片滑动到倒数第二张图片，因为倒数一张图片和第一张图片是一样的，从第一张图片瞬间换到倒数第一张图片，在样式上是看不出变化的，而从倒数第一张图片滑动到倒数第二张图片是动画效果
                if(key < 0) {
                    //显示最后一张图片 == 第一张图片
                    ul.style.left = -imgWidth * (olArrLi.length) + 'px';
                    //目标key设置为显示倒数第二个图片所需要的key
                    key = olArrLi.length - 1;
                }
                //移动
                animate(ul, -imgWidth * key);
                //
                square--;
                if(square < 0) {
                    //显示倒数第二张图片的序号
                    square = olArrLi.length - 1;
                }
                for (let i = 0; i < olArrLi.length; i++) {
                    olArrLi[i].className = '';
                }
                olArrLi[square].className = 'current';

            };

            //向右滑动和自动播放实现一样
            arrow[1].onclick = function(){
                autoPlay();
            }

            function autoPlay() {
                /**
                 * 当key == 1时表示滑动到第2张图片，当key == 4表示滑动到第五张图片
                 * 但是实际上为了实现无缝衔接，我们把ul的最后一张图片新增为第一张图片，
                 * 然后继续向左滑动，这样就是第一张图片了，然后key++ ，此时key == 5，
                 * 我们需要滑动到第二张图片，所以先把ul的left设置为0，方便向左滑动，
                 * key == 1,表示滑动第二图片。
                 * 如果没有第五张图片作为缓冲，那么从第四张瞬间回到第一张图片或者直接从第四张向左滑动到第一张
                 * 这不是我们想要的效果
                 * */
                key++;
                if(key > olArrLi.length) {
                    //表明当前在最后一张图片，此时显示和第一张图片一样，k++就是要显示第二张图片，因此，设置ul.style.left = 0;是瞬间完成的，而在样式上没发生变化，key为显示第二张图片所需的key == 1；
                    ul.style.left = 0;
                    key = 1;
                }
                animate(ul, -imgWidth * key);

                square++;
                if(square > olArrLi.length - 1) {
                    square = 0;
                }
                for(let i = 0; i < olArrLi.length; i++) {
                    olArrLi[i].className = '';
                }
                olArrLi[square].className = 'current';

            }

            function animate(ele, target) {
                clearInterval(ele.timer);
                var speed = ele.offsetLeft < target ? 10 : -10;
                ele.timer = setInterval(()=>{
                    var val = ele.offsetLeft - target;
                    if(Math.abs(val) < Math.abs(speed)) {
                        ele.style.left = target + 'px';
                        clearInterval(ele.timer);
                    }else {
                        ele.style.left = ele.offsetLeft + speed + 'px';
                    }
                },10);
            }
        };
```

### 鼠标滚动元素出现
HTML布局：flex布局，containerDiv包含所有div滑块，flex控制containerDiv居中，div滑块依次按行向下排列，默认每个滑块translate到body之外。当添加在滑块添加show类时，滑块tranlate(0);

JS逻辑：获取所有滑块并转为真数组，为滚轮添加监听事件，监听事件里遍历每个滑块，**判断每个滑块顶部和视窗顶部的距离是否小于一个视窗的高度**，如果小于则说明当前滑块应当出现在视窗内，如果大于则说明滑块在视窗的下面。
每个滑块顶部和视窗顶部的距离 = 滑块距离页面顶部的距离 - 滚轮滚动的距离；

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        html {
            box-sizing: border-box;
        }
        body {
            background-color: rgb(236, 234, 209);
            display: flex;
            justify-content: center;
            align-items: center;
            margin: 0;
            overflow-x: hidden;
            padding-bottom: 50px;
        }
        .container {
            position: relative;
        }
        .container h1 {
            width: 400px;
            font-size: 30px;
            margin: 10;
        }
        .container div {
            display: flex;
            justify-content: center;
            align-items: center;
            width: 400px;
            height: 200px;
            text-align: center;
            line-height: 200px;
            font-size: 20px;
            color: white;
            background-color: #4682B4;
            margin: 10px 0;
            border-radius: 10px;
            box-shadow: 2px 2px 10px black;
            transition: all 1s ease;
            opacity: 0;
            transform: translate(-500%);
        }
        .container div h2 {
            margin: 10px;
        }
        .container div:nth-child(even) {
            transform: translate(500%);
        }
        .container .box.show {
            transform: translate(0px);
            opacity: 1;
        }
    </style>

</head>
<body>
    <div class="container">
        <h1>Scroll to see the animation</h1>
        <div class="box show"><h2>Content</h2></div>
        <div class="box show"><h2>Content</h2></div>
        <div class="box show"><h2>Content</h2></div>
        <div class="box show"><h2>Content</h2></div>
        <div class="box"><h2>Content</h2></div>
        <div class="box"><h2>Content</h2></div>
        <div class="box"><h2>Content</h2></div>
        <div class="box"><h2>Content</h2></div>
        <div class="box"><h2>Content</h2></div>
        <div class="box"><h2>Content</h2></div>
        <div class="box"><h2>Content</h2></div>
        <div class="box"><h2>Content</h2></div>
        <div class="box"><h2>Content</h2></div>
    </div>
</body>
<script>
    window.onload = ()=>{
        var box = document.querySelectorAll('.box');
        var arr = [...box];
        var divHeight = arr[0].offsetHeight;
        console.log(window.scrollY);
        window.onscroll = (event)=>{
            arr.map((item,index)=>{
                //元素底部距离滚动窗口顶部的距离 = 元素顶部距离页面顶部的距离 + 元素高度  - 滚动条滚动的距离
                var distance = item.offsetTop + divHeight - window.scrollY;

                if(distance < window.innerHeight && !item.classList.contains('show')) {
                    item.classList.add('show');
                }else if(distance >= window.innerHeight && item.classList.contains('show')){
                    item.classList.remove('show');
                }

            });

        };
    };
</script>
</html>
```

### 懒加载+多种效果同时到达
效果：当加载页面时，背景图片从模糊到清晰，文字从0%变为100%，两者同步到达

HTML布局：flex布局，控制div块居中，里面的文字也居中。图片作为section标签的背景图片，并让section绝对定位，宽度高度铺满全屏，默认flur:blur(0px);表示模糊程度为0；

JS逻辑：获取div块和section块，设置一个全局变量load默认0用于控制文字从0到100，设置一个定时器并保存在一个变量中，定时器每次循环load变量自增,如果超过100则清除定时器，否则将load并改到文字中，同时控制section块的flur：blur(30px)->flur: blur(0px);
关键实现：当text=100%时，同时blur(0px)，

```
<html lang="en"><head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="style.css">
    <title>Blurry Loading</title>
    <style>
        * {
          box-sizing: border-box;
        }

        body {
          display: flex;
          align-items: center;
          justify-content: center;
          height: 100vh;
          overflow: hidden;
          margin: 0;
        }

        .bg {
          background: url('https://images.unsplash.com/photo-1576161787924-01bb08dad4a4?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=2104&q=80')
            no-repeat center center/cover;
          position: absolute;
          top: -30px;
          left: -30px;
          width: calc(100vw + 60px);
          height: calc(100vh + 60px);
          z-index: -1;
          filter: blur(0px);
        }

        .loading-text {
          font-size: 50px;
          color: #fff;
        }

    </style>
  </head>
  <body>
    <section class="bg" style="filter: blur(0px);"></section>
    <div class="loading-text" style="opacity: 0;">100%</div>

    <script>
        const loadText = document.querySelector('.loading-text')
        const bg = document.querySelector('.bg')

        let load = 0

        let int = setInterval(blurring, 30)

        function blurring() {
          load++

          if (load > 99) {
            clearInterval(int)
          }

          loadText.innerText = `${load}%`
          loadText.style.opacity = scale(load, 0, 100, 1, 0)
          bg.style.filter = `blur(${scale(load, 0, 100, 30, 0)}px)`
        }

        // https://stackoverflow.com/questions/10756313/javascript-jquery-map-a-range-of-numbers-to-another-range-of-numbers

        const scale = (num, in_min, in_max, out_min, out_max) => {
          return ((num - in_min) * (out_max - out_min)) / (in_max - in_min) + out_min
        }

        /*等价于
            const scale = (num, in_min, in_max, out_min, out_max) => {
                var inRange = in_max - in_min;
                var outRange = out_max - out_min;
                return (num - in_min) *( outRange / inRange) + out_min;
            }
            实际就是 num * (range/numRange) + min = （num/numRange）* range + min , numRange 就是num变化的范围，在上文中num的变化是从0-100,因此numRange = 100, 当num = 100时，公式就等于range + min,range就是我们想要的min-max的范围，而针对上文变化，我们想要的效果就是opacity和blur由大变小，因此使range 为负数，min换成30就可以实现由大到小变化。这个公式类似random()* range +min,只不过random在0-1取随机数，而上文的公式利用定时器实现匀速变化值；
        可写成如下：
        const scale = (num, in_min, in_max, out_min, out_max) => {
          return ( (num / (in_max - in_min) ) *( out_max - out_min ) )  + out_min;
        }
        */
    </script>


</body></html>
```

### 聚焦时input文字上浮
HTMl布局：input文字可以是整体也可以是全部单词分割开和input标签放在同一div中，并且input display：block,lable display: inline-block；这样默认input在上面，label在input下面，为了使label跑到input的上方 transform:translate(-40px);，并且为label添加active类样式，transform:translate(-80px);
JS逻辑：当聚焦时，设置一个定时器，控制每个label添加active类，失去焦点时删除active类，需要注意何时删除定时器，怎么控制第几个label;

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        * {
            margin: 0;
            padding: 0;
        }
        body {
            height: 100vh;
            background-color: steelblue;
            position: relative;
            overflow: hidden;
            font-size: 1rem;
        }
        .container {
            width: 18rem;
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background-color: rgba(0, 0, 0, 0.4);
            color: white;
            padding: 20px 40px;
            border-radius: 4px;
            font-size: 1.2rem;
        }
        .container form fieldset {
            border: none;
            display: flex;
            justify-content: center;
            align-items: center;
            flex-direction: column;
            color: white;
        }
        .container form fieldset div {
            width: 100%;
            margin: 25px 0;
            font-size: 0;
            position: relative;
        }
        .container form input {
            display: block;
            padding: 1px;
            width: 100%;
            height: 2rem;
            background: transparent;
            border: none;
            border-bottom: 2px solid white;
            outline: none;
            color: white;
        }
        .container form input[type="submit"] {
            margin: 0 auto;
            border: 2px solid lightblue;
            border-radius: 4px;
            height: 40px;
            width: 90%;
            background-color: lightblue;
            color: black;
        }
        .container form input:focus {
            border-color: lightblue;
        }

        .container form legend {
            text-align: center;
            font-size: 2rem;
        }
        .container form fieldset>div label{
            font-size: 1.2rem;
            display: inline-block;
            transform: translate(0,-34px);
            transition: all 1s ease;
        }
        .container form fieldset>div label.active {
            transform: translate(0,-66px);
        }
        .container .register a {
            color: lightblue;
            text-decoration: none;
        }
    </style>
</head>
<body>
    <div class="container">
        <form action="#">
            <fieldset>
                <legend>Please Login</legend>
                <div>
                    <input id="Email" name="Email" type="text" required>
                    <label for="Email" class="email "  >E</label>
                    <label for="Email" class="email" >m</label>
                    <label for="Email" class="email">a</label>
                    <label for="Email" class="email">i</label>
                    <label for="Email" class="email">l</label>
                </div>
                <div>
                    <input type="password" id="password" name="password" required>
                    <label for="password" class="psd">P</label>
                    <label for="password" class="psd">s</label>
                    <label for="password" class="psd">s</label>
                    <label for="password" class="psd">w</label>
                    <label for="password" class="psd">o</label>
                    <label for="password" class="psd">r</label>
                    <label for="password" class="psd">d</label>
                </div>
                <div>
                    <input id="submit" name="submit" type="submit" value="Login">
                </div>
            </fieldset>
        </form>
        <div class="register">Don't have a count?<a href="#">Register</a></div>
    </div>
    <script>
        window.onload = ()=>{
            let email = document.querySelector('#Email');
            let password = document.querySelector('#password');
            let emailLi = document.querySelectorAll('.email');
            let psd = document.querySelectorAll('.psd');
            let index = 0;
            let timer;

            function move(ele, elementList, method) {
                var timer;
                var index = 0;
                ele.addEventListener(method, ()=>{
                    clearInterval(timer);
                    index = 0;
                    timer = setInterval(() => {
                        if (index >= elementList.length) {
                            clearInterval(timer);
                        } else {
                            if(method == 'focus') {
                                if (!elementList[index].classList.contains('active')) {
                                    elementList[index].classList.add('active');
                                }
                            }else if(method == 'blur') {
                                if (elementList[index].classList.contains('active')) {
                                    elementList[index].classList.remove('active');
                                }
                            }
                        }
                        index++;
                    }, 80);
                });
            };

            move(email, emailLi, 'focus');
            move(email,emailLi, 'blur');
            move(password, psd, 'focus');
            move(password, psd, 'blur');
        };
    </script>
</body>
</html>
```

### 控制按钮旋转动画
效果：默认按钮为X,点击之后变为=
实现逻辑：用两个div做成两条横线，然后各自先下上偏移7px,然后各自顺时针逆时针旋转765度，形成交叉，点击之后，rotate(0) tranlasteY(0)即可；

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        * {
            margin: 0;
            padding: 0;
        }
        body {
            box-sizing: border-box;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;

        }
        .container {
            position: relative;
            width: 20rem;
            height: 4rem;
            border: 1px solid black;
            box-shadow: 1px 1px 5px #000;
            transition: all .4s ease;
        }
        .container ul{
            overflow: hidden;
            list-style-type: none;
            display: inline-block;

        }

        .container.active {
            width: 3rem;
            height: 4rem;
        }
        .container.active ul {
            display: none;
        }
        .container ul li {
            float: left;
            margin: 0 10px;
            height: 4rem;
            line-height: 4rem;
            text-align: center;
            font-size: 1rem;
            transition: all .2 ease-in;
        }
        .container.active ul li {
            display: none;
        }
        .container button {
            width: 3rem;
            height: 3rem;
            line-height: 2rem;
            position: absolute;
            top: 50%;
            transform: translateY(-50%);
            border: none;
            right: 0;
            color: #5290f9;
            background-color: transparent;
            font-size: 1.2rem;
            transition: all .4s ease;
        }
        .container .icon .line {
            margin: 0 auto;
            background-color: #5290f9;
            height: 2px;
            width: 20px;
            position: absolute;
            top: 20px;
            left: 10px;
            transition: transform 0.6s linear;
            transform: rotate(-765deg) translateY(7px);
        }
        .container .icon .line2 {
            top: 30px;
            transform: rotate(765deg) translateY(-7px);
        }
        .container.active .line {
            transform: rotate(0) ;
        }
    </style>
</head>
<body>
    <div class="container">
        <ul>
            <li>Home</li>
            <li>Works</li>
            <li>About</li>
            <li>Concat</li>
        </ul>
        <button class="icon">
            <div class="line line1"></div>
            <div class="line line2"></div>
        </button>
    </div>
    <script>
        window.onload = ()=>{
            let container = document.querySelector('.container');
            let button = container.lastElementChild;
            button.addEventListener('click', () => {
                container.classList.toggle('active');

            });
        };
    </script>
</body>
</html>
```

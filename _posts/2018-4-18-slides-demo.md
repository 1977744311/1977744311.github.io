---
layout: post
title: 制作一个简单的轮播 
categories: javascript
description: 简单制作一个轮播
keywords: 技术
---

# 轮播的思路

首先我们要想轮播是怎样实现的,可以想多种思路，然后选择一种去实现它。下面是我想的几种思路：

1. 控制图片的优先级，利用z-index属性完成轮播。
2. 设置一片固定窗口，然后设置overflow:hidden属性，利用translate属性完成轮播。
3. 预设几个css样式，然后用js去改变样式来实现轮播。

我选了第二种思路。

# 创建demo
思路敲定好以后，我们就开始着手建立demo，首先要注意的一个原则是HTML、CSS、JS 内容、样式与行为分离.

我们分别创建一个.html .css .js 文件

创建完成后我们再去找轮播所需要的图片，可以上百度或谷歌都可以，但是要注意的是图片的大小最好不要超过300kb，且不要擅自更改的图片的长宽比。

接下来我们要准备好jquery.js文件，把它放到文件夹里，并在html里面引用。

然后开启http-server，这样准备工作就完成了。

# 编写html
话不多说，直接上代码：
```angularjs
<!DOCTYPE html>
<html>

<head>
    <meta charset="utf-8">
    <title>JS Bin</title>
    <link rel="stylesheet" href="./style.css" title="" type="" />
    <script src="//code.jquery.com/jquery-2.1.1.min.js"></script>
</head>

<body>
    <div class="window">
        <div class="images" id=images>
            <img src="./1.jpg" width=1000 alt="图片1" height=500>
            <img src="./2.jpg" width=1000 alt="" height=500>
            <img src="./3.jpg" width=1000 alt="" height=500>
            <img src="./4.jpg" width=1000 alt="" height=500>
            <img src="./5.jpg" width=1000 alt="" height=500>
        </div>
    </div>
    <span id=buttons>
        <span>第1张</span>
        <span>第2张</span>
        <span>第3张</span>
        <span>第4张</span>
        <span>第5张</span>
    </span>
    <script src="./main.js "></script>
</body>

</html>
```
我们首先创建class为window的一个div作为轮播的窗口,这个窗口的宽度是固定的，然后里面是一个class为images的div，它的下面就是五个轮播图片的img。

然后再下面创建五个按钮，用以切换图片。

# 编辑css
代码如下：
```angularjs
.images {
    display: flex;
    align-items: flex-start;
    transition: all 0.5s;
}

.images>img {
    vertical-align: top;
}

.window {
    width: 1000px;
    overflow: hidden;
}
.red {
    color: red;
}
```
因为我的思路是用js控制translate，所以css比较简单

# 编辑js
接下来才算是进入正题了，那么我们应该从哪里入手呢？

首先我们应该为我们的按钮增加点击事件，点到哪个按钮就切换到哪个图片，代码如下：
```angularjs
var allButtons = $('#buttons > span')   //获取所有span元素

for (let i = 0; i < allButtons.length; i++) {   //遍历的创建点击事件
  $(allButtons[i]).on('click', function(x) {
    console.log('hi')
    var index = $(x.currentTarget).index()    //获取到当前点击的按钮的index值
    var p = index * -1000
    $('#images').css({
      transform: 'translate(' + p + 'px)'    //改变translate属性
    })
    n = index
    allButtons.eq(n)                          //给按钮增加红色样式
      .addClass('red')
      .siblings('.red').removeClass('red')
  })
}
```
这样的话就可以实现点击哪个按钮就轮播到哪个图片。

接下来实现让它自动轮播：
```angularjs
var n = 0;
var size = allButtons.length
allButtons.eq(n % size).trigger('click')   //初始化添加点击事件
  .addClass('red')
  .siblings('.red').removeClass('red')

var timerId = setInterval(() => {       //模拟给按钮添加点击事件
  n += 1
  allButtons.eq(n % size).trigger('click')
    .addClass('red')
    .siblings('.red').removeClass('red')
}, 1000)
```
现在它就可以自动轮播了。

接下来我们想让用户把鼠标浮在图片上的时候轮播自动停止，鼠标移除去的时候自动开始：
```angularjs
$('.window').on('mouseenter', function() {    //当鼠标浮在图片上时砸掉这个计时器
  window.clearInterval(timerId)
})

$('.window').on('mouseleave', function() {    //当鼠标移出时重新开始计时器
  timerId = setInterval(() => {
    n += 1
    allButtons.eq(n % size).trigger('click')
      .addClass('red')
      .siblings('.red').removeClass('red')
  }, 3000)
})
```
到目前为止，一个简单的轮播就完成了。

# 代码优化封装
封装的思路如下：
1. 从 API 开始思考
2. 尽量能让使用者猜到
封装后的代码如下：
```angularjs
var allButtons = $('#buttons > span')

for (let i = 0; i < allButtons.length; i++) {
    $(allButtons[i]).on('click', function (x) {
        var index = $(x.currentTarget).index()
        var p = index * -1000
        $('#images').css({
            transform: 'translate(' + p + 'px)'
        })
        n = index
        activeButton(allButtons.eq(n))
    })
}



var n = 0;
var size = allButtons.length
allButtons.eq(n % size).trigger('click')


var timerId = setTimer()

function setTimer() {
    return setInterval(() => {
        n += 1
        allButtons.eq(n % size).trigger('click')
    }, 3000)
}

function activeButton($button) {
    $button
        .addClass('red')
        .siblings('.red').removeClass('red')
}

$('.window').on('mouseenter', function () {
    window.clearInterval(timerId)
})

$('.window').on('mouseleave', function () {
    timerId = setTimer()
})
```

这篇博客到此就告一段落了，我会在下一篇博客里将如何实现无缝的轮播，下面是上述代码所做demo的地址：

<http://stevenself.top/slider-demo/.>


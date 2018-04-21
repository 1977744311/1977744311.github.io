---
layout: post
title: 制作一个无缝的轮播
categories: javascript，jquery
description: 无缝轮播
keywords: 技术
---

# 什么是无缝的轮播

上一篇博客我们讲了如何制作一个简单的轮播，那么我们可以发现，这个轮播的最后一张切换到第一张是不是直接切换的，而是倒着把所有图片全过一遍才切到第一张的。
而无缝轮播的意思是图片之间的切换是没有空隙的，从最后一张到第一张的切换跟第一张到第二张是一样的，这样的轮播被称之为无缝轮播。

# 无缝轮播的思路

既然两个轮播的效果不同，那么实现方法自然也不一样，所以我们现在首要做的的事就是推倒重来，把以前想的思路忘掉，重新思考。

你可能会想到好几种思路，但是都不行，那么请你不要放弃，学习就是这么一个不断去思考的事情。没有任何事情是一蹴而就的的，即使你觉得简单那也是建立在你的经验之上。

如果现在还没想到，那么没关系，我在这里就直接把答案告诉大家了。

1. 行为，样式，内容分离，用js来控制样式最好的方法是控制元素的class，不建议直接控制元素的样式，那么我们首先先尊守的就是这个原则。

2. 无缝轮播无非就是三种状态，一个是当前显示的站现在用户眼前的图片，一个是待被展示的图片，一个是该图片在做完动画的状态。我们把这三种状态分别起名为current，enter，leave，先假设只有三张图片，然后按照下图所示：

![](https://1977744311.github.io/images/posts/weldless-slider/2.png)

图示就是三种状态的转换，需要注意的是当从current状态转换到leave状态完毕后要立即转换到enter状态，而从leave状态转换到enter状态这段动画是不能被用户看到的，用户看到的只有从enter转换到leave这段。

3. 理解了上述三种状态，就应该明白无缝轮播是怎么实现得了，无非就是三种状态的变化。

# 开始动手吧

怎样获取图片和创建Demo开启http-server上片博客已经说了，这篇我们就先从css说起，首先建立三个状态的class，可以用translate控制：

```angularjs
 {
    margin: 0;
    padding: 0;
}

* {
    box-sizing: border-box;
}

.window {
    width: 400px;
    height: 300px;
    margin: 20px auto;
    overflow: hidden;
}

.images {
    position: relative;
}

.images>img {
    width: 100%;
    transition: all 1500ms;
    position: absolute;
    top: 0;
}

.images>img.current {         //展示给用户的状态
    left: 0;
    transform: translateX(0);
    z-index: 1;
}

.images>img.leave {           //完成动画后的状态
    transform: translateX(-100%);
    z-index: 1;
}

.images>img.enter {           //还未被展示的状态
    transform: translateX(100%);
}
```

写完css后，我们开始写控制行为的js,首先我们先封装几个函数

1. 第一个是得到当前图片的函数

```angularjs
function getImage(n) {
    return $(`.images > img:nth-child(${x(n)})`)
}
```
上述代码用到了模板字符串，${}就是一个模板字符串，具体可以参考[mdn](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/template_strings)

2. 第二个是得到当前图片的序号：

```angularjs
function x(n) {
    if (n > 3) {
        n = n % 3
        if (n === 0) {
            n = 3
        }
    } // n = 1 2 3
    return n
}
```

3. 然后就是切换三种状态的函数：

```angularjs
function makeCurrent($node) {
    return $node.removeClass('enter leave').addClass('current')
}

function makeLeave($node) {
    return $node.removeClass('enter current').addClass('leave')
}

function makeEnter($node) {
    return $node.removeClass('leave current').addClass('enter')
}
```

4. 接下来需要写一个初始化函数：

```angularjs
function init() {
    n = 1  
    $(`.images > img:nth-child(${n})`).addClass('current')
        .siblings().addClass('enter')
}
```

5. 最后就是真正的逻辑函数：

```angularjs
let n
init()
setInterval(() => {
    makeLeave(getImage(n))
        .one('transitionend', (e) => {
            makeEnter($(e.currentTarget))
        })
    makeCurrent(getImage(n + 1))
    n += 1
}, 2000)
```

到此我们就完成了一个无缝轮播。

# 优化

以上我们限定了是三张图片，那么我们可不可以把它封装的再好一点，这样即使是四张五张也能使这段代码正常使用，我们只需要把得到当前图片序列号的那个函数里的常量变为变量就可以了

```angularjs
let nodes = $('.images').children().length
function x(n) {
    if (n > nodes) {
        n = n % nodes
        if (n === 0) {
            n = nodes
        }
    } // n = 1 2 3 4 5 .....
    return n
}
```

# 结束

到此我们就完成了一个简单的无缝轮播，当然我们还有许多功能没有添加，比如可以手动切换的按钮；当然还有几个小小的bug需要去解决，这些我将会在下篇博客讲到。下面是本次demo的地址：

<https://github.com/1977744311/weldless-slider-demo>

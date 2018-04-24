---
layout: post
title: 无缝轮播（最终版）
categories: javascript，jquery
description: 无缝轮播
keywords: 技术，技巧
---

# 上次轮播的bug

1. 只能按顺序轮播，123轮播，不能321啊。
2. 如果在打开页面的情况下，去其他页面一段时间后再切回来，会出现一下子跳好几张图片。

# 先来解决第二个bug

我们先来解决第二个bug，你说为什什么？当然是因为第二个bug比较好解决了，这个bug出现的原因是因为浏览器会在你在浏览其他tag时会压缩你这个页面的事件，减少内存占用。
所以当你切换回来的时候，浏览器会在一瞬进把刚刚本来应该后台进行的事一下子做完。

那么解决的思路是什么是什么呢？首先我们需要一个可以监听用户是不是在浏览其他页面的API，这个API是visibilitychange,当用户监听这个事件时，会返回一个hidden的值，
当用户去浏览其他页面时，返回true，当用户返回本页面时返回false。那么我们就可以当用户浏览其他页面时，停止自动轮播的函数，当用户切换回来时，再开启。

代码如下：
```angularjs
let n 
let nodes = $('.images').children().length
init()
let timer = setInterval(() => {
    makeLeave(getImage(n))
        .one('transitionend', (e) => {
            makeEnter($(e.currentTarget))
        })
    makeCurrent(getImage(n + 1))
    n += 1
}, 2000)

document.addEventListener('visibilitychange',function(){
    if(document.hidden){
        window.clearInterval(timer)
    }else{
        timer = setInterval(() => {
            makeLeave(getImage(n))
                .one('transitionend', (e) => {
                    makeEnter($(e.currentTarget))
                })
            makeCurrent(getImage(n + 1))
            n += 1
        }, 2000)
    }
})
```

# 解决第一个bug

第一个bug其实也不算bug，是需求的问题，但是如果以上片博客所述的代码来做的话，基本上无法实现，既然无法实现，那么我们只能全盘否定，换一种思路。

这时候我们返回去看我们第一次实现轮播的思路，控制translate属性来实现图片的轮播，这种思路可以实现按照任意顺序轮播。但是又不能做到第一张与最后一张无缝，
那么我们应该怎么做呢？

我们可以在第一张图的前面创建一个假的最后一张，最后一张图的后面创建一个假的第一张，然后用我们的js偷梁换柱。

代码如下：
```angularjs
let $buttons = $('#buttonWrapper>button')
let $slides = $('#slides')
let $images = $slides.children('img')
let current = 0

makeFakeSlides()
$slides.css({transform:'translateX(-400px)'})
bindEvents()
$(next).on('click', function(){
  goToSlide(current+1)
})
$(previous).on('click', function(){
  goToSlide(current-1)
})

let timer = setInterval(function(){
  goToSlide(current+1)
},2000)
$('.container').on('mouseenter', function(){
  window.clearInterval(timer)
}).on('mouseleave', function(){
  timer = setInterval(function(){
    goToSlide(current+1)
  },2000)
})

function bindEvents(){
  $('#buttonWrapper').on('click', 'button', function(e){
    let $button = $(e.currentTarget) 
    let index = $button.index()
    goToSlide(index)
  })
}

//重要
function goToSlide(index){
  if(index > $buttons.length-1){
    index = 0
  }else if(index <0){
    index = $buttons.length - 1
  }
  console.log('current', 'index')
  console.log(current, index)
  if(current === $buttons.length -1 && index === 0){
    // 最后一张到第一张
    console.log('here')
    $slides.css({transform:`translateX(${-($buttons.length + 1) * 400}px)`})
      .one('transitionend', function(){
        $slides.hide()
        $slides.offset() // .offset() 可以触发 re-layout，这是一个高级技术，删掉这行你就会发现 bug，所以只能加上这一行。
        $slides.css({transform:`translateX(${-(index+1)*400}px)`}).show()
      })

  }else if(current === 0 && index === $buttons.length - 1){
    // 第一张到最后一张
    $slides.css({transform:`translateX(0px)`})
      .one('transitionend', function(){
        $slides.hide().offset()
        $slides.css({transform:`translateX(${-(index+1)*400}px)`}).show()
      })

  }else{
    $slides.css({transform:`translateX(${- (index+1) * 400}px)`})
  }
  current = index
}

function makeFakeSlides(){     //创建假的第一张和最后一张
  let $firstCopy = $images.eq(0).clone(true)
  let $lastCopy = $images.eq($images.length-1).clone(true)

  $slides.append($firstCopy)
  $slides.prepend($lastCopy)
}
```

下面是本次DEMO的地址：
<https://github.com/1977744311/weldless-slider-demo2>

# 关于轮播这件事

其实吧，轮播这件事说容易也容易，说难也难。如果你想做一个真正完美的轮播，那么你没有几个小时是做不出来的，因为你不仅仅是写出来代码就可以了，
你还要测试各种情况下有没有bug，还有各种浏览器的兼容性，还有手机适配等等等等。所以不建议你去手动做一个完美的轮播，当然我做这几个DEMO主要是为了让我
熟悉下dom事件，还有一些浏览器的api，是用来学习的。但是如果你用来开发的话，建议你直接使用别人编好的插件，毕竟我们作为前端程序员，最需要擅长的就是使用
现有的无bug的现成的API。轮播的插件我推荐一个名字为swiper的轮播插件，支持PC端手机端，是一个非常好用的插件，它的网址为<http://idangero.us/swiper/>
具体的使用方法可以参考官网的教程。

# 最后的最后

我自己做了个苹果风格的轮播，仿照苹果官网ipad pro介绍页面的轮播，虽然代码质量很差，但也差不多仿的八九不离十，下面是demo地址：

<https://github.com/1977744311/apple-slider>







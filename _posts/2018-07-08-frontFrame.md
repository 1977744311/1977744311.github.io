---
layout: post
title: 浅谈前端框架
categories: vue
description: vue,react,Angular
keywords: mvvm
---

# 为什么要使用前端框架？

## 前言

自 Backbone 之后前端框架就如同雨后春笋般出现,我们已经习惯了用各种框架进行开发,但是前端框架出现的意义是什么?我们为什么要选择前端框架进行开发呢?

## 前端框架的好处

最开始学习前端框架的时候(我第一个框架是 Vue)并不理解框架能带来什么,只是因为大家都在用框架,最实际的一个用途就是所有企业几乎都在用框架,不用框架就没法找到好工作.

随着使用的深入我逐渐理解到框架的好处:

1. 组件化: 高度的组件化可以是我们的工程易于维护、易于组合拓展。
2. 天然分层: JQuery 时代的代码大部分情况下是面条代码,耦合严重,现代框架不管是 MVC、MVP还是MVVM 模式都能帮助我们进行分层，代码解耦更易于读写。
3. 生态: 现在主流前端框架都自带生态,不管是数据流管理架构还是 UI 库都有成熟的解决方案。

## 前端框架的根本意义

上节只是说了前端框架给我们带来的好处，我也一直认为这是前端框架存在的意义，但是直到我看了[这篇文章](https://segmentfault.com/a/1190000014947677),我才发现：

>前端框架的根本意义是解决了UI 与状态同步问题。

例如，在 Vue 中我们如果要在`todos`中添加一条,只需要`app4.todos.push({ text: '新项目' })`,这时由于 Vue 内置的响应式系统会自动帮我们进行 UI 与状态的同步工作.

```angularjs
<div id="app-4">
  <ol>
    <li v-for="todo in todos">
      {{ todo.text }}
    </li>
  </ol>
</div>
```
```
var app4 = new Vue({
  el: '#app-4',
  data: {
    todos: [
      { text: '学习 JavaScript' },
      { text: '学习 Vue' },
      { text: '整个牛项目' }
    ]
  }
})
```

如果我们用 JQuery 或者 JS 进行操作,免不了一大堆li.appendChild、document.createElement等 DOM 操作,我们需要一长串 DOM 操作保证状态与 UI 的同步,其中一个环节出错就会导致 BUG,手动操作的缺点如下：

1. 频繁操作 DOM 性能低下.

2. 中间步骤过多,易产生 bug且不易维护,而且心智要求较高不利于开发效率

不管是 vue 的数据劫持、Angular 的脏检测还是 React 的组件级 reRender都是帮助我们解决 ui 与状态同步问题的利器。

这也解释了Backbone作为前端框架鼻祖在之后落寞的原因,Backbone只是引入了 MVC 的思想,并没有解决 View 与 Modal 同步的问题,相比于现代的三大框架直接操作 Modal 就可以同步 UI 的特性, Backbone 仍然与 JQuery 绑定,在 View 里操作 Dom来达到同步 UI 的目的，这显然是不符合现代前端框架设计要求的。

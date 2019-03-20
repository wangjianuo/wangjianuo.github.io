---
title: React丨第一季 第1集 Hello React
date: 2019-03-18 18:00:04
categories: "React"
tags: 
  - React
---


# 初识React
React是FB在2013年开源在Github上的JavaScript库。

## 专注视图层
React并不是完整的MVC/MVVM框架，它专注于视图层解决方案。对于复杂的应用，根据场景可搭配Redux来使用。

## Virtual DOM
真实页面对应一个DOM树。

传统页面开发，更新页面，都是手动操作DOM来更新。


在WEB前端中，性能消耗最大的就是DOM操作，而且这部分代码会很难维护。

React是把真实的DOM树转成Js对象树，也就是Virtual DOM。

每次数据更新后，重新计算Virtual DOM，和上一次做对比，对发生变化的部分，做批量更新。React有`shouldComponentUpdate`声明周期回调，来减少数据变化后不必要的对比过程，以保证性能。


## 3、函数式编程
React把过去不断重复构建UI的过程抽成组件，在给定参数的情况下，约定渲染对应的UI界面。

React充分利用很多函数式方法去减少冗余代码。

函数式编程才是React的精髓。





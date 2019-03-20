---
title: React丨第一季 第4集 React数据流
date: 2019-03-20 20:55:55
categories: "React"
tags: 
  - React
---

# React数据流

在React中，数据是从上向下单向流动的，即从父组件到子组件。这个原则让组件之间的关系变得简单且可预测。

`state`和`props`是React组件中最重要的概念。

如果顶层组件初始化`props`，那么React会向下遍历整棵组件数，重新尝试渲染所有相关的子组件。

`state`只关心每隔组件自己的内部的状态，这些状态只能在组件内部改变。

把组件看成一个函数，那么它接收了`props`作为参数，内部由`state`作为函数的内部参数，返回一个虚拟DOM的实现。



## state
管理组件的内部状态，React中，把这类状态称为`state`。

当组件内部使用库内置的`setState`方法时，最大的表现行为就是该组件会重新渲染。

```js
handleClick(e) {
    e.preventDefault()
    this.setState({
        count: this.state.count + 1
    })
}
render() {
    return (
        <div>
            <p>{this.state.count}</p>
            <a href="#" onClick={this.handleClick}>更新</a>
        </div>
    )
}
```
注意：`setState`是一个异步的方法，一个生命周期内所有的`setState`方法会合并操作。

`state`很棒，但项目的深入，并不推荐滥用`state`，过多的内部状态会让数据流混乱，程序难易维护。

React组件设计，分别有如下两种组件：
- 智能组件
- 木偶组件


## props
`props`是`properties`的缩写。

`props`是React用来让组件之间互相联系的一种机制 ，就像方法的参数一样。

React的单向数据流，主要的流动管道就是`props`。

`props`本身是不可变的。当改变`props`的原始值时，React会报错，组件的`props`一定来于默认属性或通过父组件传递而来。

如果要渲染一个对`props`加工后的值，最简单的方法就是使用局部变量或直接在jsx中计算结果。


### 子组件prop
在React中有一个重要且内置的`prop` -- `children`，它代表组件的子组件集合。

```js
render() {
    return (
        <div>
            {React.Children.map(this.props.children, (child) => {...})}
        </div>
    )
}
```
这种调用方式称为子组件。它指的是组件内的子组件是通过动态计算得到的。


###  组件porps


### 用function prop 与父组件通信
对于state来说，它的通信集中在组件内部；
对于props来说，它的通信是父组件向子组件的传播。 

```js
handleClick(index) {
    this.props.onClick({index})
}
```
通过点击事件`handleClick`触发了`onClick` prop回调函数给父组件必要的值。


### propTypes
JS不是强类型语言。正是一个开发时约束的问题。React推处了`propTypes`。

`propTypes`用于规范`props`的类型与必需的状态。如果组件定义`propTypes`，那么在开发环境下，就会组件的`props`值的类型作检查，如果不匹配，React实时在控制台报警告。在生产环境下，这是不会进行检查的。

```js
static propTypes = {
    classPrefix: React.PropTypes.string,
    activeIndex: React.PropTypes.number,
    onChange: React.PropTypes.func,
    isLogin: React.PropTypes.bool
}
```
`propTypes`有很多类型支持，不仅有基础类型，还包括枚举和自定义类型。
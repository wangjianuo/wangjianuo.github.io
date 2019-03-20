---
title: React丨第一季 第3集 React组件
date: 2019-03-20 19:48:04
categories: "React"
tags: 
  - React
---



# React组件
React组件基本由3个部分组成：属性(props)、状态(state)、生命周期。

官方在React组件构建上提供了三种不同的方法：
- React.createClass
- ES6 Classes
- 无状态函数(stateless function)


## `React.createClass`
这种方式是构建组件是React最传统、也是兼容最好的方法。在0.14版本发布之前，这一直都是React官方唯一指定的组件写法。
```js
const Button = React.createClass({
  getDefaultProps() {
    return {
      color: 'blue',
      text: 'Confirm'
    }
  },
  render(){
    const {color, text} = this.props
    return (
      <button className=`{btn btn-${color}}`>
        <em>{text}</em>
      </button>
    )
  }
})
```


## `ES6 Classes`
```js
import React, {Component} from 'React'
class Button extends Component {
  constructor(props) {
    super(props)
  }
  static defaultProps = {
    color: 'blue',
    text: 'Confirm'
  }
  render(){
    const {color, text} = this.props
    return (
      <button className=`{btn btn-${color}}`>
        <em>{text}</em>
      </button>
    )
  }
}
```
在React应用中，极少让子类去继承功能组件。试想在UI层面小的修改就会影响到整体交互或样式，牵一发而动全身，用继承来抽象往往是事倍功半。
在React组件开发中，常用的方式将组件拆分到合理的粒度，用组合的方式合成业务组件。

## 无状态函数
在0.14版本之后新增的，且官方推崇。
```js
function Button({color='blue', text='Confirm'}) {
  return (
      <button className={`btn btn-${color}`}>
        <em>{text}</em>
      </button>
  )
}
```
无状态组件，只传入props和context两个参数；它不存在state，也没有生命周期方法，render方法。
不过像propTypes和defaultProps还是可以通过向方法设置静态属性来实现的。

在合适的情况下，都应该且必须使用无状态组件。无状态组件不像上述两种方法在调用的时会创建新实例，它创建时始终保持了一个实例，避免了不必要的检查和内存分配，做到了内部优化。

---
title: React丨Hello React 1
date: 2019-03-18 11:38:04
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

[1] 

在WEB前端中，性能消耗最大的就是DOM操作，而且这部分代码会很难维护。

React是把真实的DOM树转成Js对象树，也就是Virtual DOM。

[2]

每次数据更新后，重新计算Virtual DOM，和上一次做对比，对发生变化的部分，做批量更新。React有`shouldComponentUpdate`声明周期回调，来减少数据变化后不必要的对比过程，以保证性能。


## 3、函数式编程
React把过去不断重复构建UI的过程抽成组件，在给定参数的情况下，约定渲染对应的UI界面。

React充分利用很多函数式方法去减少冗余代码。

函数式编程才是React的精髓。


# JSX语法

## JSX的由来
React与其他JS模板语言不同之处，在于React是通过创建与更新虚拟元素来管理整个虚拟DOM。

虚拟元素：可以理解为真实元素的对应，它的构建与更新都是在内存中完成的，并不会真正渲染到DOM中去。

在React中创建的虚拟元素，可以分为两类：DOM元素（DOM element）和组件元素（component element），
对应着原生的DOM元素和自定义元素。而JSX与创建元素的过程有莫大的关联。

### DOM元素
WEB页面从`HTML`转`JSON`对象。示例：

- HTML
```html
<button class="btn btn-blue">
  <em>hello jsx</em>
</button>
```

- JSON
```js
{
  type: 'button',
  props: {
    className: 'btn btn-blue',
    children: [{
      type: 'em',
      props: {
        children: 'hello jsx'
      }
    }]
  }
}
```
这样就可以在JS中创建虚拟DOM了。


### 组件元素
- 封装`button`元素，构建按钮的公共方法：
```js
const Button = ({color, text}) => {
  return {
    type: 'button',
    props: {
      className: `btn btn-${color}`,
      children: {
        type: 'em',
        props: {
          children: `${text}`
        }
      }
    }
  }
}
```
当要生成DOM元素中具体的按钮时，就可以使用下面方式来创建：
```js
Button({color: 'blue', text: 'hello jsx'})
```

上述构建`Button`的公共方法，也是作为元素而存在，方法名对应了DOM的元素类型，参数对应DOM元素的属性，这样它就具备了元素的两大必要条件，这样的有元素，就是自定义类型元素，或组件元素。用`JSON`描述它：
```js
{
  type: Button,
  props: {
    color: 'blue',
    childre: 'hello jsx'
  }
}
```
这也是React的核心思想之一。我们可以让元素彼此嵌套或混合。这些层层封装的组件元素，就是所有的React组件，最终我们可以用递归的方式构建出完全的DOM元素树。


如果是大量的表达式，就很难维护了。`jsx`语法就应运而生了。
`jsx`是将`html`语法直接加入到`js`代码中，在通过翻译器转换到纯`js`后，有浏览器执行。
在实际开发中，`jsx`在产品打包阶段都已经编译成纯`js`，不会带来任何副作用，反而回让代码更加直观并已于维护。

现在已全部采用`Babel`的`jsx`编译器实现。`Babel`作为专门为`JS`语法编译工具，达到了“一处配置，统一运行”的目的。

- jsx
```js
cosnt DeleteAccount = () => {
  <div>
    <p>Are you sure ?</p>
    <DangerButton>Confirm</DangerButton>
    <Button color="bule">Cancel</Button>
  </div>
}
```
- 用`Babel`转移成`React`可以执行的代码
```js
var DeleteAccount = function DeleteAccout() {
  return React.createElement(
    'div',
    null,
    ReactElement(
      'p',
      null,
      'Are you sure ?'
    ),
    React.createElement(
      DangerButton,
      null,
      'Confirm'
    ),
    React.createElement(
      Button,
      {color: 'blue'},
      'Cancel'
    )
  )
}
```
上述，除了在创建元素使用了`React.createElement`创建之外，其他的和`JSON`的结构一样的。


## jsx基本语法

- xml基本语法
```js
定义标签时，只允许被一个标签包裹。
标签一定要闭合。
```

- 元素类型
```js
-- 元素类型
DOM元素：小写
组件元素：大写

-- 注释
{/* 哈哈哈 */}

-- DOCTYPE
DOCTYPE头是一个非常特殊的标志，一般会在使用React作为服务端渲染时用到。在HTML中DOCTYPE是没有闭合,也就是说我们无法渲染它。
```

- 元素属性
```js
class属性改为className
for属性改为htmlFor


//-- 展开属性
const component = <Component name={name} value={value} />

const data = {name:'foo', value:'bar'}
const component = <Component {...data} />

// 反模式
const component = <Component />
component.props.name = name;
component.props.value = value;
上面的写法，React不能帮你检查属性类型（propTypes），这样即使组件的属性类型有错误，不能得到清晰的错误提示


//-- 自定义HTML属性
组件的最终目的是输出虚拟DOM，也就是需要被渲染到界面的机构。其狠心渲染的方式，就是render方法。它是React组件生命周期的一部分，也是最核心的函数之一。
```

- JS属性表达式
```js
const person = <Person name={window.isLoggedIn ? window.name: ''} />

const content = <Container>
  {window.isLoggedIn ? <Nav/>: <Login/>}
</Container>
```

- HTML转义
```js
React会将所有要显示的DOM的字符串转义，防止XSS。

React提供了dangerousSetInnerHtml属性。它的作用是避免React转义字符，在确定必要的情况下可以使用它: <div dangerouslySetInnerHtml={{__html: 'cc &copy; 2019'}} />
```


# React组件
React组件基本由3个部分组成：属性(props)、状态(state)、生命周期。

官方在React组件构建上提供了三种不同的方法：
- React.createClass
- ES6 Classes
- 无状态函数(stateless function)


## `React.createClas`
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




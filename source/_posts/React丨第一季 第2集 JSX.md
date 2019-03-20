---
title: React丨第一季 第2集 JSX
date: 2019-03-20 18:44:31
categories: "React"
tags: 
  - React
---



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

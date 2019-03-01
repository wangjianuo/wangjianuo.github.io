---
title: JavaScript丨selectpicker动态赋值不显示
date: 2019-02-15 14:50:52
categories: "JavaScript"
tags: 
  - JavaScript
  - Bootstrap
---

### 解决办法

封装一个自定义方法
功能：重新赋值，刷新，设置默认值。
```javascript
/**
  * bs select append 扩展
  * @param {*} id 组件id
  * @param {*} opStr option
  * @param {*} selectedVal 默认值
  */
appendSelectExt: function (id, opStr, selectedVal) {
    $(`#${id}`).empty()
    $(`#${id}`).append(opStr)
    $(`#${id}`).selectpicker('refresh')
    $(`#${id}`).selectpicker('val', selectedVal)
    //$(`#${id}`).selectpicker('render')
}
```

调用方法
```javascript
let opStr = ''
let selectedVal = ''
for (let i = 0; i < data.length; i++) {
    const item = data[i]
    let selFlag = i === 0 ? 'selected' : ''
    if (i === 0) selectedVal = item.code
    opStr += `<option value="${item.code}" ${selFlag}>${item.name}</option>`
}
// bs selectpicker ext append
utils.appendSelectExt('nodeType', opStr, selectedVal)
```


### 参考资料
```javascript
https://blog.csdn.net/u012083407/article/details/70305755
https://www.cnblogs.com/mgzy/p/5755915.html
https://www.cnblogs.com/daijun/p/7872663.html
```
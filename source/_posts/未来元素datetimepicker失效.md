---
title: 未来元素datetimepicker失效
date: 2019-02-15 14:08:16
categories: "JavaScript"
tags: 
  - JavaScript
  - Bootstrap
---



### 一、场景描述

1、页面加载初始化`datetimepicker`组件

```javascript
$(".form_datetime").datetimepicker({
  format: 'yyyy-mm-dd',//显示格式
  todayHighlight: 1,//今天高亮
  minView: "month",//设置只显示到月份
  language: "zh-CN",
  startView: 2,
  forceParse: 0,
  showMeridian: 1,
  autoclose: 1//选择后自动关闭
})
```

2、根据数据，动态创建多个`datetimepicker`组件

```javascript
/**
  * 动态创建Select元素
  */
dynCreateSelectDom: function (item) {
    let ops = ''
    item.option.map(m => {
        ops += `<option value="${m.code}" ${m.code === item.value ? 'selected' : ''}>${m.name}</option>`
    })
    return `
        <div class="col-sm-6 com">
            <div class="input-group">
                <span class="input-group-addon">
                    ${item.name}
                </span>
                <select id="${item.id}" onchange="handleDynChange(this)" class="form-control">
                    ${ops}
                </select>
            </div>
        </div>
    `
},
```

3、发现`datetimepicker`组件失效，像一个普通的文本组件，无时间日期效果。



### 二、解决办法

1、把初始化`datetimepicker`组件代码，封装为一个方法

```javascript
bindDatetimepicker: function () {
    $(".form_datetime").datetimepicker({
        format: 'yyyy-mm-dd',//显示格式
        todayHighlight: 1,//今天高亮
        minView: "month",//设置只显示到月份
        language: "zh-CN",
        startView: 2,
        forceParse: 0,
        showMeridian: 1,
        autoclose: 1//选择后自动关闭
    })
},
```

2、在动态创建多个`datetimepicker`组件后，再调用`bindDatetimepicker`方法。





### 三、参考资料

```javascript
https://blog.csdn.net/sunnyzyq/article/details/77460355
```


































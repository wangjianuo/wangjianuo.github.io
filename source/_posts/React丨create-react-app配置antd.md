---
title: React丨create-react-app配置antd
date: 2019-03-01 11:35:12
categories: "React"
tags: 
  - React
  - Antd
---

### 安装依赖
```js
yarn add react-app-rewired customize-cra babel-plugin-import
```

### 配置`package.json`
```js
"scripts": {
-   "start": "react-scripts start",
+   "start": "react-app-rewired start",
-   "build": "react-scripts build",
+   "build": "react-app-rewired build",
-   "test": "react-scripts test",
+   "test": "react-app-rewired test",
}
```


### 修改默认配置
在项目根目录创建一个 `config-overrides.js` 用于修改默认配置。
```js
const {
    override,
    fixBabelImports
  } = require("customize-cra");
  
  module.exports = override(
    fixBabelImports("babel-plugin-import", {
      libraryName: "antd",
      style: "css"
    })
  );
```


### 测试
```js
import { Button } from 'antd';

ReactDOM.render(
  <div>
    <Button type="primary">Primary</Button>
    <Button>Default</Button>
    <Button type="dashed">Dashed</Button>
    <Button type="danger">Danger</Button>
  </div>,
  mountNode
);
```

### 备注
`babel-plugin-import` 是一个用于按需加载组件代码和样式的 babel 插件。


### 参考
```js
https://ant.design/docs/react/use-with-create-react-app-cn
```



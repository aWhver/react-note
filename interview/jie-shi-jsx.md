# 解释JSX

React中用JSX语法描述视图(View)，JSX 是一个看起来很像 XML 的 JavaScript 语法扩展。当然你也可以选择直接使用`React.createElement`，但是写起页面来手速会很慢。

React 17以前，React中如果使用JSX，则必须导入React，否则会报错，这是因为**旧的 JSX 转换**会把 JSX 转换为 `React.createElement(...)` 调用。

```javascript
import React from 'react';

export default function App(props) {
  return <div>app</div>;
}
```

当然，这并不完美，除了增加了学习成本，还有无法做到的[性能优化和简化](https://link.juejin.cn/?target=https://github.com/reactjs/rfcs/blob/createlement-rfc/text/0000-create-element-changes.md#motivation)， 如createElement里还要动态做children的拼接、依赖于React的导入等等。

而React 17带来了改变，可以让我们单独使用 JSX 而无需引入 React。这是因为新的 JSX 转换**不会将 JSX 转换为 React.createElement**，而是自动从 React 的 package 中引入新的入口函数并调用。另外此次升级不会改变 JSX 语法，旧的 JSX 转换也将继续工作。

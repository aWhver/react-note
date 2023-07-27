# 什么是HOC / 如何”修改“组件属性props

props是属性，用于父子通信，且props不可修改。

如果对于组件的props不满意，可以使用HOC返回一个新的组件。

HOC：高阶组件，它是个函数，接收组件作为参数，返回一个新的组件，是Hooks出现之前，常见的复用逻辑的手段。如react-reudx的connect、router5的withRouter、AntD3的create等。

connect源码示例：[https://github1s.com/reduxjs/react-redux/blob/HEAD/src/components/connect.tsx#L771](https://github1s.com/reduxjs/react-redux/blob/HEAD/src/components/connect.tsx#L771)

inject源码示例：[https://github1s.com/mobxjs/mobx/blob/HEAD/packages/mobx-react/src/inject.ts](https://github1s.com/mobxjs/mobx/blob/HEAD/packages/mobx-react/src/inject.ts)

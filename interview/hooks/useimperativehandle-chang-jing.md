# useImperativeHandle场景

[https://zh-hans.react.dev/reference/react/useImperativeHandle#exposing-your-own-imperative-methods](https://zh-hans.react.dev/reference/react/useImperativeHandle#exposing-your-own-imperative-methods)

让用户可以把一个变量当做ref暴露出来， 经常和forwardRef一起使用。

1. 把ref暴露给父组件。比如AntD 4/5中支持类组件实现时候用到了。
2. 暴露一个重要方法给父组件。

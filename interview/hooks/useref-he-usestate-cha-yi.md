# useRef和useState差异

`useRef`用于记录一个值，这个值在函数组件卸载前，指的都是同一个引用。比如上面图中的例子，组件卸载前，我都想要的是这一个history，而不是每次都新建一个，新建就不是更新了，因此使用了useRef做了记录。这个值记录在`hook.memoizedState`上。

`useState`用于定义一个state，当state改变，组件更新。state也是记录在`hook.memoizedState`上。

![](<../../.gitbook/assets/image (8).png>)

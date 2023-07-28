# 为什么Hook出现之后，函数组件中可以定义state，保存在了哪里

hooks出现之前，函数组件内部无法定义state，主要是因为函数组件每次更新，定义在函数体里的值都要重新初始化，没法保存。

而hooks提供的`useState`或者`useReducer`可以让函数组件在组件内定义`state`，每一个hook都有个对应的hook对象，这个对象上会存储状态值、reducer等值，这个hook对象又以单链表的数据结构存在`fiber`上，而`fiber`是React的虚拟DOM，存在于内存中。

```jsx
import React, { useState } from 'react';

let state = 0

function Example() {
  // 声明一个新的叫做 “count” 的 state 变量
 const [count, setCount] = useState(0);  

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(0)}>
        Click me {count}
      </button>
    </div>
  );
}
```

# useState与useReducer

### 两者的差异

这两个Hook都是用于函数组件内部定义状态，`useReducer`可以接收一个reducer函数，意味着可以把状态修改逻辑放在reducer函数中，还可以多次复用。

因此，对于简单的状态值定义，可以使用`useState`，如果状态值修改逻辑复杂，想要抽离出来或者复用修改逻辑，可以选择useReducer。

```jsx
const [state, setState] = useState(initialState);
const [state, dispatch] = useReducer(reducer, initialArg, init);
```

使用useState的时候，如果state不发生变化，组件不会更新。但是useReducer反之。

使用方式：

```jsx
import {useState, useReducer} from "react";

const third = (x) => x;

export default function App() {
  const [first, setFirst] = useState(0);
  const [second, setSecond] = useReducer((x) => x, 0, third);

  console.log("render");

  return (
    <div className="App">
      <button onClick={() => setFirst(0)}>first：{first}</button>
      <button onClick={() => setSecond(0)}>second: {second}</button>
    </div>
  );
}
```

### 这2个hook返回数组的结构而不是对象或者其他结构？

因为函数组件中可以多次使用useState/useReducer，可以定义多个不同state，数组可以一次返回state、setState，命名交给用户自定义。如果是对象再解构，那么state的命名就要写死了，用户想定义多个state的时候，还得自己改写名字，太麻烦了

### 两者的原理

用于定义函数组件内部状态，状态变更，组件更新。

状态值存储在函数组件的fiber的hook.memoizedState上。

组件初次渲染阶段：

1. 把state初始值存储hook.memoizedState
2. 初始化更新队列，存储到hook.queue上
3. 定义dispatch事件，并存储到hook.queue上。（注意现在useState/useReducer的dispatch事件并不相同）
4. 返回 `[hook.memoizedState, dispatch];`

组件更新阶段，（批量更新）：

1. 检查是否有上次未处理的更新，如果有，则添加到更新队列上（更新队列是个环形链表）
2. 循环遍历更新队列，得到newState
3. 把最终得到的newState赋值到hook.memoizedState上
4. 返回 `[hook.memoizedState, dispatch];`

执行useReducer的dispatch事件：

// dispatchReducerAction

创建一个update对象，存储到更新队列中，然后执行scheduleUpdateOnFiber函数，去更新。

执行useState的dispatch事件：

// dispatchSetState

创建一个update对象，如果新的state和老的state相同，则退出更新，即进入bailout。否则存储update到更新队列中，然后执行scheduleUpdateOnFiber函数，去更新。

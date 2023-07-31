# Context

### Context使用方法

Context使用场景：当祖先组件想要和后代组件快速通信。

1. 创建Context对象，可以设置默认值。如果缺少匹配的Provider，那么后代组件将会读取这里的默认值。
2. Provider传递value给后代组件。
3. 后代组件消费value：
   1. contextType: 只能在类组件且只能订阅单一的`Context`来源
   2. useContext: 只能在函数组件或自定义`Hook`中
   3.  Consumer组件，无限制



### Context原理

总览

![](<../.gitbook/assets/image (2).png>)

1. 创建Context对象：createContext
2. Provider传递value 给后代组件 updateContextProvider
   1. 获取props.value
   2. pushProvider(worklnProgress, context, newValue);
   3. 比较newValue和oldValue
      1. 不变 —— 退出：bailoutOnAlreadyFinishedWork
      2. 变了 一 一 检查匹配的子孙组件，调度更新：propagateContextChange(worklnProgress，context，renderLanes);
   4. reconcileChildren(详情看要点的diff)
3. 子孙组件消费前 一 prepareToReadContext(worklnProgress,renderLanes)；一一 初始化currentlyRenderingFiber等值，有点像Hooks
4. 子孙组件消费value
   1. contextType : 只能用在类组件且只能订阅单一的Context来源 — context = readContext(contextType); 来自context.\_currentValue&#x20;
   2. useContext：只能用在函数组件或者自定义Hook中 一 一 context = readContext(context);
   3. Consumer组件：
      1. prepareToReadContext
      2. const newValue = readContext(context);
      3. newChildren = render(newValue);
      4. reconcileChildren(current, worklnProgress, newChildren, renderLanes);&#x20;
5. 其他：Context中的栈

#### CreateContext

```typescript
import {ReactContext} from "shared/ReactTypes";
import {REACT_CONTEXT_TYPE, REACT_PROVIDER_TYPE} from "shared/ReactSymbols";

export function createContext<T>(defaultValue: T): ReactContext<T> {
  const context: ReactContext<T> = {
    $$typeof: REACT_CONTEXT_TYPE,
    _currentValue: defaultValue,
    Provider: null,
    Consumer: null,
  };

  context.Provider = {
    $$typeof: REACT_PROVIDER_TYPE,
    _context: context,
  };

  context.Consumer = context;

  return context;
}
```

全局变量：

`currentlyRenderingFiber`：记录当前可以消费Provider value的后代组件，dependencies属性记录context单链表。（这个地方很像hooks的实现方式)

`valueCursor`：栈，记录每一层Provider value值，有点像括号匹配。

`lastContextDependency`:记录最后一个`currentlyRenderingFiber`上的最后一个context

1. 能消费Provider value后代组件类型只能是函数组件、类组件、Consumer组件、forwardRef组件，因此在这些组件开始更新的时候，会执行一个`prepareToReadContext`函数，用于记录当前组件的fiber，记录到一个叫做`currentlyRenderingFiber`全局值中。
2. 当执行到Provider组件的更新函数时，执行`pushProvider`函数，用于把value存到一个栈中，当这个Provider组件执行完毕，则把这个value出栈。
   1.  后代组件消费：readContext(context)，简版实现如下：

       ```typescript
       export function useContext<T>(context: ReactContext<T>): T {
         return readContext(context);
       }
       ```
3. Provider组件更新完成之后，把value出栈。

#### Provider组件

```typescript
function updateContextProvider(current: Fiber | null, workInProgress: Fiber) {
  const context = workInProgress.type._context;
  const newValue = workInProgress.pendingProps.value;

  pushProvider(context, newValue);

  // context newvalue，存储
  workInProgress.child = reconcileChildren(
    current,
    workInProgress,
    workInProgress.pendingProps.children
  );

  return workInProgress.child;
}
```

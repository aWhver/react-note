# hook只能在React函数顶层作用域使用的原因

Hooks使用规则：**不要在循环，条件或嵌套函数中调用Hook， 确保总是在你的React 函数的最顶层以及任何return 之前调用他们。**

React中每个组件都有一个对应的FiberNode，其实就是一个对象，这个对象是有个属性叫memoizedState。当组件是函数组件的时候，fiber.memoizedState上存储的就是Hooks单链表。

单链表的每个hook节点没有名字或者key，因为除了它们的顺序，我们无法记录它们的唯一性。因此为了确保某个Hook是它本身，我们不能破坏这个链表的稳定性。

试想一下，如果在if内部使用Hook，那么组件更新的时候，hook3可能就变成了hook0，这不就乱了，毕竟每个hook都存了很多信息，比如state、effect、依赖项等。

在嵌套函数内部使用Hook，我们也不难确保这个嵌套函数里的Hook到底什么时候调用，所以还是不能确定fiber.memoizedState单链表上hook节点的稳定性。

Hook类型定义如下：

```typescript
export type Hook = {
  memoizedState: any,  // 不同情况下，取值也不同，useState/useReducer时候存的是state，useEffect/useLayoutEffect时候存的是effect单向循环链表
  baseState: any,
  baseQueue: Update<any, any> | null, 
  queue: any,
  next: Hook | null, // 下一个Hook，如果为null，证明这是最后一个hook
};
```

```typescript
fiber.memorizedState(hook0)-> next(hook1)-> next(hook2)->next(hook3)->next(hook4)->next(hook5)(workInProgressHook)

const workInProgressHook = null
//let workInProgressHook = null
//hook3


function FunctionalComponent () {
  const [state1, setState1] = useState(0)

  const [state2, setState2] = useState(1)
  
  const [state3, setState3] = useState(2)

  
  return ...
}

hook1 => Fiber.memoizedState
state1 === hook1.memoizedState
hook1.next => hook2
state2 === hook2.memoizedState
hook2.next => hook3
state3 === hook2.memoizedState
```

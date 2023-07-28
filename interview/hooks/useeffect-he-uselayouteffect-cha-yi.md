# useEffect和useLayoutEffect差异

### 两者的差异

`useEffect(setup, dependencies?)`

该 Hook 接收一个包含命令式、且可能有副作用代码的函数。

在函数组件主体内（这里指在 React 渲染阶段）改变 DOM、添加订阅、设置定时器、记录日志以及执行其他包含副作用的操作都是不被允许的，因为这可能会产生莫名其妙的 bug 并破坏 UI 的一致性。

使用 useEffect 完成副作用操作。赋值给 useEffect 的函数会在组件渲染到屏幕之后**延迟**执行。默认情况下，effect 将在每轮渲染结束后执行，但你可以选择让它 在只有某些值改变的时候才执行。

`useLayoutEffect`函数签名与 `useEffect` 相同，但它会在所有的 DOM 变更之后同步调用 effect。可以使用它来读取 DOM 布局并同步触发重渲染。在浏览器执行绘制之前，`useLayoutEffect` 内部的更新计划将被同步刷新。

`setup`函数返回值在源码中称为destroy，如果destroy是函数，则会在组件更新或者卸载前执行。经常用于清除订阅、清除定时器等操作。

### 其中的延迟、同步是什么(任务调度)

这里所谓的延迟、同步，指的是React任务调度中的任务调度，所谓延迟就是`useEffect`的effect不与组件渲染使用同一个任务调度函数，而是再单独调用一次任务调度函数，即用的不是一个task。因为如果effect和组件渲染使用同一个task，那么effect势必会加长这个task的执行时间，阻碍组件渲染。同理，`useLayoutEffect`所谓同步，指的是`useLayoutEffect`的effect和组件渲染使用同一个task，那么就会阻碍组件渲染。

因此大多数情况下，尽可能使用标准的 useEffect 以避免阻塞视觉更新。举个需要使用`useLayoutEffect`的场景，AntD4/5 Form的底层form，也就是rc-feild-form中的field如果用函数组件实现，就得这么写，如果换成`useEffect`，你会发现组件没有初始值，这是因为使用`useEffect`的时候，订阅会被延迟，那么组件接收到store变更，却没有要执行组件更新的操作，因为这个时候订阅还没有发生。

![](<../../.gitbook/assets/image (1) (2).png>)

同样的事情在react-router中也存在，因此都是适用的useLayoutEffect，如下图所示。![](<../../.gitbook/assets/image (2).png>)

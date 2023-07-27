# React18新特性有哪些，并解释

### 正式支持Concurrent <a href="#concurrent" id="concurrent"></a>

Concurrent最主要的特点就是**渲染是可中断的**。以前是不可中断的，也就是说，以前React中的update是同步渲染，在这种情况下，一旦update开启，在任务完成前，都不可中断。

#### 为什么要这样修改：react-dom/client中的createRoot取代以前的ReactDOM.render

以前root只存在于React内部，现在用户也可以复用root了。

```React
const root = createRoot(document.getElementById("root"));
root.render(jsx);
```

### 自动批量处理 Automatic Batching / setState是同步还是异步 <a href="#automaticstate" id="automaticstate"></a>

可同步可异步。

在React18以前，可以把setState放在promises、setTimeout或者原生事件中实现同步。

而所谓异步就是个批量处理，为什么要批量处理呢。举个例子，老人以打渔为生，难道要每打到一条沙丁鱼就下船去集市上卖掉吗，那跑来跑去的成本太高了，卖鱼的钱都不够路费的。所以老人都是打到鱼之后先放到船舱，一段时间之后再跑一次集市，批量卖掉那些鱼。对于React来说，也是这样，state攒够了再一起更新。

但是以前的React的批量更新是依赖于合成事件的，到了React18之后，state的批量更新不再与合成事件有直接关系，而是自动批量处理。

```TypeScript
// 以前: 这里的两次setState并没有批量处理，React会render两次
setTimeout(() => {
  setCount(c => c + 1);
  setFlag(f => !f);
}, 1000);

// React18: 自动批量处理，这里只会render一次
setTimeout(() => {
  setCount(c => c + 1);
  setFlag(f => !f);
}, 1000);
```

所以如果你项目中还在用setTimeout之列的“黑科技”实现setState的同步的话，升级React18之前，记得改一下\~

虽然建议setState批量处理，但是如果你有一些其它理由或者需要应急，想要同步setState，这个时候可以使用`flushSync`，下面的例子中，log的count将会和button上的count同步:

```TypeScript
// import { flushSync } from "react-dom";

changeCount = () => {
  const { count } = this.state;
  
  flushSync(() => {
    this.setState({
      count: count + 1,
    });
  });
  
  console.log("改变count", this.state.count); //sy-log
};

// <button onClick={this.changeCount}>change count 合成事件</button>
```

### Suspense

支持Suspense，可以“等待”目标UI加载，并且可以直接指定一个加载的界面（像是个 spinner），让它在用户等待的时候显示。

```React
<Suspense fallback={<Spinner />}>
  <Comments />
</Suspense>
```

### 错误处理边界 <a href="#boundary" id="boundary"></a>

在 Suspense 中，获取数据时抛出的错误和组件渲染时的报错处理方式一样——你可以在需要的层级渲染一个[错误边界](https://link.juejin.cn/?target=https://zh-hans.reactjs.org/docs/error-boundaries.html)组件来“捕捉”层级下面的所有的报错信息。

```TypeScript
export default class ErrorBoundaryPage extends React.Component {
  state = {hasError: false, error: null};
  static getDerivedStateFromError(error) {
    return {
      hasError: true,
      error,
    };
  }
  render() {
    if (this.state.hasError) {
      return this.props.fallback;
    }
    return this.props.children;
  }
}

```

### SuspenseList

用于控制Suspense组件的显示顺序。目前正式环境中还不支持，预计在18.X再支持。

### transition

React把update分成两种：

* **Urgent updates** 紧急更新，指直接交互，通常指的用户交互。如点击、输入等。这种更新一旦不及时，用户就会觉得哪里不对。
* **Transition updates** 过渡更新，如UI从一个视图向另一个视图的更新。通常这种更新用户并不着急看到。

并引入了相关API，startTransition、useTransition与useDeferredValue。

`startTransition`可以用在任何你想更新的时候。但是从实际来说，以下是两种典型适用场景：

* 渲染慢：如果你有很多没那么着急的内容要渲染更新。
* 网络慢：如果你的更新需要花较多时间从服务端获取。这个时候也可以再结合`Suspense`。

`useTransition`：在使用startTransition更新状态的时候，用户可能想要知道transition的实时情况，这个时候可以使用它。

使用方法：`const [isPending, startTransition] = useTransition();`

`useDeferredValue`使得我们可以**延迟更新**某个不那么重要的部分，相当于参数版的transitions。

### 新的Hooks <a href="#newhooks" id="newhooks"></a>

`useTransition`：如上。

`useId`：用于产生一个在服务端与Web端都稳定且唯一的ID，也支持加前缀，这个特性多用于支持ssr的环境下。

`useSyncExternalStore`：此Hook用于外部数据的读取与订阅。

`useInsertionEffect`：函数签名同useEffect，但是它是在所有DOM变更前同步触发。主要用于css-in-js库，往DOM中动态注入`<style>` 或者 SVG `<defs>`。因为执行时机，因此不可读取refs。

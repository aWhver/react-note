# 组件常见的性能优化方式

### **复用组件**

在协调阶段，组件复用的前提是必须同时满足三个条件：同一层级下、同一类型、同一个key值。所以我们要尽量保证这三者的稳定性。

![](<../.gitbook/assets/image (1) (1) (1).png>)

常见错误：key=Math.random()

常见不规范写法：key=index

key值标记了节点在当前层级下的唯一性，因此我们尽量不要取值index，因为在同一层级下，多个循环的index容易重复。并且如果涉及节点的增加、删除、移动，那么key的稳定性将会被破坏，节点就会出现混乱现象。



### **减少组件的重新render**

组件重新render会导致组件进入协调，协调的核心就是我们常说的vdom diff，所以协调本身就是比较耗时的算法，因此如果能够减少协调，复用旧的fiber节点，那么肯定会加快渲染完成的速度。组件如果没有进入协调阶段，我们称为进入bailout阶段，意思就是这层组件退出更新。

让组件进入bailout阶段，有以下方法：

1. `shouldComponentUpdate`：`Component`类组件的一个生命周期，当用户定义的这个函数并且返回false，则进入bailout阶段。
2. `PureComponent`：更新前会自行浅比较新旧props与state是否改变，如果两者都没变，则进入bailout阶段。
3. `memo`:这里的`arePropsEqual`是个函数，用户可以自定义，如果没有定义，默认使用浅比较，比较组件更新前后的props是否相同，如果相同，则进入bailout阶段。![](<../.gitbook/assets/image (3) (1).png>)
4. `useMemo`可以缓存参数，可以对比`useCallback`使用，`useCallback(fn, deps)` 相当于 `useMemo(() => fn, deps)`。
5. 再说Context，Context本身就是一旦Provider传递的value变化，所有消费这个value的后代组件都要更新，因此应该尽量精简使用Context。

[React项目中Context value太大怎么办](https://juejin.cn/post/6924933173801549837#heading-1)

react源码函数浅比较：

```javascript
function shallowEqual(objA: mixed, objB: mixed): boolean {
  if (is(objA, objB)) {
    return true;
  }

  if (
    typeof objA !== 'object' ||
    objA === null ||
    typeof objB !== 'object' ||
    objB === null
  ) {
    return false;
  }

  const keysA = Object.keys(objA);
  const keysB = Object.keys(objB);

  if (keysA.length !== keysB.length) {
    return false;
  }

  // Test for A's keys different from B.
  for (let i = 0; i < keysA.length; i++) {
    const currentKey = keysA[i];
    if (
      !hasOwnProperty.call(objB, currentKey) ||
      !is(objA[currentKey], objB[currentKey])
    ) {
      return false;
    }
  }

  return true;
}
```

### 举例：OptimizingPage

```typescript
import {
  Component,
  PureComponent,
  useEffect,
  useReducer,
  useState,
  memo,
  useMemo,
} from "react";

export default function OptimizingPage(props) {
  const [arr, setArr] = useState([0, 1, 2, 3]);

  return (
    <div className="border">
      <h3>OptimizingPage</h3>
      <button
        onClick={() => {
          setArr([...arr, arr.length]);
        }}>
        修改数组
      </button>

      {arr.map((item) => {
        return <Child key={Math.random()} item={item} />;
      })}
    </div>
  );
}

function Child({item}) {
  useEffect(() => {
    return () => {
      console.log("destroy"); //sy-log
    };
  }, []);
  console.log("Child"); //sy-log
  return <div className="border">{item}</div>;
}

class ChildPureComponent extends PureComponent {
  render() {
    console.log("ChildPureComponent"); //sy-log
    return (
      <div className="border">
        <p>{this.props.item}</p>
      </div>
    );
  }
}

class ChildShouldComponentUpdate extends Component {
  shouldComponentUpdate(nextProps) {
    return this.props.item !== nextProps.item;
  }
  render() {
    console.log("ChildClass"); //sy-log
    return (
      <div className="border">
        <p>{this.props.item}</p>
      </div>
    );
  }
}

const ChildMemo = memo(
  function Child({item}) {
    console.log("ChildMemo"); //sy-log
    return <div className="border">{item}</div>;
  }
  // (prev, next) => {
  //   return prev.item === next.item;
  // }
);

function ChildUseMemo({item}) {
  console.log("ChildUseMemo"); //sy-log
  return useMemo(
    () => (
      <div className="border">
        {item}
        <Child2 />
      </div>
    ),
    []
  );
}

function Child2() {
  console.log("Child2"); //sy-log
  return (
    <div className="border">
      <h1>Child2</h1>
      <Child3 />
    </div>
  );
}

function Child3() {
  console.log("Child3"); //sy-log
  return (
    <div className="border">
      <h1>Child3</h1>
    </div>
  );
}

```

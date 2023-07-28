# 懒加载

懒加载或者按需加载，是一种很好的优化网页或应用的方式。这种方式实际上是先把你的代码在一些逻辑断点处分离开，然后在一些代码块中完成某些操作后，立即引用或即将引用另外一些新的代码块。这样加快了应用的初始加载速度，减轻了它的总体体积，因为某些代码块可能永远不会被加载。

[webpack方式](https://webpack.docschina.org/guides/lazy-loading/)

### [React.lazy](https://zh-hans.react.dev/reference/react/lazy)

#### `lazy(load)`

`lazy` 能够让你在组件第一次被渲染之前延迟加载组件的代码。

在组件外部调用 `lazy`，以声明一个懒加载的 React 组件:

```typoscript
import { lazy } from 'react';

const MarkdownPreview = lazy(() => import('./MarkdownPreview.js'));
```

**参数**

* `load`: 一个返回 [Promise](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global\_Objects/Promise) 或另一个 _thenable_（具有 `then` 方法的类 Promise 对象）的函数。在你尝试第一次渲染返回的组件之前，React 是不会调用 load 函数的。在 React 首次调用 `load` 后，它将等待其解析，然后将解析值渲染成 React 组件。返回的 Promise 和 Promise 的解析值都将被缓存，因此 React 不会多次调用 `load` 函数。如果 Promise 被拒绝，则 React 将抛出拒绝原因给最近的错误边界处理。

**返回值**

`lazy` 返回一个 React 组件，你可以在 fiber 树中渲染。当懒加载组件的代码仍在加载时，尝试渲染它将会处于 _暂停_ 状态。使用 [`<Suspense>`](https://zh-hans.react.dev/reference/react/Suspense) 可以在其加载时显示一个正在加载的提示。

### 使用方法

#### 使用 Suspense 实现懒加载组件

通常，你可以使用静态 [`import`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/import) 声明来导入组件：

```typescript
import MarkdownPreview from './MarkdownPreview.js';
```

如果想在组件第一次渲染前延迟加载这个组件的代码，请替换成以下导入方式：

```typescript
import { lazy } from 'react';

const MarkdownPreview = lazy(() => import('./MarkdownPreview.js'));
```

此代码依赖于 [动态 ](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/import)[`import()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/import)，可能需要你的打包工具或框架提供支持。

现在你的组件代码可以按需加载，同时你需要指定在它正在加载时应该显示什么。你可以通过将懒加载组件或其任何父级包装到 [`<Suspense>`](https://zh-hans.react.dev/reference/react/Suspense) 边界中来实现：

### react-router6中的懒加载

[https://reactrouter.com/en/main/route/lazy](https://reactrouter.com/en/main/route/lazy)

实例代码：[https://github1s.com/bubucuo/router6-nut/blob/lesson9-new/src/App.js](https://github1s.com/bubucuo/router6-nut/blob/lesson9-new/src/App.js)

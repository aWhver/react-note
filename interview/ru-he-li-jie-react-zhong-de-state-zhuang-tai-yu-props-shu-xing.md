# 如何理解React中的state(状态)与props(属性)

React UI = fn(state)

fn(x) = x+1

state是变量，一般情况下，state改变之后，组件会更新。

props是属性，用于父子通信，且props不可修改。

注：上面有两个**一般**，下面解释下二班情况：

状态改变，组件不会更新的情况：指更新被拦截，比如使用`PureComponent`组件、`Component`组件的`shouldComponentUpdate`、memo、useMemo。具体可参考下面的组件优化小节。

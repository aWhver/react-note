# Redux工作原理

Redux是React核心团队的Dan本人开发的状态管理库。

Redux基于函数式编程思想实现，集中式管理状态：

* **单一数据源**: 整个应用的 [全局 state](https://cn.redux.js.org/understanding/thinking-in-redux/glossary#state) 被储存在一棵 object tree 中，并且这个 object tree 只存在于唯一一个 [store](https://cn.redux.js.org/understanding/thinking-in-redux/glossary#store) 中。
* **State 是只读的**: 唯一改变 state 的方法就是触发 [action](https://cn.redux.js.org/understanding/thinking-in-redux/glossary)，action 是一个用于描述已发生事件的普通对象。
* **使用纯函数来执行修改**: 为了描述 action 如何改变 state tree，你需要编写纯的 reducers。

![](<../../.gitbook/assets/image (4).png>)

1. createStore 创建store 数据状态管理库
2. reducer 初始化、修改状态函数，**定义修改规则**
3. getState 获取状态值 （getter）
4. dispatch 提交更新 (setter)
5. subscribe 变更订阅 订阅state改变之后要做的事情，一般是组件更新

下面是一个简版的redux实现：



```javascript
export default function createStore(reducer, enhancer) {
    if (enhancer) {
        return enhancer(createStore)(reducer);
    }
    let currentState = 0;
    let listenerIdCounter = 0;
    let currentListenersMap = new Map();
    
    function getState() {
        return currentState;
    }
    
    
    function dispatch(action) {
        currentState = reducer(currentState, action);
        
        currentListenersMap.forEach(listener => listener());
        
        return action;
    }
    
    function subscribe(listener) {
        const listenerId = listenerIdCounter++;
        currentListenersMap.set(listenerId, listener);
        return () => {
            currentListenersMap.delete(listenerId);
        }
    }
    
    dispatch(`@redux/INIT${Math.random().toString(36).slice(2)}`);
    
    return {
        getState,
        dispatch,
        subscribe,
    }
}


```

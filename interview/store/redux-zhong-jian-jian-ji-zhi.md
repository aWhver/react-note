# Redux中间件机制

#### 如果不用中间件能否实现异步？

可以的。 middleware只是包装了 store 的 dispatch 方法。技术上讲，任何 middleware 能做的事情，都可能通过手动包装 dispatch 调用来实现，但是放在同一个地方统一管理会使整个项目的扩展变的容易得多。



![](<../../.gitbook/assets/image (1) (1) (1).png>)



![](<../../.gitbook/assets/image (2) (1) (1).png>)

Redux只是个纯粹的状态管理器，默认只支持同步，实现异步任务 比如延迟，网络请求，需要中间件的支持，比如常见的redux-thunk、redux-saga、redux-logger 、redux-promise等。

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

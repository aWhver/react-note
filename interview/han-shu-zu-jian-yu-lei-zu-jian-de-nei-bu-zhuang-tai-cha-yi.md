# 函数组件与类组件的内部状态差异

### 举例:下面2个console.log输出的值不一样

```javascript
export default ClassStatePage extends Component {
    construtor(props) {
        super(props);
        this.state = {
            count: 1,
        }
    }
    
    addCount = () => {
        const count = this.state.count;
        flushSync(() => {
            this.setState({
                count: count + 1,
            })
        });
        // 下面输出1
        console.log('count add', this.state.count);
    }
    
    render() {
        return <div>
            { this.state.count }
            <button onClick={this.addCount}>add 合成事件</button>
        </div>
    }
}

function FCStatePage(props) {
    const [count, setCount] = useState(0);
    function addCount() {
        flushSync(() => {
            setCount(count + 1);
        })
        // 下面输出0
        console.log("Hook's count", count)
    } 
    return <div>
            { count }
            <button onClick={addCount}>add</button>
        </div>
}
```

### 相同点

定义组件状态，并且状态更新，组件也要更新。

### 不同点

#### API不同

类组件：this.state、this.setState

函数组件：useState、useReducer

#### 存储方式不同

类组件的state存储在类组件实例与fiber上

函数组件的state存储在fiber的hook上

#### 更新不同

setState时候，类组件的新的state与旧的state合并对象，而函数组件是新的state覆盖老的state。并且，在useState的setState中，新旧state相同，则函数组件拒绝更新。

#### 组件中使用状态的时候，取值直接来源不同

类组件中使用状态，直接使用this.state，它的直接来源是类组件实例。

_注：fiber与类组件实例上的state保持同步。_

函数组件中使用状态，直接使用useState或者useReducer**函数返回值**数组的第0个元素，这个值来着fiber上的hook对象。

换句话说，如果想要获取类组件的新的状态值，可以直接访问this.state。

而如果想要获取函数组件中的一个新的状态值，必须重新执行useState或者useReducer函数，即必须执行函数组件。

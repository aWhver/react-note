# hooks产生的原因？解决了什么问题

React出现最初，99%组件都是类组件，因为我们可以在类组件内部使用状态、使用副作用等，而这些在函数组件内部都做不到，因此以前的函数组件基本上只能作为静态组件展示。因此，哪怕刚开始你使用了函数组件，随着项目迭代，可能也要再修改为类组件。

而在类组件中，有以下缺点：

1. **组件之间复用状态逻辑很难**：React 没有提供将可复用性行为“附加”到组件的途径（例如，把组件连接到 store），因此我们只能使用render props或者高阶组件(HOC)，而这很容易形成嵌套地狱。
2. **复杂组件变得难以理解**：类组件每个生命周期函数只能写一次，那么复杂组件的某个生命周期函数，如`componentDidMount`里，可能会存在很多不相关但是不得不组合在一起的代码。这个时候很容易产生bug，并且导致逻辑不一致，主要看着也臃肿。就像一家十口人，但是因为没有大房子，不得不挤在60平的小房子里，时间久了容易打架。
3. **难以理解**，比如初学者经常会找不到this。类组件也给工具带来一些问题，比如class 不能很好的压缩，并且会使热重载出现不稳定的情况。

而Hooks的引入，使得在函数组件内部定义状态、使用副作用等成为可能。并且Hooks还带来了其他好处，比如使得可以使用自定义Hook复用状态逻辑、也可以定义多个副作用处理函数。完美解决了类组件臃肿的问题。



这个可以对比下AntD3(HOC) 到AntD4/5 Form的演进(下面分别是使用HOC的AntD3 Form，和使用Hooks的AntD4/5 Form)：

![](<../../.gitbook/assets/image (6).png>)

![](<../../.gitbook/assets/image (7).png>)

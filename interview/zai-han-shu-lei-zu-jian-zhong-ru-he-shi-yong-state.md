# 在函数/类组件中如何使用state

*   组件内部state，适合只在本组件内部使用的state，优点就是灵活，随时定义即可用。缺点是难以实现在组件间共享。

    函数组件内部state可以使用useState、useReducer定义。

    类组件组件内部可以使用this.state定义，使用this.setState修改state。
* 组件外部state，也就是所谓的状态管理库。目前用的比较多的是Redux、MobX，DVA/Umi也算，但是DVA/Umi是在Redux的基础上封装的，AntD4/5 Form也是自己在外部定义的状态管理。

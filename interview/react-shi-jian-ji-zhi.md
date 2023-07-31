# React事件机制

我们在React中平常使用的onClick、onChange等写法，其实用的就是React中事件，我们通常称之为合成事件。

React自定义的合成事件机制，帮助用户解决了平台兼容性、事件委托等优化机制，用户只需关注写React本身就行了。

关于合成事件，React17曾发生过一次变化，在17以前，事件是委托在document上的，但是实际上，React项目是可以作为其它项目的子项目的。那么这个时候就会出现这样的问题，比如父项目在document层定义了事件，而React的事件也委托在document层，那么这两个事件就会产生交叉，出现bug。React17解决了这个问题，把事件委托在了自己的container层。

![](<../.gitbook/assets/image (1).png>)

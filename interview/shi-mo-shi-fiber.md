# 什么是fiber

Fiber是VDOM的一种表现形式。

传统的VDOM中，如React15 VDOM 和 Vue3 VDOM，当父节点有多个子节点的时候，父节点标记子节点的属性children是个数组，在更新VDOM的过程中，我们会按照深度优先遍历的方式，自上而下，自左而右，遍历子节点。

但是随着React的演进，传统VDOM被淘汰，Fiber取而代之，Fiber与传统VDOM的不同之处主要体现在它的结构上，如child、return、sibling属性的添加。关系如下图所示：

![](<../.gitbook/assets/image (12).png>)

这种传统VDOM到Fiber的演进，被称为stack reconciler到fiber reconciler的演进。

**为什么会发生这种改变呢？**

在传统的stack reconciler中，一旦任务开始，就无法停下，不管这个任务有多庞大，而这个时候如果来了更高优先级的任务，那么高优先级的任务无法得到立即处理，从而会出现卡顿现象。

**如何解决这种问题呢？**

做任务分解、给任务添加优先级，即实现增量渲染，把把渲染任务拆分成块，匀到多帧。这也就意味着一个任务执行完毕后，下个任务可能是它的下一个兄弟节点或者叔叔节点，这个时候Fiber的链表结构就派上用场了。

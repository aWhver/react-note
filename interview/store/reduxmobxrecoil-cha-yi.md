---
description: 这3个东西解决了什么问题？设计原理是什么？有什么优势
---

# Redux、Mobx、Recoil差异

这三者都是状态管理库，当组件内部状态无法满足需求的时候，比如需要实现组件间的状态共享，此时就可以定义一些外部状态，同时还要保证外部状态更新了，组件也随之更新，状态管理库就是做这件事情的。(这三者解决的问题与出现原因)

注：下面解释各个仓库特点以及各自优缺点。

Redux和Recoil都是Facebook内部开发的状态管理库，其中Redux是React核心团队的Dan本人开发的。

按照出现时机来说，先后分别为Redux、MobX、Recoil，这也基本决定了它们的市场占有率。

**Redux**基于函数式编程思想实现，集中式管理状态仓库，即一个项目中通常只定义一个store。

![](<../../.gitbook/assets/image (3) (1) (1).png>)

MobX是个响应式状态管理库，实现之初参考了Vue的设计思想。与Redux不同，MobX奉行分散式管理状态，即你可以定义多个store。其主要实现思路是拦截状态值的get与set函数，get时候的把状态值标记可观察变量，set的时候让组件更新。![](https://secure2.wostatic.cn/static/kGiMEcYgjUSnYSEkJzYH4p/image.png?auth\_key=1690429708-mjocgS3UsTQfDGxGho4wNu-0-c171af56f1f3e9b7a0601309bde9db29\&image\_process=resize,w\_760\&file\_size=545025)\
![](<../../.gitbook/assets/image (4) (1) (1).png>)

Redux、MobX本身都是js库，如果想要和React一起使用，经常需要再使用一个与React的绑定库，如react-redux、mobx-react或者mobx-react-lite。

**Recoil**与上面两者不同的是，Recoil本身就是React的状态管理库，不再需要与React的绑定库，属于一体机。在Recoil中，状态的定义是渐进式和分布式的。

![](<../../.gitbook/assets/image (5) (1).png>)

### 总结

Redux集中管理一个大状态，优点是比较专一，缺点是对于某些场景，比如不需要大量共享状态的时候，就不是特别灵活。而MobX和Recoil是可以分散式管理状态，因此相对Redux来说，状态比较单一来源。Recoil由于又多了一层selector，因此又可以渐进式定义状态。因此，就学习成本来说，一般是这样：Redux\<MobX\<Recoil。

目前国内来看，Redux的使用率是高于MobX的，比如Umi/DVA底层就是Redux。Recoil到现在一直没发正式版，主要是用在Facebook内部比较多。

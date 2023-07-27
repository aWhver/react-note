# 组件如何通信以及不同通信方式的特点

**父子组件**：props。缺点是不适合多层级的祖先与后代。

**兄弟组件**: 交给共同的父组件管理，如AntD3 Form。缺点是一个子组件更新，导致整个父组件更新。

**祖先/后代组件**：Context。不适合大量使用，因为一旦Context value发生改变，涉及所有组件都要更新，影响可能会比较广，因此项目中应谨慎使用。Context在React周边库中使用非常广泛，如React-Redux、MobX-React、React-Router、Umi、DVA、AntD组件中。

**远亲组件**：即组件层级关系不确定，此时比较适合使用第三方状态管理库，如Redux、MobX、Recoil、Zustand、jotai、valtio、xState等，AntD4/5 Form中也是此种用法。

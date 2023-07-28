# react-router6原理

router6相比router5发生了很大变化，大部分API都变了。

如，相比router5，router6实现了配置式路由，渲染子组件需要手动渲染`Outlet`组件，404路由需要配置path=\*，且放在任意位置都可以。

使用Context机制，从Router层传递naviagtor、location、match等参数给后代组件，同时BrowserRouter、HashRouter组件会监听location，一旦location变化，即路由变化，那么就会执行setState，导致组件更新，后代消费naviagtor、location、match等参数的组件也更新。

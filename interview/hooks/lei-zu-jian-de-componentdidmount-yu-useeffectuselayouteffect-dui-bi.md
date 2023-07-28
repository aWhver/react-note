# 类组件的componentDidMount与useEffect/useLayoutEffect对比

前置知识：[useEffect和useLayoutEffect差异](https://juntong.gitbook.io/react/interview/hooks/useeffect-he-uselayouteffect-cha-yi)



赋值给 `useEffect` 的函数会在组件渲染到屏幕之后**延迟**执行。

`useLayoutEffect`函数签名与 `useEffect` 相同，但它会在所有的 DOM 变更之后**同步**调用 effect。

`componentDidMount`执行时机同`useLayoutEffect`。

源码截图：

![](<../../.gitbook/assets/image (3).png>)

引发的思考：涉及的更新的订阅应当写在`componentDidMount`或者`useLayoutEffect`中，比如用函数组件实现antd4/5 form的[Field](https://github1s.com/react-component/field-form/blob/master/src/Field.tsx)，注意rc-field-form的Field使用类组件实现的，因此用的是`componentDidMount`生命周期，如果要换成函数组件实现，则要使用`useLayoutEffect`。这个可以参考[我实现的Field函数版](https://github1s.com/bubucuo/form-nut/blob/master/src/components/my-rc-field-form/Field.js)。

比如react-router6 [BrowserRouter/HashRouter/HistoryRouter](https://github1s.com/remix-run/react-router/blob/HEAD/packages/react-router-dom/index.tsx)。

![](<../../.gitbook/assets/image (4).png>)

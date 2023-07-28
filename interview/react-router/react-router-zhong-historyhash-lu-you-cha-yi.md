# React Router中history、hash路由差异

HashRouter最简单，因为服务端并不解析#之后的字符，但是前端可以监听#之后字符的变化，因此前端可以依据这个变化决定渲染哪个组件。也因此，使用HashRouter不需要服务器端的特殊支持。

但是BrowserRouter就不同了，BrowserRouter使用HTML5 history API（ pushState，replaceState和popstate事件），让页面的UI同步与URL。服务器端对不同的URL返回不同的HTML，后端配置可[参考](https://pro.ant.design/zh-CN/docs/deploy)

![](<../../.gitbook/assets/image (1).png>)

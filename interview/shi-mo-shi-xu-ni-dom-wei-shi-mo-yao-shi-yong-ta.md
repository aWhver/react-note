# 什么是虚拟Dom,为什么要使用它

![](<../.gitbook/assets/image (10).png>)

用 JavaScript 对象表示 DOM 信息和结构，当状态变更的时候，重新渲染这个 JavaScript 的对象结构。这个 JavaScript 对象称为虚拟DOM。

DOM操作很慢，轻微的操作都可能导致页面重新排版，非常耗性能。相对于DOM对象，js对象处理起来更快，而且更简单。通过diff算法对比新旧vdom之间的差异，可以批量的、最小化的执行dom操作，从而提升用户体验。

![](<../.gitbook/assets/image (11).png>)

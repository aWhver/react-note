---
cover: >-
  https://images.unsplash.com/photo-1583264725407-fd4b17d8e09c?crop=entropy&cs=srgb&fm=jpg&ixid=M3wxOTcwMjR8MHwxfHJhbmRvbXx8fHx8fHx8fDE2OTA3NzE2Njl8&ixlib=rb-4.0.3&q=85
coverY: 0
---

# React18和Vue3的VDom diff比较

![](<../../.gitbook/assets/image (9).png>)

1.子节点数据结构上 React的old是单链表，Vue的old是数组 React只单向查找，Vue双向查找 2.哈希表 为了快速通过key值找到节点，双方都用到了Map React中根据old做出Map，Vue中则是根据new做成Map 延伸：为什么是Map，而不是Object React18/Vue3VDOMDIFF比较 3.如果old和new其中一方已经遍历完毕，两者处理相同。这也是必然的 4.Vue用到了LIs. 注意掌握其算法

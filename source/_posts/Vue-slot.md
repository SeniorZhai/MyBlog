title: Vue slot
date: 2019-05-29 15:10:38
categories: Vue
tags: [Vue, slot]

---

Slot 是一种父组件与子组件通信的一种方式

<!--more-->

```jsx
// 父组件
<div>
  <SayHello>
    world! // 可以是组件 和可以是简单的文本
  </SayHello>
</div>
// 子组件 SayHello.vue
<span>
  Hello,
  <slot></slot> // 由父类决定是啥
</span>
```

![](./img/19053002.png)

slot name

```jsx
// 父组件
<SayHello name="world">
  <a slot="link" href="https://www.google.com">link</a>
  <p slot="page">Text page</p>
</SayHello>
// 子组件 SayHello.vue
<div>
  Hello, {{ name }}!<slot name="page" />
  <slot name="link" />
</div>
```

![](./img/19053003.png)

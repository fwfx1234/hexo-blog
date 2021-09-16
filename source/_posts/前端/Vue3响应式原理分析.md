---
title: Vue3响应式原理分析
date: 2021-07-02 10:50:53
comments: true
categories:
  - [前端]
tags:
  - vue3
hidden: true
---

### 先立个flag吧， 顺便当个草稿纸

```ts
type Dep = Set<ReactiveEffect>
type KeyToDepMap = Map<any, Dep>
const targetMap = new WeakMap<any, KeyToDepMap>()
// 看着不方便,展开下
// targetMap: {
//     any: {
//         any: set[ReactiveEffect]
//     }
// }

```

component渲染的时候创建了effect

此时 effect 是全局的， 然后执行updateComponent的时候，会创建reactive对象，在getter里会track对象

track是将effect添加到target对应的key对应deps里， 然后setter的时候，trigger, 会触发target对应的key的effects，
effect执行前会先执行其options上的ioTrigger,和scheduler，最后执行其本身

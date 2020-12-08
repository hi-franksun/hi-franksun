# Vue overview

Vue：渐进式 JavaScript 框架，解决数据驱动问题，避免手动更新 DOM；

Vue 特点：
更加轻量级，20 Kb；
渐进式框架，不需要全部学习完成就可以做出项目；
响应式更新机制，相应式不需要手动更新；
学习成本低，模板语法基于 HTML；

学习建议：
基础：Vue 基础核心知识；
生态：Vue 大型项目需要的周边知识；
实战：通过实战，学习应用；
进阶：Vue 使用技巧和进阶；

## 环境搭建

- 浏览器：chrome
- IDE：VSCode
- Node.js 8.9+，npm

[安装可以使用 cdn 版本：](https://cn.vuejs.org/v2/guide/installation.html)

## Vue CLI

[文档地址](https://cli.vuejs.org/zh/guide/)

Getting Started
```shell
# Install -g 是 global，全局安装的意思:
npm install -g @vue/cli
# OR
yarn global add @vue/cli

# Create a project:
vue create my-project
# OR
vue ui
```

- npm install -g @vue/cli
- vue create my-app
- cd my-app
- npm run serve

20200926 install version is @vue/cli@4.5.6

## Vue.component 缺点

- 全局定义：强制要求每个 component 中命名不得重复；
- 字符串模板：缺乏语法高亮，在多行时候，需要用丑陋的 \
- 不支持 CSS：意味这当 HTML 和 JavaScrip 组件化时候，CSS 明显被遗漏；
- 没有构建步骤：限制只能使用 HTML 和 ES5 JavaScrip，不能使用预处理器，如 Pug 和 Babel

## Vue 单文件组件

## Vue 组件核心

Vue 组件 = Vue 实例 = new Vue(options)

Vue 组件三大核心：属性、事件、插槽

### 属性

- 自定义属性 props：组件 props 中声明的属性；
- 原生属性 attrs：没有属性的声明，默认自动挂载到组件根元素上，设置 inheritAttrs 为 false 可以关闭自动挂载；
- 特殊属性 class/style：挂载到组件根元素上，支持字符串、对象、数组等多种语法；

### 事件

- 普通事件，通过 this.$emit('XXX', ...) 触发
  - @click
  - @input
  - @change
  - @XXX等

- 修饰符事件，一般用于原生 HTML 组件，自定义组件需要自行开发支持
  - @input.trim
  - @click.stop
  - @submit.prevent

### 插槽

现在不区分普通插槽和作用域插槽了，推荐统一写法

- 普通插槽
  - `<template v-slot:xxx>...</template>`

- 作用域插槽
  - `<template v-slot:xxx="props">...</template>`

### 大属性

大属性：不管是事件还是插槽，本质上都是属于属性，他们都是通过父组件传递给子组件内容，然后有子组件分别根据传递的内容来执行行为。

### 双向绑定和单项数据流

双向绑定：model（数据） 和 view（视图/页面） 会互相影响，一个更新另一个也会随着更新

单项数据流：model 更新会更新 view，但是 view 更新不会影响 model 更新；

#### Vue 是单项还是双向

- Vue 是单项数据流，不是双向绑定；
- Vue 的双向绑定不过是语法糖；
- Object.defineProperty 是来做响应式更新的，和双向绑定没有关系；


### 虚拟 DOM 和 key 属性

### 生命周期


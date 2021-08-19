---
title: React表单解决方案
date: 2021-08-19 21:11:22
categories: React
tags: 前端
excerpt: 本文介绍了React Hook Form的基本用法，简要说明了React中受控组件和非受控组件的使用场景，并给出了代码示例。
index_img: /img/chore/react_hook_form_index.png
---

## 前言

表单是很多 Web App 的核心，本文将在 React 框架下，用三种不同的方式实现同一个表单，以此来介绍 React Hook Form 的基本用法和使用场景。

## 受控和非受控组件

[受控组件][controlled]与[非受控组件][uncontrolled]都是针对`<input>`、`<select>`、`<textarea>`这类表单元素而言的，这类元素自己维护 state，并根据用户输入更新 state，受控组件与非受控组件的区别如下：

{% note secondary %}
[受控组件][controlled]：数据源来自 React 的 state，若使用 Function Component 实现受控组件，则需要使用 useState 钩子来维护受控组件的值。

[非受控组件][uncontrolled]：数据源来自 DOM 节点，使用非受控组件的 ref 来获取它的值。
{% endnote %}

受控组件的优点在于符合 React 的哲学，能实现更强的功能，缺点是代码量较大。

而非受控组件的优点在于编码简单，适用于逻辑不太复杂的表单。

## 简单登录表单实现

下面将用三种方式（本质上是受控和非受控两种）完成同一个登录表单，加深读者的理解。

该表单只有用户名和密码两个字段，用户名的限制为非空、最长10个字符，密码的限制为非空、最少8个、最多20个字符。

### 非受控组件方式

[代码实现](https://codesandbox.io/s/uncontrolled-component-6nuds?file=/src/ThirdParty.js)

### 受控组件方式

[代码实现]()

### 第三方库React Hook Form

[代码实现]()

## 相关链接

- React 首页：https://reactjs.org/
- React Hook Form 首页：https://react-hook-form.com/

由于作者水平有限，所以文章中难免有少数不严谨之处，如有读者发现此类疏忽，恳请读者指出。另外，如果认为本文对您有帮助，欢迎请作者喝咖啡！![Matrix53的微信赞赏码](/img/global/wxQRcode_pay.png)

[controlled]: https://reactjs.org/docs/forms.html#controlled-components
[uncontrolled]: https://reactjs.org/docs/uncontrolled-components.html

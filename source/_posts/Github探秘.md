---
title: Github探秘
date: 2021-07-25 09:48:22
categories: 随笔
excerpt: 本文针对Github上的一些好用的功能进行介绍，包括但不限于Git page、Github Action等功能，帮助读者了解Github的强大之处。
index_img: /img/chore/github_logo_index.png
---

## 前言

Github 作为全世界最大的代码托管网站，无数顶尖极客云集于此，更有无数极为优秀的开源代码以供学习及应用。而 Github 网页端功能及其提供的 API 非常强大，配套 Git 直接就能实现完整的项目管理，下面本文将向读者介绍几个 Github 的进阶功能，抛砖引玉。

## Github Actions

简单地说，Github Actions 是一个脚本，能够在一个指定的时机，对于某个指定的 Github 仓库做一些事情。

- **指定的时机**可以是在某个人对该 Github 仓库 push 之后，或者是该仓库新增了一条评论之后，诸如此类时机都可以被指定为执行脚本的时机，详情见[官方文档](https://docs.github.com/en/actions/reference/events-that-trigger-workflows)。
- **脚本**可以非常的复杂，这里的脚本，是运行在 Github 提供的一台临时服务器上的，你可以指定该服务器的系统以及系统型号，你可以在该系统上安装 node.js，毕竟是服务器嘛。

一个容易想到的 Github Actions 用途就是自动部署，在成员向指定仓库 push 之后，可以利用临时服务器进行源代码的编译，并将要部署的文件传到自己的服务器上。从某种意义上来说，Github Actions 有点像钩子函数，只不过这个钩子函数的功能非常强大。

关于 Github Actions 入门及脚本编写，可以看这个[官方文档](https://docs.github.com/en/actions)。

![Github Actions](/img/chore/github_action.png)

## Github Apps

Github Apps 和 Github Actions 比较类似，你可以在 Github 顶栏的**Marketplace**里同时找到这两项 Github 提供的功能。如果要说 Apps 和 Actions 的不同的话，我认为体现在**集成度**上面，Apps 的功能更为直接和完善，开箱即用。Apps 就好像给编辑器安装的插件，而 Actions 就好像给编辑器写的脚本。基于 Github Apps，你可以实现但不限于以下几件事（事实上，这只是冰山一角）：

- CI/CD:自动化项目构建、项目测试、项目部署等工作流
- 图片自动无损压缩，加快网络传输
- 自动关闭不活跃的 issue

{% note secondary %}
关于 Actions 和 Apps，读者可以在 Github 的[Marketplace](https://github.com/marketplace)上尽情探索。
{% endnote %}

![Github Apps](/img/chore/github_app.png)

## Github Pages

Github Pages是一个用于网页展示的功能，于**2008年**推出，世界上的任何一个人，都能通过Github展示自己的网页。可以利用Github Pages进行个人博客托管、项目成果展示等，[我的个人博客](https://blog.matrix53.top)就是利用Github Pages进行部署的，免去了购买服务器的费用。

具体来说，想要使用Github Pages，必须建立一个名为username.github.io的仓库（其中的username为用户名），然后在该仓库的设置中打开Pages服务，这里就不赘述了。需要注意的是，不是只有该仓库才能启用Github Pages服务，其他仓库也能启用Pages服务。

关于Github Pages的详细介绍和教程，请见[官方文档](https://docs.github.com/en/pages)

## 其他功能

Github还有很多强大的功能，比如其提供的REST API、GraphQL API、projects和gists等，这里也不再叙述了。最后讲一个小故事吧，Github在2018年3月1日遭遇了有史以来最严重的DDoS攻击，峰值流量达1.35Tbps，而Github在10分钟之内化解了这次攻击。

## 相关链接

- Github Actions 官方文档：https://docs.github.com/en/actions
- Github Apps 官方文档：https://docs.github.com/en/developers/apps/getting-started-with-apps/about-apps
- Github Pages 官方文档：https://docs.github.com/en/pages
- Github 功能总文档：https://docs.github.com/en

由于作者水平有限，所以文章中难免有少数不严谨之处，如有读者发现此类疏忽，恳请读者指出。另外，如果认为本文对您有帮助，欢迎请作者喝咖啡！![Matrix53的微信赞赏码](/img/global/wxQRcode_pay.png)

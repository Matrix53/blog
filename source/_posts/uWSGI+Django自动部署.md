---
title: uWSGI+Django自动部署
date: 2021-08-10 20:07:17
categories: 随笔
excerpt: 本文记录了一种使用Nginx反向代理uWSGI，进行Django项目部署的方案，使用了Github Action实现了项目的自动部署。
index_img: /img/chore/django_logo_index.jpg
---

## 前言

> 博客近半月没有更新，一位好友前来催更，于是有了这篇文章（bushi

近期在学 React，本来想写几篇关于 React 的入门博客，但是 React 这个框架越学越觉得博大。遂暂时放弃了写 React 入门博客的想法，以免贻笑大方。本文记录了 Django 框架的一种简易部署方法，使用 Nginx 反向代理 uWSGI 在一台服务器上部署 Django 项目，并使用 Github Action 进行自动部署。

## 技术栈选型

- [Nginx](https://www.nginx.com/)：一个 Web 服务器，还能够实现反向代理，均衡负载等功能
- [uWSGI](https://uwsgi-docs.readthedocs.io/en/latest/)：一个 Web 服务器，常用于结合 Django 等框架处理动态请求
- [Django](https://www.djangoproject.com/)：一个成熟的 Python 后端框架，自带多个模块
- [Github Action](https://docs.github.com/en/actions)：Github 自动运行的脚本，详情见我的另一篇博客{% post_link Github探秘 %}

使用这个技术栈的原因：若要在一台 IP 唯一的服务器上同时部署前端和后端，且均使用 HTTPS 协议进行传输的话，前端和后端必定监听不同的端口。但是监听不同的端口就会产生跨域问题，而在一台服务器上解决跨域问题，一种较为简洁的方案是前端将请求反向代理给后端。而使用 Nginx 将请求反向代理给 uWSGI，uWSGI 处理动态请求的方案配置较少，较为简洁，于是采用这个方案。

在请求的处理上：将带有 api 前缀的请求视为动态请求（访问 uWSGI），其他的请求视为静态请求（仅访问 Nginx）。即，将**https://ip:443/request**视为静态请求，将**https://ip:443/api/request**视为动态请求。

## 部署流程

本文不关注 Nginx、uWSGI、Django、Git 等环境的安装，具体的安装步骤请到[相关链接](#相关链接)中查看。

### Nginx 反向代理

> 关于 Nginx 的配置基础，可以参考[这个链接](https://www.runoob.com/w3cnote/nginx-setup-intro.html)

使用 Nginx 做反向代理，一般用到的配置参数是 proxy_pass。但是对于 uWSGI，nginx 专门提供了一个更优的配置参数进行配置，即 uwsgi_pass，配置文件核心内容如下：

```plain
# Nginx配置文件部分内容
# 处理动态请求
location /api {
  include /etc/nginx/uwsgi_params; # Nginx提供的uWSGI配置
  uwsgi_pass 127.0.0.1:8000; # uWSGI监听的地址
}

# 处理静态请求
location / {
  try_files $uri $uri/ /index.html;
}
```

编辑完配置文件后，需使用`nginx -s reload`命令重启 nginx，配置文件才能生效。

### uWSGI 处理动态请求

uWSGI 可以手动启动，但是需要输入启动参数，这里让 uWSGI 从配置文件启动。假设 Django 项目叫做 backend，uWSGI 的配置文件叫做 uwsgi.ini，那么一个便于管理的项目结构大致如下：

```plain
# Django项目结构
- backend
  - backend
    - __init__.py
    - settings.py
    - urls.py
    - asgi.py
    - wsgi.py
  - manage.py
  - uwsgi.ini
```

配置文件 uwsgi.ini 的参考内容如下：

```plain
# uWSGI配置文件
[uwsgi]
socket=127.0.0.1:8000 # 使用nginx连接时使用
chdir=/path/to/backend # 项目目录
wsgi-file=backend/wsgi.py # 项目中wsgi.py文件的目录，相对于项目目录
processes=2 # 指定启动的工作进程数
threads=2 # 指定工作进程中的线程数
master=True # 指定在这些进程里有一个主进程
pidfile=uwsgi.pid # 保存启动之后主进程的pid
daemonize=uwsgi.log # 设置uwsgi后台运行，uwsgi.log保存日志信息
log-maxsize = 100000 # 设置日志文件最大字节数
max-requests = 1000 # 设置每个进程最大请求数
```

在 uwsgi.ini 文件中输入配置后，就可以 cd 到项目根目录下，使用`uwsgi --ini uwsgi.ini`的方式启动 uWSGI 了。

### 处理请求前缀

若通过上述配置文件部署 Django，那么请求的前缀不会被自动去掉，即 Nginx 虽然将**https://ip:443/api/request**这个请求传递给了 Django，但是 Django 会将这个请求的 url 解析为 api/request 而不是 request。

Nginx 官方推荐的解决方案是 Nginx 给 uWSGI 传递参数，uWSGI 根据参数去除请求前缀，向配置文件中添加如下配置即可：

```plain
# Nginx配置文件部分内容
location /api {
  ... # 表示其他配置，实际文件中不是省略号
  uwsgi_param SCRIPT_NAME /api; # 表示需要去掉api前缀
}
```

```plain
# uWSGI配置文件
... # 表示其他配置，实际文件中不是省略号
route-run = fixpathinfo: # 根据参数去除请求前缀
```

### Github Action 自动部署

> 若要实现向 Github 项目 push 之后就进行自动部署，可以考虑 Github Action、Github Webhook 等方案，这里使用 Github Action。

事实上，Django 项目的部署与更新非常简单，总体上来说需要做两件事：

1. 更新服务器上的 Python 代码，考虑用`git pull`命令完成，这里需要注意服务器需要有操作项目仓库的权限
2. 重启 uWSGI，使用`uwsgi --reload uwsgi.pid`即可

注意，Nginx 是不需要重启的，因为 Nginx 不需要做任何代码或者配置更新。

于是，可以这样编写[Github Action](https://docs.github.com/en/actions)的脚本：

```yml
name: deploy

on:
  push:
    branches: [main]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: setup
        run: sudo apt install sshpass

      - name: pull and reload
        env:
          DST: ${{secrets.DST_FOLDER}}
        run: sshpass -p ${{secrets.PASSWORD}} ssh -o StrictHostKeyChecking=no ${{secrets.USER}}@${{secrets.IP}} "cd ${DST}; git pull; uwsgi --reload uwsgi.pid"
```

## 结语

至此，Nginx+uWSGI 部署 Django 项目的大致流程就介绍完了。读者在自己的服务器上实践可能会遇到一些坑，建议在[Google](https://www.google.com.hk/)或者[StackOverflow](https://stackoverflow.com/)上查询相关答案，或者在下方留言区写下您遇到的问题。

## 相关链接

- Github Actions 官方文档：https://docs.github.com/en/actions
- uWSGI 官方文档：https://uwsgi-docs.readthedocs.io/en/latest/
- Nginx 首页：https://www.nginx.com/
- Django 首页：https://www.djangoproject.com/

由于作者水平有限，所以文章中难免有少数不严谨之处，如有读者发现此类疏忽，恳请读者指出。另外，如果认为本文对您有帮助，欢迎请作者喝咖啡！![Matrix53的微信赞赏码](/img/global/wxQRcode_pay.png)

title: Flask-Bootstrap的默认样式加载过慢
date: 2015-10-22 18:21:03
tags: 
- Python
- Flask
categories: 
- Python
- Flask
permalink: flask-bootstrap-css-load
---
刚开始学习flask，调试程序的时候发现在加载flask-bootstrap的样式和jquery文件的时候加载速度过慢，原因是在默认情况下加载的是美国的CDN节点下的样式文件，解决方法：
　　
我是在``pyvenv``虚拟环境下

　　修改默认配置：
　　　　1. 进入 ``venv/lib/python2.7/site-packages/flask_bootstrap`` 目录，修改``__init__.py``文件
　　　　2. 找到以下地方并修改
　　　　```python
　　　　    bootstrap = lwrap(
　　　　        WebCDN('//cdn.bootcss.com/bootstrap/%s/' 
　　　　            % BOOTSTRAP_VERSION),
　　　　        local)
　　　　        
　　　　    jquery = lwrap(
　　　　        WebCDN('//cdn.bootcss.com/jquery/%s/' 
　　　　            % BOOTSTRAP_VERSION),
　　　　        local)
　　　　```
　　　　
> 使用bootstrap中文网提供的免费CDN加速服务（同时支持http和https协议）之后，加载节点变成了国内，比用默认的美国节点快的不是一点半点（为什么慢我相信你知道）

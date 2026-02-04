---
date: 2022-01-14
title: Openstack glance 单元测试中遇到的插件无法加载问题
author: Jn
categories: 工作
tags: 
- Openstack
---

## 问题
通过vscode 直接跑function test时候报错找不到插件。（手动跑结果也一样：`stestr run --no-discover glance/tests/functional/v2/test_images.py::TestQuotasWithRegistry::test_image_upload_under_quota --debug`）
报错日志：

2022-09-26 10:33:16.081 16636 ERROR glance.common.wsgi   File "/home/jn/code/cee/openstack/glance/glance/common/location_strategy/__init__.py", line 120, in get_ordered_locations
2022-09-26 10:33:16.081 16636 ERROR glance.common.wsgi     strategy_module = _available_strategies[CONF.location_strategy]
2022-09-26 10:33:16.081 16636 ERROR glance.common.wsgi KeyError: 'location_order'

## 背景知识：
* setuptools 通过读取 setup.cfg 生成相关的打包文件
* stevedore 根据打包之后生成的 entry_points.txt 动态读取插件

由于没有用仓库定义的 tox.ini 先生成上述文件，导致读取不到插件。

## 解决办法：
先执行原来的测试框架生成必要的配置文件。
```
tox -e functional-py37 glance/tests/functional/v2/test_images.py
```
代码根目录会多出 /etc/ /glance.egg-info/ 等文件夹，之后再手动跑/用vscode test 跑都没在出现问题。

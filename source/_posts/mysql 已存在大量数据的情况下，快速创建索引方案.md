---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466T3TAQGQA%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T100049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBhHWgHhZo4u7HF%2FE7H5uZYbIWGabXvsjWmQj%2FIxcyWEAiEAyBGu%2FoWkvZpJFJjJzY8%2BZxeaiFhVVcORtZsfQVwv9e8q%2FwMIbhAAGgw2Mzc0MjMxODM4MDUiDGKGkbvxoUaODBP7lircA8O4utnBRFpZqoq4vkXGMtEXzefblyVJ%2F17ZXG46eSOeVECbQGUILTFUs%2BGDVzakFSYuYpw3gDq%2B0LFOJOG9whSGPLFWgvVACV3Voe70trHDweFinpArav65BO1wnRtwxmrztJNKP7HXO1kfnN569%2FkDj2%2FE6wjaycr8IzHNCmXSHdTQF3FbgO%2F1zxtq1EZcLA7tO1gUKImYD6cGN7tD4TWMd7n%2BSZ877F6eBzZDIws7XFhLxcHl7Jz6q7XS8d0BaNJ72gJAivjw8caKLXSs%2B6UxaqyqVOA4XYE6uWZxAXSafgg2TVLT6CieSf6JHnFpHvM9tXfQvZMr0p4CEvLbdfJ55PqJH3HWR9JrDPncQczAF3dp68Xd65z2A%2BnL4S62YV43dVfPUdk5wKL9YbyNfAMCJjV8lgUG4HChd1YJ0wrx74s%2B6Ej6tFwCmczbBe0LG3%2FXFnRMe2p2zSomlvixxuolJI0vtsS3GIqwUuW%2BF0zyxhfx5uYI9s1opF8akiU9LjOaxTmX9q7v9VzA0NIuAamiOzYLYT9ChpofM%2BRN3da7VzXUoMcMPZgRb7e7WSgB2ZqPoA5j0L%2BqBpPEsP1j7DIThDAh6nfwnpgz%2Bj9q4tCn4O5yszcuxH7tFsQ%2FMNX7h8cGOqUBUppSieK2GnrToN9hckIzvgvnx61AhB%2F7GSz3VhM7aeYG%2BGubDKLXkJIEHxn64ty0VW29l7Q2zDY5DWszFgvjuEKMLgR0vyDBZE3k57blt9inrtyrqj23bowbNWBKrgU6KNsaOnN47iCy7aGaxaHMrFU3iX9Ccrq6jhP%2Fvtsh1DLRq7oNtsKIexgYfOEsAWmqnf82Vdn9Cva97WAEK3JN0%2BdeDj8c&X-Amz-Signature=d6ae2413999b2eda2cc6ea8b555a914bc43366702bf9f8d27e4a73f6375f036a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-16 13:56:00'
index_img: /images/c34f92fd7edfbc072452166489949590.png
banner_img: /images/c34f92fd7edfbc072452166489949590.png
---

# 原因


最开始设计表的时候，没有设计好索引，等数据量多了，查询效率慢了，想再次简历索引。但 数据量巨大，一创建索引，数据库直接崩溃


# 解决一（慢 但是不影响系统使用

1. 设置mysql文件导出权限
2. 导出文件为txt文件
3. 创建一张临时表，与原来的表结构一样 `create table text_assets like network_assets_blend`
4. 导入数据到临时表
5. 导入完成之后，将原来的表改为其他表名，作为备份，将原来的临时表改为真正的表名。

# 解决二（速度快，但是影响系统使用

1. 直接备份数据，导出sql文件，（这一步几分钟
2. 截断表（就是清空数据保留结构
3. 建立索引
4. **将sql文件中的删除表结构和新建表结构语句进行删除（重要）**
5. 导入sql备份文件

# 解决三（保守一点


就是方案2的改版，额外创建出一个临时表来存储数据。


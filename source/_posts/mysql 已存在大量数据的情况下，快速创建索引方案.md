---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666PIL4PDA%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T080048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEH8aCXVzLXdlc3QtMiJHMEUCIGqsv7UxopkPJdHPUfG3PWJL6SW4m5meTAL3CEPm1CwgAiEAprzKv0jnhVyeMlRvX7N%2FslEzoigI%2Bivlr9wn8WvJ7Icq%2FwMIRxAAGgw2Mzc0MjMxODM4MDUiDCmBp9sZa7GpA%2FpIZSrcAzdipK2kW06doZRY%2FHTJhk%2BTPSIZKJoAQrt0j6ww3utHGlBa5jGMoO%2BN%2F8OQuYh63kYGk8Bvy7JaL0oEkza%2FJAfBtbxh986yF0MgotNGOqvQmSBoxunvIRq8pFQCm7xrMzjhC5wVJN311p5KZWm8jQ0ULZ%2BlmomzT0iFnuD8I6z6VgNhUjaxL5EjajJW%2F9F2NarSQyMzVCcJNykbfh%2BHj46wAGspJYfUFrEslml%2F9XdoFBfGQWt5jzUfC0uYIIieVm4UnNgxzWXtgTiXD21fMIiTJuwhEhzBBlga50sKnGh0qwwXCIQiPcSYCtMmEoEHCGPjVWdPU%2FykZT0oVLz1MonjfDRaZhaa0XRX%2FOGuHIFOZzaYPuW23QXCJU6zy8eBO2FVTyHGEYvvx5%2Ft%2Bw6TCm3DcM%2FJW7wlBQ4u3H%2FLxCZz%2FSOYl0rxBkuCLEvz%2FpRY9FCFKj1M15a89zwXOzZaVnsA9cWjoHKtfWhDRIHzH%2FxJhujB4hGv8XDqnY%2BRkYaX5xm5ZxLx8%2BZzqCDOcF03OB6Inr%2F8hKD5ChjzS0by3oPugsrsfO54RkF0rqiM%2BLQqomwDbjAdVujznH6r%2BuoFKxHsGQJvn0LevXMg5OrJ2fujpw3xgZvZBg83H3eDMNjx1cgGOqUB7jmse8s3G1tIS%2BNLF0UHgcfv2EDr2b7YsDeZdZQiUh%2FJe6QIYyhO6WyaBbYz2AAi0BA2lchRuwbQp70d5354WOLPMsadCN%2FDnV%2FV4GS%2F1Lm%2Bfo6CNGTDH3qgW3oJwRuxASln5kmg3i0z2JXgHawkN86lj0ESJeP6auKJ%2Fc8RelCb%2Bu0emAklf3XMV88CO7cxIlL3Fc8cG4zpn91ed79fEu%2BhQEiV&X-Amz-Signature=945d5eab760bf7446abcdd76efa2d73cfccc887b782297ba4321dc4c06121da9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


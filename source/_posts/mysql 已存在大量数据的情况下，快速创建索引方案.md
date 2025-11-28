---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466R2AWMEAW%2F20251128%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251128T050042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAmDtTT07cXlqwoarn2p%2B5A3fXa%2FMQQl6QBtfvshzJlzAiEAgDICvF2HG1FzQbrqIQ5kEUvkFKWZEXq8BbNKTMhpkCUqiAQIrf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBmVNt8OR7Bp4SBYNircA5sgsxX%2BOkzwFNt5mhh9RC1hVbCEIdd1v5VR999K1YXw2%2BZluo3lbDpRHpz6Aj7ctL85%2F3UwsD8NvaxnAld1EEuLzyzpisp7J2BcvZYCMmjozljpsdDNQZ6VV6a1%2Fl8qdyFFoi2jy%2BU3%2F5scMN%2Fo1v9fvYOhyD1PU4UlYapi0N8L8t%2BUYR9W61Idp0b9YK9zVDGpkVdr%2BkghvKLMAi8h4mlPttdatSqf1iTc0ALnFwnvCpdFk6PXNTbGzPpyXuOZiLN5r%2Byi%2BEzGTfF7kKtfR23cyYQ8cfa2xbMgwd3yJaAZ%2FmMKHqj5k8m12hGj4DkE0bB6UJZOk9H0O3fLPrfcICectAV%2FGxOg1GkIb5n16jXK8GPe04FJeFERiN7rfpOWfcK6YJ1OeQ5CzO9m3do8xQVo5qua83OChVWjqoDYybJgAI%2B1l2MrB7ICGeQYkp52cJ9MkLZHHwkW40GaPaBpQ0DmMvU37ez%2F4PSoNqCkdQM%2BVAD7nyDBg9QiImrLzExAT6tYtOMHGHpQ1DPIloUFKBzTNPGPmwWbTGEvblQ8ud8YdUGmgtYtgfCw%2FoCwRXn7hBuEjih9X77M%2Fieaf6yGMkhZl2TCx4naBI9SwMZ%2BmwDVkWaT2JZgnrXjE2VvMMO7pMkGOqUBop7mRWU9q6z7H81T15LIyNWfv%2BLj%2FtNBEsFadPpQhCqR1qQUXokGMIrPgIilC%2F5xswsgdsHSsKldZ3R0rCNwnrr7T8t3BzCciKpa5UGqca32b4SEfqV7DYcRVfkHMfWoJVRyhi7PeI70jrx0UoAadpn5%2BpUZmKnpp6J6E0Ky6D5%2F0TWRinxXnKqoGJt9DEVmkEDWDS6rvRZU0wIojwG4UIaV1rK1&X-Amz-Signature=bb9d44f4cf693a7e4d105f563006ca9568e75b82dc940a0081fec3cc393c9c80&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


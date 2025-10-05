---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WFFVRPQ5%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T180038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCUImIznjk18P0OLHKvoNLIGL8xZgfGzT%2BOS8iDAOwxVAIhAO3ptfwsGoU6PA4FQ1WpEou6eZL%2FR25AeuIpAhonYoV1Kv8DCHoQABoMNjM3NDIzMTgzODA1IgyxF4QB8XMevybRmHgq3AMOFK4N6BCcmJgovlJmWHNsXEEsM7SKCUDxmgxzP0vRozzYAvTHpN9ZntdOXjgV28De2JqSCQwfRajWlQfPfyN49gJDvdt0mYdZ9BiECqeijiBl9zEN3%2Bd5Zpn4BvB9az2%2Fwsv03XwMsvrb4nizl0xsLBGjRSt%2BbyoChCfWmsSzVbOyEZy5im67ueHYdZfuLifkJM1o1h0M0PUT7kN%2BhrU3w1Kwn8Cx7T%2FmeyXTLuyCu%2FdTk5csCOHpoYoRRTPN8C5EZ8uK5PSQ1C6CqfRvOs%2FCFntdYqiIjfuxJpSOLQlbNXlbZ9hNtzWGyiAqhYkaE00VHhA%2FbJYJpmRLr4BZRCEhDIrEa0Kuuwy%2F2tIdt%2BX5p4xY8H4ZKHvWyUSe7H3dlDmTXrxrPx5%2FEPad98CHG2v2eErHWuoU0JuW%2F1h%2BRzC7QD217Mq0%2BV0reiWS%2FihyP%2BjEan4e7d8r6RFjjniBAKn8k5Q7Qm91H3sa6yLDsMWjFmFhQPZX%2Br6k5CpIhvwmUSIVFtF1FROEOnZ%2BUaAV0bdDa15MWx51vEymn8blkYQjkZcBWNUPi4fCnjfwY%2BikJtLkyyVsZLJLyzQOXlu%2F4RO3blotFp%2BMxcBpE%2BA%2FZe20Zr4ZbBBol2WXWg39xTC%2BwIrHBjqkAT3tiFqvnRYlHIKuX4zNbaDEC%2FL6HVXPMOdgtBvRXJIf%2FsQGRDhSuJSHqbo%2BeHsngLM28wLhbTqzC9RW%2B%2F%2BPq5PcsdJN0RwtMVxOa3aYXAaYkHhRgT55Yylfc1A6VW49SsvejBbSntTkwrlRVidjBoDOblsnBJYIcuGnq%2FHtQrSYszYXhw9DfjstOLH3BLuvx%2BAsbUFyZvRMwKVn8RQ0IsF15pir&X-Amz-Signature=d4b3d93158bf87d91f85224c43b1eb172d91482e325ebf3dbf3e3af75994b6e8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


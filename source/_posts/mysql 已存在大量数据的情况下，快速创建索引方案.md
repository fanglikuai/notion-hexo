---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QLCP4XX6%2F20250915%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250915T082105Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCICYYZAgpLbF0KnydhJFzoqm7C%2FpzDZZgx3ImYbRk2xCuAiEA3rZcoPCAzbu%2BzTnDM28v%2FuyV6OmMswZ%2BmYfUxVXAIoEq%2FwMIcRAAGgw2Mzc0MjMxODM4MDUiDCxJOJNB15rAcHKaMCrcAxJ5%2FS7cg%2Fu2U5iuHlTwW7U4zrRLFKhlsclI0Qu7bW1E83HM%2FvycNsNSxcXbixvf69NVHe77bucmnyjAIJUX2pv%2BMtr6I%2BR%2BlcG1T5Vt1xsLwZrxuMnuwG%2B8DLDPOQJlZMbV%2FrxWgyPlTMG7JO4eGO16JYusOjLQpjR0mOFJ5BXNIs0CZDaWIViQa7KHHq3tBCtWjr2Z%2B7lhR0IDRK0adK1aJrquz%2BnYT136eOtJbM0T9%2Fq8u4nhPldOjh03F2J%2Fd9NnjAw7A%2BrsAfl2Ne1BhK4BFqQ1hHgyHGMBcz%2BWm%2Fh9NUH9NR%2B648p3uAbsoJFTMU81uAWpl9s98lPf3Rvd3FQup6TevbpZsanBDzOk99WZKOysHvUW%2Frj3dMY%2BByunDi9AW2Pdlrl59ZWyD2%2F8O%2BV1xR8nQSVRr%2B1XSfZB1W%2FBv6tShvsLdZv5dF7ynzcLuTP%2FnkJ7eXK85xFcU7o%2Bf%2BFxAUO2uDtEuuKdK5lDhFoLWdlPOhof7tMKjHAdofWlmLfpYJbmGtqOfny26wLHbVVy4MFgncL36VmYlOf2JChG8DmS9j64%2FS%2BOhpAD%2FlOitQz55ZTx238hMWMjIlogK4MNx%2BNyghYAFHYj6ZHF7jlDZwDmB80yJjy7MQZwMJGTn8YGOqUBIopqxtIn0XQtcWIUImmRb3zqd0nQXqwdTgKTSv0v1IccHK8KN5ZKixgTqGD2N8%2FLK2%2BupJEdNS%2BriScko1YFQVTgHqs%2F0gsuNDpbGBkgFgLtW7giYM448kXv8sZNFJD%2F05XcMaa%2B%2FFRuzxvbtzftlCVTrDE3caOgdmin0nuUntaQNAXQ3Sd4DtmMl6ORWuE0NbStFraMnW1rb%2F5IucVcK72VadEU&X-Amz-Signature=df84be81619d7f5620f5119c1c0e2da8e4fae4390865ef6533e0e9d832ce2e7a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:56:00'
index_img: /images/c34f92fd7edfbc072452166489949590.png
banner_img: /images/c34f92fd7edfbc072452166489949590.png
---

# 原因


最开始设计表的时候，没有设计好索引，等数据量多了，查询效率慢了，想再次简历索引。但 数据量巨大，一创建索引，数据库直接崩溃


# 解决一（慢 但是不影响系统使用

1. 设置mysql文件导出权限
2. 导出文件为txt文件
3. 创建一张临时表，与原来的表结构一样 `<font style="color:rgb(77, 77, 77);">create table text_assets like network_assets_blend</font>`
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


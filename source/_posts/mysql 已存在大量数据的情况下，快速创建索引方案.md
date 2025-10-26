---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WNBQD2C2%2F20251026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251026T010053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGU8QH3v5BaJ%2F254Yzaq0IPpT6zq3vUjW7kNROTMQXWuAiEAn1TYROhDkTxbS8xChfmQKH5Qa9ji7BUe9D24L%2BLOdOUq%2FwMIfxAAGgw2Mzc0MjMxODM4MDUiDDv%2FAJP7mOtZ5wm4ECrcA3Mzzk0wRP8j5A02GMI%2F6I0yzYwqHXmYnqS5yB1ajNyVTigPeOzTYs3GZB9eZyYeAiS3yow8dokPDXoNX%2FruVHNLskZ7SV5g0MxTPQ4umrnXUuot3IO9ZNqf9Zw8Uj3nUkTpFFjA3HCWdKCJSqVcvO07Eg1v3IiEaxQVR03kTukYO%2BWI5blAC9mglSZEF8hfDWk%2BK%2FjRCzyVaDAb5Kaxz3LbSSzLEACcU9wzK8fRVCUy66HuYOWwtbR9gQkz80fVALWdYbC3Pa%2BaMuDyL07mYcw39%2FZ3vJkuYNTerpM%2B3LzlTxBcJQ7a2BDOK7n6WMFhpFxVE4fKAtLsZ25WAkGe%2BKN2PA8%2FYfk3FkbPKPkIYemg%2Ft28Vwirk%2FJnBfNnuo%2B2FB3mrbTnJZJ6%2FQ9ETHi1wZXgf4WXkbafXNsPe8Nbyw342Km%2BHeQLFhqEbgz%2B0jRm0Yt0Qsqox5enVllMuZ5G3ZA%2FqVA2QJJXpUlyaRiFsntK2k%2BQT3Nrsu%2FPk2DSKBMrE9QyEjos4MQetbAaV8%2FWyKCEmb4IeKXiLK1VaNjYRNZiRv3yKbQq6DJDgVoh7YUwLz6I9OYeGCumVCQxX9fPyyl8GwLCynZtL7d28vk2JIlnOzDiEdBabMGia1lzMP2Q9ccGOqUBUEngZPa1bwoSCJKVbykWV0bKRIXdswYLhBwD0bNYXchObuhdwczWSaXuLg4pOcWaY%2Fnm68evZ%2BTIlEmlTGnKksX6jtFhAUuIiP%2BajqiKImd1I58zgD2NSLqq7xiIlrXUFC3tMI21pI3ux7ANePtw0ayJFzg8gBdCmYjDBa3G0TWIWUZyqtVjX%2FusMoyHUTpjVBMcowXhIvSyt290KNczqYZrWn9Y&X-Amz-Signature=3711f2b4908f31c1fd83d9ddd20f3db430884aafe988f3275d7f732f512f434a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


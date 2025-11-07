---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WDHKICWB%2F20251107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251107T020045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICZ3yudo29IHhijihO4z0wsD0RUPAMjDBSaMGP5%2B0SdtAiAt30YyjgZ%2BY5siewpEdeiKzf2y4XI8MN%2F%2B5lTvYMDGjSqIBAiz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMUw1%2FnclBC68%2BCwenKtwDVnFIR694SU39NCDdd9yRDPwuS709nC9RcThO2%2F4ncr5vUGEiVk%2B9DZQ4FzsOUZUzyCgTqW8iAxjm9ngcQXz%2BA1jpcOe3qh9x%2BcOTONeLBAuu1ngExl0%2BCfw1EnJ4IJ8pUhiUfNOMVgGsgelfIg4acpjoQgugKs7tvFkUF83UGsrdSRDGxUDtNjFfW%2BZiKcZ60RzfJc%2BanXlbYDVIe64FScP6ddxySwwBCmtozSRwQnzZ56GH0R5ivsRZVvPYaC%2FZI0PCjZHcXrWB%2Bp4zANkPzh%2Ff7F6x8p7FJbhyfIyT5vwlEr%2BjF9jLKtnn42uz87DW5rVfQvdbD%2FKuDO5%2FuRa7coH7mB2pToDsSB9WJuo2YjV%2FnWGVjAQE24DC9OTr8mxO7vXwD1Xia68EV9YXkJiIe4hx%2FNcXKZByxCOCPfaIQHJDzniTybBhZkvI42%2B6yWCk58yTdPdGBqAvWYmFCQCoUZxMlgYtTk1V15ZmV7xIS9UOeK%2FS2jglttQ6OCblxlT3G8T%2Ft%2F8y7%2BoPJ4R0WjvhAfrwjSDC%2B4RAReW13ZJGiOuAO%2Fc1rdvtumrTsVfgvBmKYdznRZ0poQ%2FTswRWrRC9E4d5KrWxcQ4EoMFVGJKRGjoF0yth9EuJ4gyCUeUwm6K1yAY6pgHnDYljwctp6OHA99Oo56QqCz8khAvnSnZKdmt0MBtefKKnfflq2cfUxcVGI0Rx%2BUf6fsbQ83%2BrjFbXtghceumnv5312DkpyOQLcEMxXgWI43f5mUxRtL45FrkUxHnIxgb5X6Uv4ctQ8mRxnA2W3JRaWTuye2zCuVfzw483ePkceCDdyhsV82RIRqIRSUDa8vCSg9ljUnV4z2xymSEyjScQ7DxpZt0B&X-Amz-Signature=938b587050e73e186202a16a94d6af7e7b83b7a04ee2f33583a3ff47226cee56&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


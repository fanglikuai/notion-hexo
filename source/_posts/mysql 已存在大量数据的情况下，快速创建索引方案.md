---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663J2XJNVH%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T030042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEPOjhz7%2FV%2FxtZFkWFYJ8Cy4DRuFz8bxJp4ChJH6NCVmAiAWu5rYpieoI3ILVVvxY%2Bgg%2FYKeATKo2x4aZyOFZ6b9lCr%2FAwg8EAAaDDYzNzQyMzE4MzgwNSIMCGCKzuDzeYEWFeGqKtwDiXe6KAMX75GwgKHqWHYceEnwJav4R1VY1nP6XSHThsM3izh21QoLilcx8DgL9%2BVdZmlxfdgVwN%2FUoTy%2B9WdZPbKXOVFkf6tmfTGOYA%2BJCUCq4Rql%2FZ%2BIwKvg42vE9y%2BKG8b41UECFAlx9w7bWaXF%2F7xov7s9vMwyn1BPvzqjDW%2F68x0o3dMFjD5mhUJ4sM88jzysl6xSypKo%2FvVtyK5izopNC9O19eehsZOIdHWM0mmCJNrueaHGK%2FtQkB%2BYUX5DTW4C4cw4uvtB40xURSsTeyUV0EK%2FoWbfC%2FTJXg7Y5fAYrFaiPm1RvCtr4MlXjsex3MLhNO8%2B99Ag02wLxY01HHqpNTGNdZMfQ4TOcNZkiyu4nKeD2%2F5wfqxZ1NjNu4uNS7OKh3JbBZisx78ridZkecuEgWHp8D6f3FMQ2a5hrsBHh0wUGoJh96%2FvNoatxAoutjmaQ0evNqut6UoNaW1a98%2FjFgrrbrIrzINQj7k8PWAZ8HZ15%2FmCIvIcwHADF8s5s4UlGlmp2XrzlQ79hWQDf7HILPIAlnL4LbmeocYq3VGDIGybAKdgZIn2emwvYrugYdCshANnh6mUPvnHwhVY4mmsiTv%2FsyvvJQUvnQC3gDjUDUt%2BYl%2BQx0cKYZ8wj5LIxgY6pgG1Qd5Wi%2BA1wVSqcdk2w4NmqkUaw97aKrMcbVDb9OnT3H6i1sdtDHFhsJNf7WqdfFdWbbUXk2bOdUBl0vPGQ6BMlxS6y3oVG%2FkB2tShk1n5oj4pTTX9aoXAwagXoRNa7ktmh9jl0BGzgtvSQlgqSkDjFia3nrzqzmtMe%2FqHBeEotyz9TXvY4PcQUoOHlMZtZi2QMH2ea0iTpeccnNNZXovsJIZ53z2h&X-Amz-Signature=7a594dba551bae73daec85d4afb849137a4336bfb2c220d2d2f5b782c6244ed3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


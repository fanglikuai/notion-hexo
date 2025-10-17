---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666STS3PTY%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T190051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAIaCXVzLXdlc3QtMiJGMEQCICoSp7U9vOS4zu%2FK46mrF2gERzZuEIhzAIbe4F%2FlINMKAiB4a8JKpes7AyENF3AYmC1td5aJjZiA4BdDBBbrXzLsOiqIBAir%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMzDV%2FruiSf1sq1CNTKtwDyXoMrT4xz0yNthHsZ1wZDfUPiPCmDAgCKGYKBmi1HcB1TtHBaAOTnCmW%2FF9zlFAneMVFz2C7TKz0%2BAH9wp%2F8QPGtXyNtNgYG41q3KyWTBB3d68ulGS43LRE3Iqp9Ks02YRTNzIfvNXMSkG8zGq8%2FDjxOXg3yMdrvdB89XD5hWRt4isws9Fv47BxjzXXGsAclANcjZcMWf7fpVlIGsKGG5PLJRcU1Jyda%2FMu3HJInz8TTiLRuQQ%2BxGy%2FixtDb%2FmBK6wXWbTarw4WDRzSxas7SE8v1DUGvB5hB7dU7cqRoP%2B0H0XnW0%2BddaQLtu3nB4ORTQR7wQRgJcW4OuVg5ce9evOWI3FknwfYf1%2Bt3hEGHEypt2XUBLRW%2BYH9ChCJstj6%2Fc2JusBVtVapI7BQjPtIkRzCLeFWFNbgw5iR%2FXcaHlz3FLj9nd9Ny2YJPhpG8hhokuasMLMJ4vDExoUF%2FcolE%2BdkcN1pA4BmTBrAFlpi611hWOW8tkz9mFj9SqdNpnwRAfPkbGHicK38mdNk7b5fCjM0Ro55DWtYNnOokzmoDINYkKny7%2FicrYarM8kLLPN%2FQMDcfhr2TG4K5sg6nD2TMl1te%2Bfw52%2BG2HtaVDfHYTlrqrQl%2FaXm%2Bl%2F%2B%2Fg5Ewuf3JxwY6pgGOfkz4MD6BNl5%2FBuedgOdDf0D9YmwDB3T7wNM6%2BDFMnIcFWK1i7M1FkLHFkol%2B1j4T54f9SU8Yr7gvbVT%2BgNgY243HaP7WkrjezjiyQrh2VSQmsBoOfeIdnMzmQYGyy4xMmvfdhTJ2hOwtzrVqnL7RS2yft%2BK5tjUcOs5KbGrwUUEyAWSu40op9JLfY31fdjhkGrhvsk8xVbQollLEe2HlbMldb8uE&X-Amz-Signature=d93f6bbdcb1135b3c5b9ecbc46117cec1348648edb2aa43e8105df9e41e36b45&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SXW4EM4X%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T110046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDWNcDPAAUy3sXWCUctkM0h56NHEIH2LCqm28r3C9MSuAiAPfJZJFNFt0wgEZ38w0QjL2TRn8MWxW8kW4D76tQL8lSr%2FAwhbEAAaDDYzNzQyMzE4MzgwNSIMHrqFdpj1W8%2FQpES5KtwDThj5Oy6w5KIQwT7zktTWVEoRYOSNXPiKJ1Ziwp4L8XOpAoezVgGiWeNqwEukNIo2FzuU9IQ23eWUhKZ8FuibrVDvaUZtXEfR%2FmKPU9Y7Y1R9j6XgWO4qTzD8Dd6X3AGijumAVpF6hPhChanAlIfU86KGEf%2FCixtfpXUstd%2FBd7%2Fcj3ze5HxK561pd4M2pIwXo40LnXYX9Glero%2FZNLDRkEaz%2F%2FZh0d%2B5lHcUgr0wZIs2DbboJkwqRU8uMhMhGPicguDe3ypgMQHIbc9JgLPQwbfLTrVz1O7zLYezItCLR5QhlcPksk8ky3vyM3OH0PIEDt7pm5Uik48FePyPURiI3xmjXaiZG4peIbUjVCXCl%2FqQIO2Vt4dl2cUBfAvM88qC5wyQulx1bYgDrHiUnHHfgnSZ%2BHdWr7v1071%2Bksg8xDfS3si5kYFwIgBz9uviB3ujYLIz3L%2Ff10V%2Fjr9jCvV3M7LVxjp%2BeCpG3KDrXXI16f9SkiIO9kTK0y%2Fr%2FDFOdv1LMHYfiJCVUE0bNGGPv3NevOHvbAlJf6paK8kMxFzjy7PaEsOmw0omlcCSvm7niSk9I0SUsJDPLuLC07xhcdEvX07hNtIt4AZKByexCJYh9Ddt0Hau7VL0uXAWBCown5rtxwY6pgGA6e6quDo56gpyGvFZpbX5X47f7Fz9DKHhWIol0K7PgHaREIRSM81M%2FuFf%2BBLuXxnSagMKGxAa9luoGeNXzclirCDt0HqTu8iOljwqDv5MV5LWUACr21tDwsAyKvqdpJRoEH8R2uko9XMp69kyblTeHjjFbLR3o7VYyCXcK0kdDDW5br9cdkYi59Ft9WaLVy1Z7sa%2FeXKnRVguoJYAOgXLXz9pr1BZ&X-Amz-Signature=7af565fe089959e58c069c0cb34c3c245e4dcff76f476a93fa255204fa61f251&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


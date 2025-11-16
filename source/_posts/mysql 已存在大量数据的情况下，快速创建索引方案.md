---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VRKQVSGT%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T080048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIHD9LzrOvtwcHQbZS%2BostJxLiRkTNpf3mIxA8VySOOU7AiEAgP4ywo7a8VTJ0lOwgEYbr2eLeZcfnb4FKbzmozyUeyIqiAQIkf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDK5TNSUCYhmeEKFkWSrcA%2BpzANLiYp%2B2hMa5SbNHHrn4AaG2PDFdkkDl54E90Cfb5oNaCmwGe4f6HsGsC9kK7KTg9t%2Fif2VA7U8LuSGoqN2xYsVKVZbIOPJ5zERwR3%2F9A01GJpeo1TIO5MxKgHl22WL6G%2FNsiFTdvs5xFBSTEzsKqjITwk2whI%2FkvNIYw3Qr97wqIu28EKF%2B81kOdgTG2BR%2FysVHU8eoGyCq%2BICoqFeNmRGzP3U3BYisVz9QLA1iWKTqZxuFDy5duAIVV8LJciUm5zTBbcSFDQqOcDFcCn2MmQyxexVo%2FOnfdKRE2ldjp8HpR28KARlugmaze37bvOh13Ow0gFtPF%2FOZYBFYC04vRnlNJWUQWPOkm3jZzQdGcMg5LLS00%2BoOanAI1LIsMqkm%2FnWuwj6AD9dJTrhq9HmWXYX0Mb3fSeBMCwHrYtFxOXJpFWAuNBNQtVdORB%2FCMVNK%2FMQdz%2FbYfE9q%2FbUcjZCaxG39%2BZzyaIlHsclex3gvlnJbgk1%2F3hVJoJZFxXWBGosb6c5ZNHC%2BitXwboBvhz1jVbVcWpkWp0dXQPzFYkc9thU%2BnCvVaKxfC2wqHzH21EVUd%2Fc3%2BpEn8fiWOFRa7HVxw5F4gF6UVlFZcaqgFuixqp5dHcZKxAErg52JMMP95cgGOqUBdmSweKo9JGsYjC7dlT9tfMg9F7MUGmRTfPBPAEse21F%2BovEh7zjSUkfmOevKLAD9zeJiYKLFbgCgL9HGGwEmartGe3IomeYq9EIQLH4tcIGuScKzOwQyOQMJ%2FJmzLFKKXjjT07sddjorIgIlE9rMsmmESkuBcvHafAwHkCpNcGl%2F6EyEMsDOAzvo3HUrWylx%2BSDXpOR2%2BpfaYga%2FHufQZv8pdNY8&X-Amz-Signature=7fc922c539b1136b0be283a0013bebfc54a32902f099b8939c2b25ba112da15a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


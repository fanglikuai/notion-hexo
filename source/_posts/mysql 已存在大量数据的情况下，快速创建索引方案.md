---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QHSINTRM%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T210040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHUaCXVzLXdlc3QtMiJGMEQCIGkWs1tADg7E%2BHJym6UzbvQd%2Bf1q%2B9OmEpem6NEYjNp9AiBkrtp430P99Q6NOVSOl7ATBn%2Fl03wgth8WMDNcBn1UZSr%2FAwg%2BEAAaDDYzNzQyMzE4MzgwNSIMApegjDmo6KuuH%2FcMKtwDc%2B4o7rfLe7Q1VJBzMc65X6gnxPYYCuKeG9u7MGleA09DaCPYTvZMNZMMckqz%2FMAi1O9Wb0qiOtsCUdf7%2BhVWhrN7AYZA1T%2BGJdlF6PUy7HxqCUkvIcGhiVa2pLsBrbifdo9BKL9rP074kiJqFuDDRjqX%2FHPBicysatSE6MAZ6i177gLkkFDxyDxKz79FmDE%2Fxp36EvehuC9RmkQc9gPftXas2YSZ35P0fKn5lCmurWA%2FaL5E%2F7TvKKDl9bzOq8FW15aUkq%2FgEyfULduwB68wjy9HmS%2BMKaNH0AnBWhyODP4SiT1QMYrawkZ%2BeWYKYctKklvVoQo4gTIUdprAVZMJvvj%2F2quroVpfE0JNfOo0D%2FKphhhoZjxwWh%2FyrJeFJL3L5XroJBgj6mVfZRYRliV3%2FV2MR3WfLAk0wlyGOwCJRsf1k%2FOt4fN1ruzgPxtVzSuUYJn2guhwMYxMxHnuTuSh%2FrrAPektnh7s1Q9klK7EHLAGlRQvRWHjvJ40GPLYBxBqDyNTOEL65ccItVuPDq8PBlSUCB3M2dEthd7Yyz6TQzYPZj2%2FZfkKW856%2BgBmEGpbQYyz1EqLv1lfzGFvLatkAUA2eqZ7Y0yzPFgEmUbIR8aLwI7D29mNgBCrd1owu%2BDTyAY6pgEl3BQ6dIm78TCr1yiLNq%2BLrCVps2LKjgmly80RBBzTeC%2FyCzMTe8A2GG9432%2F1rzrG9wv3M2UwOf8okNgiZJl1EEv9iwc1oPYOq2HBabmE9CPtrxnTwYXows2CaMlJBjaiwQQFREjSiaJlgD5u3V1T2mlECI1UU4OOweZQSgCCw5rs3vBL36sMyFeECUxR1dIrpOse8%2FbOQztcHUhzR3G5vqAVu1b9&X-Amz-Signature=0a2507bb73b6111b0bf62a7446e4986fdcf4732d858fd7c64f91d74f89449f1a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


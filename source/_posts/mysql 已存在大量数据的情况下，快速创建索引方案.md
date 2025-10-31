---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SG6DYVK4%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T080040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEgaCXVzLXdlc3QtMiJIMEYCIQCU0WKfi3v8KiLLndl5aodNJ0Q3r1z%2FFlNJ0e2jGBUn7AIhAKARVAI3u%2FwrzVhRWdTW3Zmf36hRtZO6Gk011V8xo3BNKv8DCBEQABoMNjM3NDIzMTgzODA1IgyE7I%2B1AOuHkb7LBbIq3ANJ%2Flb2kJs%2FSw3Gev6mtWc2fUKaa4VUEeSu5etAhE1%2BquYEc5tVIAPXu6hCIATUU17oZRmryJ1gvsyVBuxOZ8tzgGacqn4xsciaZoG8lZsdqP%2FAEN8Wm4Ym%2B2JK9ErzWndrmVtIuMrGCabsflZxL5K%2FjCbjyJAU3t%2Blc7Q71OEimOBw6lG77WoU%2FfCJtzCyhK8Gn20qgyM9YTbyMUlMX2sHcDPlzI04JpM4OIvyH1aLivqC%2F6Qc%2BAtKf3%2BRKnejvRht2etBedwa23gHmUxyepglbCl8srYwkHD%2FQuc2U0DJ%2FAW4G0n89%2FPz9CAe0jyfQJn7nkkRVlOidNYSqLWf06Ue9jPMe7FnKLzWCmbAwnDLjcY7WzjpqjT3eR75phV%2FgwF6roNhQFJl8TsIrZxa%2BTXc4tI4FDuVU6nrcHOY6sTeovOMXoooP0DhUkVKuPbWt5aFHp46%2BficF4r5gy96qi6O7ip4EmV5MPIsYdFStGsLFuAgap06QUUrauHZ18iRd4HN5WU8PmuoxpQ7sgZxBFEurCB61rthOgzzBF%2BFYc0tsnLqwWxQ3rC%2B7U%2BGtdDe%2FWq0HdgkDxGZY2APOd28hs6ZYYoysSki9ecgeOer1ytkhQ%2BJP%2FOG3GK2tKn59zD3x5HIBjqkAQeao9WJhrlNCrlmzmpBqExJLE1FHI5c6RWdPmh1AK3SDkv93f7DnFSbV4faxENSNfs8b52dgKXexgdMkBeKHq%2FXElrJpxwpXOKKSjfNS2SFbcyELxJNs47RRLSvsBKmzm%2Bb60gYzSp%2FeK5wwrlh5K3OO6WoPmffvjIYwHq3iSZn2d80%2BLJaF30gvOT3xdylv1s5DR3xS2LcJzyl%2B21PDlHE4TNA&X-Amz-Signature=6ba5f820a70334d41dd4c8fff244e528f4b9ee1f00a6ce2367f6118322f4f687&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


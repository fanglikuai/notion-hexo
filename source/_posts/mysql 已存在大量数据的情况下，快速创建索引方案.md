---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZPKUBOC7%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T080051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE8aCXVzLXdlc3QtMiJHMEUCIDOhbZ67Lb6pWFNE0QosUJB%2FZM2WMRKNGxRmuhPlFZm4AiEAgQ2T0u16bhIaLYxxVlC09h79CKGeAvj%2B%2FZrCzU0ByiUq%2FwMIGBAAGgw2Mzc0MjMxODM4MDUiDL5veLwVXU%2BKoWes1CrcA4H1IksJJyzzGkN%2F86KvJFx44AfCSwCINocRHeosahLbfc3MlPiCLoBNxrT2mG0w7mVnPbfwDlUje%2F%2FSfbXXsrZoX9c10so6w5VJqTQ8NiYmWozIZGbro715E4pdcmpbR2gk4MDhxZ8Oyn%2FFBppRoBFklSbqn%2BQvsW3EwK2tezEeJehDYTy574zRpYB1IgvKiBCx4eL93jSmDdN4y6Wjv0QMMVUGYU3N7OEJ553vs5BR2R7NQhzeZBdNWcTaX01MN42cGRCEaBoYvfX4SvDZMskvsX3IJla7H%2BT6U0dhyMZZ2KMZ3qvo7VbGv2T6MGW6QPAI2ekvIeHGT3BSeCWL5abUh04D57%2BfAGFrwdkrq43v5ObyjFO8OP58zB0lbN2kO%2B0pTt7h7kL6AomvoA%2BbCMP5TemsPgRFst3gjm%2Fdz7NERohaH6f1p1oKK0yEB4hFuhs1J%2FXiR1p1g0sZoe3He3GgiIDvB7Ynf%2BCCfMzBGcRELrMsVHQWrFDWlHidNCO7c8jYQjMh8ZM0pTzmkPhq9UCJDLKMR7DqtH1w1hosjXtKshsWDOqSeXmK8Op2Cb%2BSJT4E%2F8i4JkB3hs%2B9HeQ1tuRQ4aqWXyyNb2oovajR0%2FuAwcy3BxH%2FvqWkOhyGMLXBy8gGOqUBUnHhZpl9GTa3YXF3CJyrnZhJVdw6LxjpGHzAdNdo2LMNex067mfnZaoQMiQ0Wi4Be7b4jgV1%2Fw06O0tAPCHArcu%2BUevKBCik9xmKfOx9hoiwgcNJUK1T2NQhfN2h6keNFz6fq0iJoyjAIgclkHqURk2sP83r%2BDAehJbyI6QijYTcfhSvJf0gFm672hhUeIEPanpNZ2MBnYJAe9WGaor4B8pVCtXB&X-Amz-Signature=32dde4a0017ed0cb4930d9ca6b37bdb554ef8cc6ec0d9490e771ce9ee587cde1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


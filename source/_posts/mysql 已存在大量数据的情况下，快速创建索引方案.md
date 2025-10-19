---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XQXINEFQ%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T090047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECcaCXVzLXdlc3QtMiJHMEUCIQC1ExpW93gOgl28pcKLXqvZVv3A8zoahByuTZE%2BigMtIwIgdcpkuY0jj57FIZgPRhaPBak5lnncfQ4HOk3sKHZWny8qiAQI0P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDFHNcuB9TGEQTt2%2BNCrcAzGwA23CPLcOKnt9UtDdMDQjo4z6zLdnil5yrH3%2B2u1ndnypjsoTmJXRvlHT7eJ4T24S0Mqn9mjmtHTOlgis%2FALKN%2Ft5dV17GUDUKN07zQxJco%2BHHqSj5aGoJueCmgzBvjTCaD4w36KJJD%2FWl%2B%2Fyj%2F6cxGbvU1BbIBzcxZKgKwcxtgE%2BIg%2BM2I1q2D6KRpG2xWYC5gdTzsC9IdcY7yR2VSktsrHM44MfD2pV8SjCc7lHtMwT9zjMcD9iuSjplapAk0JoNXfx9paV08L5fEODN%2FEj0sKf796UALPdqtBAgNp%2FIZZb91zBjmLQXrCcu9Xb8qRq%2Fp0x8UT0Wox3AcTCjqqDkTYIpITIMMNiS6M2PkrLY2tvIYMlDWtLhuVUKIaw4Tn%2B5SwXztxgqdGlZcIOb79qW4vF8qVIMBWxg25HG3V3mjh72fmSi9tpSao508gZ3UNYOrgcSlUxiK1E67Nw9J8qgPPhjri5sFUpEof7%2F6MRbZ%2FgrsYOobZnmMmsCZe3JGf2%2BIS%2BWv0cgm7PLBk8V0PkxQB0xj9qrx3QxBmtLHSKQf24CUlwOv7MaE5JzKzLKY4MdZAv3xS%2BoDskI4WTcONKu5nBiwn9S6HF9cqmkWj%2FTlHmR0BjsL88w%2FFCMICK0scGOqUBCfj%2FCE5b%2F1NDk7Wrkq%2FWJeVvE1OgEZppM%2Ft4DJi13sixfit91GJCr%2ButkzOlZNKi2uhl%2FCv9s8McsajuSIXUqPxmDqA4EP1eyiHm0fJZ%2BuzJzIJnLU2a3CcSqTny6xNfVKdqOjyp9G25K8WzWRAd%2FaTof1%2BsqYljjIdgcIQd7rl0BciAQyubvavAANdU8F%2B9F8KEDnJVX28BHSikxfhtDpRcOhSR&X-Amz-Signature=4648700e1a12b7e61ecd2de5e6e1fdafc8ffd261a0fd36c65fbf7b13c0b0330f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


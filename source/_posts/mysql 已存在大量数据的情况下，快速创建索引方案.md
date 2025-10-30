---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663DUPKNLD%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T040056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECwaCXVzLXdlc3QtMiJIMEYCIQCGHnaD6tXpxPBctFEFA9H3EKpalcofdyKKoLEnZmCzCQIhAO%2F685uAuHHBzY8CWoiS8ao9mbGqr2Q%2BUolOKNjZOaoMKogECOT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igz8RSiGpZQaAOcWLqkq3ANPp41uob71vCJZsV5dFW2Gfv%2FQ1bASCUmFw5hcJ5NAYFtQRusr12fnn1ZPvNm53TCJ%2B4bYc0JXVM7Ut3s6a4VhxL0VimGvZmE7pMw3wp80GZikFvtmjVQVC9riFrWDaVn6mG2g7mEOGWYj4x7uYPcBkA8irAJXRcu58DF6xxHaGmLtwLYV2XT%2BK0xzEi3Ej34B%2FhJ%2BZ4l%2FbeM09iT9KRFo1Gijr96A8rHLhpbOkAnOQQrZTDwmPmuZMrrcyfP2d02TxbV6IZus5Fb3V0sVCmqv6knkgG9sL93y%2FwCK9jlegHYWLjc%2Bz0rgmXkR%2B%2FU9QCUrWSlOmWsp13zyioNiYpc1qP7vd9qZO2wjVRowmSYtYpcBtIDNInUaWsnf%2Brq2x%2BcbLpB8enHzQUGHULQjJRwH1mTomxcSCYkXVgxXVfmVPXoeuGdG8Td3%2BPzci%2FGLG2s7hCzpL9BmfY7skEgD7aFjx4hHuBxgoLGJlfCp%2Fk2rWCk0rI52JDnJg60nmhAMDKqKz0%2BnbeCPrpFHfmdKiDxGiN2ZJH1HT9YXV5CME4GbIqbTAlS0%2FWuCFVli7VFvQnwwtpIQhtRd0lY09Jozi0%2BBYLgAlC5TystnLupKFv3Wj15S%2BcFKmXj0jmeEJDCms4vIBjqkAQZwK%2FggCNnBgCY7RFzEq3kqJM5%2BrhC45H0DE4h4dtY9sOi6%2BBmZW9c7o1ksuIU%2FRtth%2F2mu8nmGu5tzl6u%2FsJNlSSB5haowl75jPMq%2F%2BXhXhFCvKM2oSvH4qvnXcnOP3aRFeVD1hljrXLBVMJ1%2F5JufwrnYUyI%2Fz41yDQCsn2wq0snH1D7Y6MtaqQi6B5fgUJ%2Beqm%2FLGgEqh55RT2IaXoYIv9%2Bx&X-Amz-Signature=4a938f824fbab7a690e777816c288c2f06629aee09a57c07d67bbe54cf8b7163&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


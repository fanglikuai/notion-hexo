---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QWDOTGXQ%2F20250918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250918T090048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED4aCXVzLXdlc3QtMiJHMEUCIQCSbWYY7LC5vE%2FKtNTvNbOpP4qyRCPryRlEFDZehXmsAAIgUT5EEwIDAdBDkF4Yy8CVoj00h7R3pp6lgQMAciN%2BYgwqiAQIt%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAQk0EEI0O9sOPBNdyrcA0sgdO%2FNfANd95dAKpHPeNBnZx6C0Hob9N%2Fp%2FYexJxTo5MUhjBA%2BsmVduswCo9DWZFkHlkO1HRvvZs2OZIS6nfJt4q%2FOz6yTPDxi6pCvE6nhrOQEigl9YYv34YoIQQbg7J3Bw9NNG0%2BeopLH9byVdqKu4jSI%2Flg7xuL1YMvI59PsoMwTfhKmLrrp815M7Q6kH3Il5A9DD53rx4kytzzztTLwv%2B5etXoV9WS9t%2ByjXZFw6bX46Vzv%2B3LMX8lfxc0mouihyANPiVqAEXWNvQxpFwj7b%2Bh7Ufg92yhDZ%2FRmGP9%2FMTYrHCWo945VwGDhHL9uVr0Jeb7Gc%2F8Jt3bMZNaWVHGMiY2LXP7bpWBVZlTdhqf7hheDfg65TJUjgByYRAy93kiDNZGYIFJelo1BzLVbWF75bruOdegt%2Bwx4X81CRN8ieK65Clj3yoh6i%2BxUnsB0v9vZbYbxPkqqaHIAdlZUQH08SR%2FkGNpkDQ8BIvqc5srdm0lxjTkItwFFpsho6JP5lB1X3WmBe64SDHtVLEdpBOQIPxbuM5Nr6GsgCNCo0Dki%2FleCEOxIIFMPgG3Ccr8Xj1I6fODf4ulnbNen8bpN5SIW4vUJogCQslD2vaDRPigsTs95K9W7oBsFnozTMLi9rsYGOqUB8dVOSdhDzSrn9p87CsOoQ5WsQ5kQp10PsZ%2FTsi2Fq%2FNMyFphVmJfhv2rNwJ4RhxkjeQbChVAWLmzNYEE8aqLgW19T2whLnpWy2nW%2FFnbIzhIuRIIj33Bl4tGlH%2FBGdXzODy59H7bymeetCmeWHaYud%2FAPCmfGqWBzsZb4TKld6ZAJ10Y%2Bhz00KIGUtz0kVWj3p4NwcpNfoAESTYmLyTpK5GkpDh2&X-Amz-Signature=d137bf6b1bf9f8c5f94a482eeafbd4db52064199acf0d6b3fa3bb23098a5cd85&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666E7XIEOA%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T150047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEH8aCXVzLXdlc3QtMiJHMEUCICTVI2dMLU4f%2BYJtweOchyGEQKXIwy%2FiZ8VrbwLV5qc%2BAiEA6d7nNJ4ZZ562XViN8nc4nCUtR2PPp%2B%2Fr%2FH1izc3K014q%2FwMISBAAGgw2Mzc0MjMxODM4MDUiDAO4IpNljg6%2BwEONHSrcA%2FWskkJ%2F1bFPdHNGfMu0RGNOyeRGcN9GxUHudg4uUNDjQrlDp%2FPqR5jFvpdDfyVAeB1xmS4QfTf3ekb7bsgMvyAo5G5yYN7TGmQqw9rj%2FDmo0rTv8xmoG2b53%2F5MyhGr7cUfVuGnpL3i62k2F9puGgIGYvYkLlesKu%2FkZthH2f00Sus8%2BHhd6zViTibjQjx%2FFH%2B8iIMgWOQgLhFgPsAcbliPI2GUuyi0zxE1uMMHII6bwCTjM9IdmHw0PCpUGIiGUXYnAiqLTpq5dAJJUCidhObKv6cqTLeQv9H2Mn00amkmWhvTbKGkK8ENWPRBBmS8yikTIQx96lEp4u1vL7infm8NOJLH4HNdBKbg4k6I0RecT5llVFg%2BE2e2Qz8c%2Fx86GQgMLEJcSi0Ev%2FZZp2CvkWeNqLile6yZKuYgEV%2BtJRwt3Hexip7XTPIYoR8A4M%2B8QhPlX8QAgkWAuV1Uw%2FudTeTvzMl%2BEM8NlWDeqwSt0X9%2BnCcuXP7QP%2BYDTUVGoAwuPnAI%2BkVR0JsITpGD8a7NNgu7XW97xSO%2FF6z0sPeHtt3KtCknnltJT%2FAWLWRLsrrzBCjPnxYEi58DyQ3DKMFTy5AvG4IV7h1aJurN5Phvgki8of2dL2T82fl5ejqpMPPincgGOqUB16GHPCzfz1Do212cZuldjICxhT5%2FqJ1aVq90cR%2BPRdNrwrFRDioFfY2Y%2FSfebqApjXckVsYHfkBBwY7AjIWMfqJ2I%2BUb37WT0ko2UbZlDWVl40KbzjKWQPhKyjfYiypb2Oh9aGmLzVZcfkJw%2B7cMkFhCuiq38%2FohoPdtKAOZBqFXCrbDNVbiCTV9JdX%2FP82QSxImdMWbGtSs4fO%2F8YoQMN2Gmxut&X-Amz-Signature=bba858734d8b8d847618b717674af373eb3ccf6d8eacd385a3525f1f4b4a018f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


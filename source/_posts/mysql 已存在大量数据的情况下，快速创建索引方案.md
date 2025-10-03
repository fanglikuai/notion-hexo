---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46633NQSE4D%2F20251003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251003T110044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAc1SXEt52kyj0%2BZS%2BX0xXh%2Fp6EZzQQur4Zdnfqmjfg3AiBPOIFTzbU7sxWMhOoHaIm3jS62LyVXEo67ODGroLYLBir%2FAwhEEAAaDDYzNzQyMzE4MzgwNSIMeIRzVUrjAp3lyNERKtwDfMrOx9cZVgoBjg7h7dCmia9GIhvHzF5xv%2FSZouI1uml8OrnMfGmY7PnBAUZDry%2B9ioo4n7oX%2F5L%2Fe0PaZaFW0IGx4sJwa%2FZwrvvyK%2F9pJDGMEMcqV24nmAg9WtEqkc6vb%2FLoleAQRS8fTlxsdsdfV%2Bkz1PicrdsIP4JXEW%2BBcl9w6oj6ysFKIdstCWvWDtK07SX%2BrshFuh9Hl70Ev%2FYt8lIl%2FpLFlKlH6rHzpv%2FrD4QxgGar5L8XCWdZvwsuJl1Iu%2BAk1EdWQdFjimXZ3ZPvhMDgcAdvqdXklnVHEDz%2BVOAweFqylqS0950Y68W3hib0Xtg90r3NlUjR4kxGsXUJSyTZBhTofRpemo43sgZou9pRZTPYfGnlGeDlwZiK9qA3gLOnkMVzmpjFj2GzuMWth0iMVwJSH%2BlmsP%2BcnvplKd8fIP2K1pO%2BFcOF0cyF8vDm6WQNc7jM1prhLzROEzKFQ0PDgFiMZOkwH5AF3rPwEYg6An%2BUUHp55NsmR8yP87jf6%2F34reeVSfQXIpAKNa56tDUWByj5g%2FBrbEmwoZpiLO%2FMXJ1ZFrfsvfc2ou5Mhvm7tCvps%2FWQuvRxfmX0JX0I0%2FGrgJ0bCrR%2BTdkNl5EkSTAnEo1BdhL698ks4TQwhc%2F%2BxgY6pgHTtWBW2eGbi2%2Fbsvm6MCMu28x5h81ur79XeIxO1m8uhxvpsun5nLk4%2F1EDgMvzBnF8OZMdpBbVlmGqhtquxFovIkuhlHATLpA0DRA0su%2FOMIqthFQq72ImVA9DTD%2B9xPTFhxnkTEKmUXE92zRcXsbJ1lJ%2FEMCTQ3rJB6yCAr%2FFr2wwttnmQZl6cjan%2Bk09q%2BmNQx7aeNjhBeS%2Fg%2B4J2dPS3R4vde5K&X-Amz-Signature=22448da98bf29eecdef5af6af8482e658826531f5841918fdf64a8a2e9d91aad&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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


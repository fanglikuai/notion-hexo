---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TE6VJYQH%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T140305Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEP3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIE0T1I0FQJy6dnjZGKibgEfu1wKWDFy9FpiEdfRkmPiCAiEA%2B4SBXM5n%2F1JiPpsOfLVhJcCI%2FLp%2FgX4oexzGMRqdTo8qiAQIpv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDMvJUY64NzXJI4HM1SrcA6D9LgmWmOBD4pwlWNYETjcWqXas5VEl6cr5jQ2jlQhwCvablUuPfpq8j%2Fjm225bi2YLwFjK7EiuAVFhvUPZDg43%2FAFA1z9zFPDdAtrvM5C5xxwuxkD1e9b9u6XbaDh%2B1nF44brIhnLmFEsEYp5kZys2KgLX4%2BN9RyUsJNsJf5A3iIuix3aDytf4rMzB7BJiKSSojKxYvGCrrujrFow14hmM4ASc5w8jQz%2F4hwtisp%2B3uzGyuhDM2%2BUWZX9dp0TjXHCBbU%2BYbX0asrDv7ioxZqRYQZdcvCRLLHYrtWyRudbWMDp0429T8cMxdtAasfexJG9q56xX4CXOgC6xWK6gVGwu%2FnDzJIiHsji4pDwBH6fPtHSeCw7TyEynr5Z4ypM59x2YNsKPZt2%2B8uD99mknUf7mxTNCMFdnqRWFdass%2BNgx8wegcbtpgpq8V3EHvRDS17a0Q0LvaWuwoSNujO%2BRDGqGPdECshr3hxU3rNyhUDtaE6ehmXqaB8YM43G4YaeMlcZivKzIMhgQh6s2HjrRs90oeofwxqnkw%2FpHbbCnlnCJ0QPUNt7f4y2kLMS5Yz%2BncYAQ7j%2Bodqvc3hovIa8bdvYD4me8hfP6DUDtM%2Fv24MutR1o0yh%2Fp8uegMEYwMNSByccGOqUBQiYzV%2BKeROGnRgryHIoOHlZ4S%2Fj25J5UJVtORX1sAuW1UYp4q%2F8bKY8nnpqF66OvNgRXvXJdyczo9GZ8VmS1MugVkK3NhHLau9Wa9X048koTfWt%2BNftkKQNcVEWq9wOB22xFElWlvAo3AkGla2Bq4sHPnrz9Sh%2BvwfiXGcxC8H1kpzG9HcC4Q1CVAgARmGwEiUqtZWAieV6XVGwdmoF0ywf4HnC1&X-Amz-Signature=e40f6fbada4356cbfcb04fd96b6c085c2f70d2ece0b239725f4db339078dacea&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

